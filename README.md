# Nidhi's Personal Website

This repository contains the code of my personal blog, built using [hugo-PaperMod](https://github.com/adityatelange/hugo-PaperMod) and [Github Pages](https://docs.github.com/en/pages/getting-started-with-github-pages/about-github-pages).

Check out my website at [nidhihemanth09.github.io](https://nidhihemanth09.github.io) 👋

## How to run this project locally?

### 1) Installing pre-requisites

- [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
- Latest version of [Hugo](https://github.com/gohugoio/hugo/releases)

Visit [here](https://gohugo.io/installation/) for detailed instructions.

### 2) Set up the project locally

Clone the project on your local machine.

For example, using https ```git clone https://github.com/nidhihemanth09/nidhihemanth09.github.io.git```

### 3) Run the hugo server

Open the command line in the cloned directory.

Run ```hugo server``` which will run the application locally on the default port [http://localhost:1313/](http://localhost:1313/) 

Now you can view & test your changes locally before deploying it 🍜

> Create new pages using ```hugo new <filename>``` on your website, refer to your respective theme documentations for detailed instructions.

## How to create your own website using Hugo & Github Pages?

Hugo is a fast and modern static site generator written in Go, and designed to make website creation fun again. 

Hugo provides us a wide variety of [themes](https://themes.gohugo.io/) which makes it easier to create & maintain simple websites. The theme used in this repository is called [PaperMod](https://themes.gohugo.io/themes/hugo-papermod/).

Follow the detailed installation guide provided here [PaperMod Installation Guide](https://github.com/adityatelange/hugo-PaperMod/wiki/Installation).

## How does the website get published to the GitHub Pages?

Hugo generates your actual public website inside a temporary folder when it builds.
This repository is configured to deploy GitHub Pages using the custom build output created by Hugo.

You can do that by:
1. Adding the following file [.github/workflows/deploy.yml](.github/workflows/deploy.yml) with the same directory structure under your project root directory.
     > You might want to change the version of hugo being downloaded to the latest version.
2. Changing the repository's Settings > Pages > Build and deployment: Source to **GitHub Actions** 

GitHub automatically deploys the code at `https://<username>.github.io`
