# Contribute to the Keptn Website

This is the repository of the [Keptn](https://keptn.sh) website, which uses the [Hugo static website generator framework](http://gohugo.io).
Keptn is a message-driven control-plane for application delivery and automated operations.
## Run locally

Perform the following steps to create a copy of this repository on your local machine:

1. Fork the Keptn repository.
A copy of this repository is available in your GitHub account.

2. Clone the forked repository in a local directory.
    ```
    git clone https://github.com/UserName/keptn.github.io
    ```
	Where UserName is your github username. The keptn.github.io directory is available in the local directory.

3. Install the **extended** version of [Hugo](http://gohugo.io) in [Version 0.53](https://github.com/gohugoio/hugo/releases/tag/v0.53) (see [netlify.toml](netlify.toml) - `HUGO_VERSION = "0.53"`).
The themes directory on your local machine (localdirectory/keptn.github.io/themes) is empty because the Hugo serif theme is available at https://github.com/zerostaticthemes/hugo-serif-theme. You need to load the git submodule (see next step) to install this theme.

4. Install the git submodule in the themes directory.
    ```
    git submodule update --init --recursive --force
    ```
5. Execute the `hugo server -D` command from the root folder:
    ```
    hugo server -D
    ```
6. Enter the following in a browser to view the website:
    ```
    http://localhost:1313/

	# Press Ctrl+C to stop the Hugo server
    ```

Start contributing! Note that all edits to the files are updated immediately.

## Push to production

Before you push to production, make sure to run the following command in order to get the correctly built files:

```
hugo
```

# License

keptn.sh is licensed under an [Apache 2.0 license](./LICENSE).

The Keptn documentation (e.g., `.md` files in the `/content/docs` folder) is licensed under a [CC-BY-4.0 license](./LICENSE-docs).
