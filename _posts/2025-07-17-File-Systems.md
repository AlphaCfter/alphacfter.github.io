---
title: "Decoding File Systems"
date: 2025-07-17 00:00:00 +0530
categories: [Linux]
tags: [Linux, Unix]
author: aku
---

![Files](/assets/img/filesystems/Canva-Blog-banner.png)

---

## Filesystems??
<ul align="justify">
  <li>
    How would you feel if your files stored in your drive would look like the image above? Unorganised and untidy mostly like our book shelfs?😅
  </li>
  <li>
    Ever wondered how your data is stored under a computer or your Smartphone? Even a USB drive with billions of files which could be a document, image, audio or an application knows which files to retrieve and store 💾 
  </li>
  <li>
    Of course the operating system like Windows or a Unix-based OS does it for you, but some file systems also provide extra features you could use explicitly to manage your files 🗂️
    </li>
    <li>
    Every piece of data being stored on your physical drive with any number of partitions is well organized using a file system. Let’s put a new perspective about these types of file systems being used across operating systems 💽
  </li>
</ul>

<p align="justify">
All file systems are not the same. There exists different file systems which are classified based on security, speed or compatibility. It ranges from a simple 
<a href="https://en.wikipedia.org/wiki/File_Allocation_Table">FAT</a> (File Allocation Table) to the most advanced file systems like 
<a href="https://wiki.archlinux.org/title/ZFS">ZFS</a> (Zettabyte File System).
</p>

<p align="justify">
Let’s <strong>assume a power failure happens</strong> just before you were modifying a file, the file system logs the change (in the journal) before modifying the actual file. This ensures that if something goes wrong, the system knows what was supposed to happen. After the power restores, the <strong>system checks the journal, finds the incomplete operation, and re-applies the necessary changes</strong> to restore the file to its proper state. This is known as <strong>Journaling</strong> and is used by modern file systems.
</p>

---
## NTFS (New Technology File System)
<p align="justify">
  <ul>
    <li>
      🖥️ <a href="https://learn.microsoft.com/en-us/windows-server/storage/file-server/ntfs-overview">NTFS</a> is the <strong>primary file system used by the Windows operating system</strong>.  
      It is <strong>deeply integrated into the Windows ecosystem</strong>.
    </li>
    <li>
      📝 NTFS uses journaling to track changes.  
      However, <strong>its journaling is limited compared to other file systems</strong> because it stores only metadata, not the actual content.
    </li>
    <li>
      📉 Over time, NTFS can lead to <strong>fragmentation</strong>.  
      This affects performance and is a known drawback of the system.
    </li>
    <li>
      💾 NTFS is the default format for most USB drives and partitions used by Windows users.  
      <strong>Your USB drive is probably using NTFS if you're a Windows user</strong>.
    </li>
  </ul>
</p>


---
## FAT (File Allocation Table) (FAT12, FAT16, FAT32)
<p align="justify">
  <ul>
    <li>
      💿 Back in the days of floppy disks and small memory cards, <strong>Microsoft introduced the <a href="https://en.wikipedia.org/wiki/File_Allocation_Table">FAT</a> file system</strong>.  
      FAT12 could manage up to 32MB of storage, while FAT16 extended that to 4GB.
    </li>
    <li>
      📈 Due to the limitations of FAT16, <strong>FAT32 was developed</strong> to support larger capacities.  
      FAT32 can handle up to 2TB of data, making it suitable for more modern storage needs.
    </li>
    <li>
      🌍 FAT32 is almost universally supported.  
      <strong>It’s often used for boot partitions or the EFI (Extensible Firmware Interface) system partition</strong> across many devices.
    </li>
    <li>
      ⚠️ FAT32 does <strong>not support journaling</strong>.  
      In the event of a power failure or disk error, the file system cannot recover lost data since it doesn’t keep track of changes.
    </li>
  </ul>
</p>



---
## Ext4 (Fourth Extended File System)
<div align="justify">
  <ul>
    <li>
      🐧 <a href="https://wiki.archlinux.org/title/Ext4">Ext4</a> is the most widely used file system across Linux distributions.  
      Even <strong>Android has used Ext4</strong> at one point due to its reliability and support for up to 1 EB of data.
    </li>
    <li>
      🚀 Ext4 is <strong>fast and performance-oriented</strong>.  
      It also offers <strong>backward compatibility</strong> with Ext3 and Ext2 — allowing older USB drives or partitions to be mounted easily.
    </li>
    <li>
      🔒 Ext4 <strong>lacks built-in encryption features</strong>.  
      Most users rely on <strong>LUKS (Linux Unified Key Setup)</strong> for encryption instead.
    </li>
    <li>
      ⚠️ Ext4 does not guarantee data integrity after a power outage or disk failure.  
      It may result in data corruption unless combined with proper journaling or redundancy systems.
    </li>
  </ul>
</div>

---
## APFS and HFS+ file systems
<div align="justify">
  <ul>
    <li>
      🍏 <a href="https://support.apple.com/en-in/guide/disk-utility/dsku19ed921c/mac">APFS</a> (Apple File System) and  
      <a href="https://apple.fandom.com/wiki/Hierarchical_File_System">HFS</a> (Hierarchical File System) are used in the Apple ecosystem.  
      HFS+ is common on older macOS versions, while APFS is used on modern macOS and iOS devices.
    </li>
    <li>
      💽 APFS supports up to 8 exabytes and offers journaling for data protection.  
      It was <strong>initially designed for SSDs only</strong>, as most Apple devices use SSDs by default.
    </li>
    <li>
      🔐 APFS supports <strong>full disk encryption and snapshotting</strong> for secure and quick backups.  
      These features are ideal for privacy-focused users and system rollbacks.
    </li>
    <li>
      ⚠️ APFS <strong>suffers from fragmentation</strong>, meaning files may be split into pieces and scattered across the drive.  
      This can reduce performance over time.
    </li>
    <li>
      🧹 HFS+ solves fragmentation with <strong>auto-defragmentation</strong> and also supports snapshots.  
      However, these file systems are <strong>limited to Apple devices</strong> — USB drives formatted in APFS/HFS+ need 3rd-party tools to be used on other OSes.
    </li>
  </ul>
</div>

---
## BTRFS (B – tree File system or Butter FS)
<div align="justify">
  <ul>
    <li>
      🔬 <a href="https://wiki.archlinux.org/title/Btrfs">Btrfs</a> is an advanced, <strong>experimental file system.</strong>  
      It's widely used in <strong>unstable or bleeding-edge Linux systems</strong> like Arch Linux.
    </li>
    <li>
      📷 Btrfs supports <strong>snapshots</strong>, allowing you to capture the state of your disk like a screenshot.  
      🖼️ In case of data loss, you can restore from a previous snapshot and create new ones anytime.
    </li>
    <li>
      🛠️ It supports multiple <strong>RAID levels (0, 1, 10, 5, and 6)</strong>.  
      It uses silent checksums to detect and fix errors, acting like a <strong>self-healing file system</strong>.
    </li>
    <li>
      🧩 Btrfs allows <strong>live resizing of partitions</strong> even when mounted.  
      Originally developed by Oracle, it’s now maintained by the open-source community.
    </li>
  </ul>
</div>


---
## ZFS (Z File system)
<div align="justify">
  <ul>
    <li>
      💡 <a href="https://wiki.archlinux.org/title/ZFS">ZFS</a>, originally developed by Sun Microsystems (now part of Oracle), is a <strong>high-performance and high-capacity file system</strong>.  
      It is known for its advanced features and reliability.
    </li>
    <li>
      🏢 ZFS is widely adopted in <strong>data centers, enterprise setups, and large-scale storage systems</strong>.  
      This is due to its powerful data integrity and management features.
    </li>
    <li>
      🐧 While not natively available on many systems like Windows or macOS,  
      it is <strong>commonly used in Linux and FreeBSD environments</strong>.
    </li>
    <li>
      ⚠️ <strong>Linus Torvalds recommends against using ZFS</strong> due to licensing issues.  
      However, <strong>OpenZFS</strong>, an open-source version, is available and can be used without licensing concerns.
    </li>
  </ul>
</div>


---
## Does it matter?
<p align="justify">
Umm, I would say yes if you are a CS grad or someone who wants more options. 
  <strong>Windows, the most popular operating systems holding a market share of 72% (2024).</strong> 
  They dominate the market forcing users to use their file systems. 
  <strong>You’re probably locked within NTFS and extFAT without even realizing it.</strong> 
  <strong>Microsoft has kept this ecosystem closed for decades</strong> while other operating systems have moved ahead with powerful and efficient systems. 
  <strong>NTFS is like the old-school file system unlike Btrfs or HFS+.</strong> 
  You literally have to rely on <strong>additional software’s for features like self healing data or built-in snapshots.</strong> 
  <strong>If you’re really interested in real life innovation, time to step out of this loop and try something new and experience it.</strong> 
  Even Linux support native NTFS support from the <strong>“ntfs-3g” library</strong> so you have a system which just supports all those file systems making it almost completely robust. 
</p>