---
title: "Unencrypted DNS"
date: 2025-11-16 00:00:00 +0530
categories: [Network]
tags: [Network, Linux]
author: aku
---

## DNS?
![DNS Resolution Diagram](/assets/img/unencrypted-dns/dns_resolution.png)
<p align="center">A recursive DNS resolver</p>
<p align="justify">DNS is basically the phonebook of the internet. Instead of you remembering long IP addresses, your device asks a DNS server, “Hey, what’s the IP of this website?” and gets the answer back.

Before it even sends anything out, your device actually checks its own DNS cache first. If it has seen the domain recently, it just grabs the old entry and uses it. If not, the request goes out to different DNS servers (root → TLD → authoritative) until the final IP is found.

It’s a pretty simple process in practice, but a lot happens behind the scenes. The diagram above is more or less how the whole chain works.</p>

---

## Whats my DNS?
<p align="justify">DNS servers are usually configured automatically by the router. When a user connects to a WiFi network, the router assigns the client an IP address through DHCP (Dynamic Host Configuration Protocol), unless the device is manually configured. Along with the IP address, DHCP also provides other network configuration details such as the default gateway and the DNS nameservers. This means the client can immediately start resolving domain names using the DNS servers specified by the router. In most home and office networks, the router itself forwards DNS queries to the DNS servers configured by the Internet Service Provider (ISP). Because ISPs typically preconfigure these DNS settings on the router, their DNS infrastructure is used for domain name resolution by default, giving them control over how your DNS queries are handled.</p>
![DHCP](/assets/img/unencrypted-dns/dhcp.jpeg)
<p align="center">IP Settings configured to DHCP</p>
---

## Problems
<p align="justify">
Since DNS servers are often maintained by your local ISP, they essentially determine how your DNS requests are handled. Traditional DNS typically uses UDP port 53 (User Datagram Protocol), and most of these requests are sent in an unencrypted form. Because the traffic is unencrypted, it becomes vulnerable to sniffing attacks. Anyone with basic networking knowledge can open a tool like Wireshark and easily capture and inspect DNS queries as they pass through the network.

This risk isn’t limited to ISPs or large network operators anyone connected to the same local network can potentially sniff DNS traffic as well. For example, in public spaces such as coffee shops, hotels, or coworking spaces, users share the same subnet on open or semi open Wi Fi networks. An attacker sitting on the same network can capture unencrypted DNS queries and see which websites nearby users are trying to visit.

These queries are in plain text, meaning they can reveal the websites you are trying to access, and automated logging systems can store this information to build a profile of your browsing habits. While DNS is not the only source of such leakage every packet still contains the destination IP address unencrypted DNS forms a baseline where a user’s privacy can be compromised.</p>
![Wireshark](/assets/img/unencrypted-dns/wireshark.png)
<p align="center">Wireshark showing my unencrypted DNS query</p>
![Cloudflare](/assets/img/unencrypted-dns/cloudflare.png)
<p align="center">Cloudflare Report</p>
   

## How to check if my DNS is leaking?
<p align="justify">
You can run a DNS leak test while connected to your current network. A DNS leak basically means your device is still talking to your ISP’s DNS server even though you’ve set something else, like Cloudflare or AdGuard. So you think you’re using encrypted DNS, but in reality your queries are still going out in the open.

Doing a DNS leak test quickly shows who’s actually resolving your domains. If you see servers you don’t recognize, something in your setup is leaking. Take a look at these cool websites
<ol>
  <li><a href="https://dnsleaktest.com" target="_blank">DNSLeakTest.com</a>: Standard & extended leak tests</li>
  <li><a href="https://browserleaks.com/dns" target="_blank">BrowserLeaks DNS</a>: Detailed DNS resolver inspection</li>
  <li><a href="https://ipleak.net" target="_blank">IPLEAK.net</a>: DNS, WebRTC, and IP leak checks</li>
  <li><a href="https://1.1.1.1/help" target="_blank">Cloudflare DNS Checker (1.1.1.1/help)</a>: Confirms DoH/DoT status</li>
  <li><a href="https://www.dnscheck.tools" target="_blank">DNSCheck.tools</a>: Quick DNS resolver  identification</li>
</ol>
</p>
---

## Solutions
<p align="justify">
A few solutions to counter this issue are using DoT (DNS over TLS) or DoH (DNS over HTTPS), both of which encrypt DNS queries to prevent anyone on the network whether an ISP, a spoofy actor on the network, or someone sniffing traffic on a public WiFi subnet from easily viewing or tampering with them. Once you decide to use encrypted DNS, you then need to choose a trusted DNS provider.

Personally, IMO I use Cloudflare. I got my Cloudflare configured on my Android phones and my laptops. Cloudflare’s DNS resolvers are publicly accessible and widely used for IP resolution, making them a popular choice for users looking to improve both privacy and performance. I simply can't strightaway trust any kind of XYZ network I am connected too. Even my college has got the same issue of strightaway sending DNS requests in plain text. I could litrally see those on my wireshark. The best thing about it is, it has got multiple VLANS and subnets and when a user gets connected to the system, it just allocates a random subnet.

Another option some users prefer is using DNS resolvers that provide built in ad and tracker blocking. Services like AdGuard DNS, NextDNS, or control list based Pi hole setups can filter out domains associated with ads, trackers, malware, and telemetry before your device ever connects to them.

These DNS providers maintain large blocklists of advertising and tracking domains. When your device attempts to resolve one of these domains, the DNS server simply refuses to return a valid IP address (or returns a non routable one), effectively preventing the ad or tracker from loading. This approach is system wide, meaning it works for all apps and browsers on the device without needing browser extensions.

Ad blocking DNS doesn't replace encrypted DNS; in fact, many of these services support DoH, DoT, and DNSCrypt, allowing you to combine privacy, security, and ad filtering in one setup.

<ol>
    <li>
      <strong><u>Android Private DNS</u></strong>
      <ol>
        <li>Open <code><strong>Settings</strong> &gt; <strong>Network &amp; internet</strong></code></li>
        <li>Tap <code>Advanced &gt; Private DNS</code></li>
        <li>Select <code>Private DNS provider hostname</code></li>
        <li>Enter <strong><code>one.one.one.one</code></strong> and Save</li>
      </ol>
    </li>
    <li>
      <strong><u>WiFi network (unencrypted)</u></strong>
      <ol>
        <li>Open <code><strong>Settings</strong> &gt; <strong>Network &amp; internet</strong> &gt; WiFi</code></li>
        <li>Long press your WiFi network &gt; <code> Modify network</code></li>
        <li>Advanced options &gt; IP settings: set to <code>Static</code> (or edit DNS fields)</li>
        <li>Set <strong>DNS 1:</strong> <code> 1.1.1.1</code>, <strong>DNS 2:</strong> <code>1.0.0.1</code> &gt; Save</li>
      </ol>
    </li>
    <li>
    <strong><u>Linux</u></strong>
    <ol>
        <li>
        <u>Add Cloudflare GPG key:</u><br>
        <code>curl -fsSL https://pkg.cloudflare.com/cloudflare-main.gpg | sudo tee /usr/share/keyrings/cloudflare-main.gpg &gt;/dev/null</code>
        </li>

        <li>
        <u>Add Cloudflared APT repository (Debian bookworm):</u><br>
        <code>echo "deb [signed-by=/usr/share/keyrings/cloudflare-main.gpg] https://pkg.cloudflare.com/cloudflared bookworm main" | sudo tee /etc/apt/sources.list.d/cloudflared.list</code>
        </li>

        <li>
        <u>Update and install Cloudflared:</u><br>
        <code>sudo apt update</code><br>
        <code>sudo apt install cloudflared</code>
        </li>

        <li>
        <u>Create systemd service:</u><br>
        <code>
    sudo tee /etc/systemd/system/cloudflared.service &gt;/dev/null &lt;&lt;EOF
    [Unit]
    Description=cloudflared DNS over HTTPS proxy
    After=network.target

    [Service]
    ExecStart=/usr/bin/cloudflared proxy-dns
    Restart=on-failure

    [Install]
    WantedBy=multi-user.target
    EOF
        </code>
        </li>

        <li>
        <u>Reload systemd and enable service:</u><br>
        <code>sudo systemctl daemon-reload</code><br>
        <code>sudo systemctl enable --now cloudflared</code>
        </li>

        <li>
        <u>Set resolver to use DoH:</u><br>
        <code>sudo nano /etc/resolv.conf</code><br>
        Add:<br>
        <code>nameserver 127.0.0.1</code>
        </li>
    </ol>
    </li>
    <li>
  <strong><u>Windows (DoH built-in)</u></strong>
  <ol>
    <li>Open <code><strong>Settings</strong> &gt; <strong>Network &amp; Internet</strong></code></li>
    <li>Click <code><strong>Properties</strong></code> on your active network (WiFi or Ethernet)</li>
    <li>Scroll down and click <code><strong>Edit</strong></code> under “DNS server assignment”</li>
    <li>Change the dropdown to <code><strong>Manual</strong></code></li>
    <li>Enable <code>IPv4</code></li>
    <li>Set:
      <br><strong>Preferred DNS:</strong> <code>1.1.1.1</code>
      <br><strong>Alternate DNS:</strong> <code>1.0.0.1</code>
    </li>
    <li>Under “DNS over HTTPS”, set both entries to <strong>On</strong></li>
    <li>Click <code><strong>Save</strong></code></li>
  </ol>
</li>

<li>
  <strong><u>macOS (using encrypted DNS profiles)</u></strong>
  <ol>
    <li>Download Cloudflare’s DoH profile:<br>
      <a href="https://1.1.1.1/dns/" target="_blank">https://1.1.1.1/dns/</a>
    </li>
    <li>Under “DNS over HTTPS”, download the <strong>Apple Profile</strong></li>
    <li>Open the downloaded <code><strong>.mobileconfig</strong></code> file</li>
    <li>macOS will open it in <strong>System Settings &gt; Profiles</strong></li>
    <li>Click <code><strong>Install</strong></code></li>
    <li>Confirm again if prompted</li>
    <li>After installation, your Mac will route DNS through Cloudflare DoH system wide</li>
  </ol>
</li>
  </ol>
</p>
---

## Conclusion
<p align="justify">
Securing DNS isn’t some magic “fix everything” button, but it is one of the easiest things you can do to stop your ISP or anyone on the same network from seeing every site you try to visit.

Whether you go with Cloudflare, AdGuard, NextDNS, or run something yourself, using encrypted DNS (DoH/DoT) at least makes sure your requests aren’t flying around in plain text.

Sure, encrypted DNS might add a tiny bit of delay sometimes, but honestly it’s a small price to pay for not giving your entire browsing history away for free.
</p>
