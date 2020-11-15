https://github.com/gohugoio/hugo
https://gohugo.io/

## Hugo Docker
* Hugo doesn't seem to have an official image
* We will create a Hogo Image for working with VSCODE

For installing current version of hugo we downlaod the build binaries using curl: apk add curl
For installing themes hugo typically uses git submodules: apk add git

Okay. so Apparently we will need the EXTENDED version of hugo which includes SCSS, SASS to CSS support. I chose 3 themes and they ALL required this.
https://gohugo.io/troubleshooting/faq/#i-get-this-feature-is-not-available-in-your-current-hugo-version


## Using Hugo


By Default Hog generates to the public directory.
But we don't want it to generate there because we are building the site with multiple areas.
Blog and doc. So we will actually want to output furhter up tree

Hugo Server can be used to run livereload version. This makes sense. Again we have multiple sections or hugo sites... so we can start them up with Dockercompose
in the deploy/development/compose.yml


* By Default the configuration is created in a toml file: config.toml
* Git modules requires the solution or porject already be a git repository!


Site Root with coder theme
```
hugo new site root
cd root
git submodule add https://github.com/luizdepra/hugo-coder.git themes/coder
echo theme = \"coder\" >> config.toml
```

Doc section using the book theme:
```
hugo new site doc
cd doc
git submodule add https://github.com/alex-shpak/hugo-book.git themes/book
echo theme = \"book\" >> config.toml
```

Blog section with jane theme
```
hugo new site blog
cd blog
git submodule add https://github.com/xianmin/hugo-theme-jane.git themes/jane
echo theme = \"jane\" >> config.toml
```

## Post creation
Edit the config.toml for each section of the site


## Run with docker and docker compose

compose command:
hugo serve --bind 0.0.0.0 -w





