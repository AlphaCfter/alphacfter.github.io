# AlphaCfter's Blog

Welcome to my stronghold!

This repository contains the source code for my personal blog, hosted at [alphacfter.github.io](https://alphacfter.github.io).

This blog is where I share what I'm tinkering with, including Linux internals, Java, desktop customization, virtualization and many more. It serves as my personal lab, notebook, and a place to share what I learn.

## Built With

This site is built using [Jekyll](https://jekyllrb.com/) and is based on the [**Chirpy**][chirpy] theme. Do checkout them for a cool starter page

## Running Locally

To preview new posts or changes on your local machine, follow these steps:

1.  **Install Dependencies**
    This project uses [Bundler](https://bundler.io/) to manage the Ruby gems specified in the `Gemfile`.

    Run the following command to install the required gems:
    ```sh
    bundle install
    ```

2.  **Troubleshooting (Permission Errors)**
    If you encounter a `Bundler::PermissionError` (e.g., "error while trying to write to `/var/lib/gems/...`"), it means your user doesn't have permission to install gems to the system-wide directory.

    To fix this, install the gems locally *within* this project folder by running:
    ```sh
    bundle install --path vendor/bundle
    ```
    This creates a `vendor/bundle` directory for all the project's gems. This path is already included in the `.gitignore` file, so the gems won't be committed to your repository.

3.  **Preview the Site**
    Use the provided run script to build and serve the site:
    ```sh
    bundle exec jekyll s -l
    ```
    The script will start a local Jekyll server. You can then view your blog preview by opening the "Server address" in your browser, which is typically `http://127.0.0.1:4000`.

## Contributing

This is a personal blog, but if you find any bugs or have suggestions for the site's structure or theme, feel free to open an issue.

## License

This work is published under the [MIT][mit] License.

[chirpy]: https://github.com/cotes2020/jekyll-theme-chirpy/
[mit]: https://github.com/AlphaCfter/AlphaCfter.github.io/blob/main/LICENSE