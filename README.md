<!-- # Hugo Commands

## Deploy with draft posts
`hugo serve -D`

## Create new post
`hugo new post/post-title.md`

## Build static pages
`hugo` add `-D` flag to build draft sites. 
Command if stuff not updating `hugo --cleanDestinationDir -d docs`

#  starting up
After cloning, add theme: `git submodule add https://github.com/dillonzq/LoveIt.git`

# Demo mp4 to gif
`ffmpeg -i demo.mp4 -vf "fps=20,scale=1000:-1:flags=lanczos,split[s0][s1];[s0]palettegen[p];[s1][p]paletteuse" -loop 0 demo.gif` -->

# Akansha Jha's Personal Blog

## Overview
A personal blog where I write about programming and other tech-focused topics.

## Live website
[https://jhaakansha.github.io/](https://jhaakansha.github.io/)

## Built with
- **Framework**: [Hugo](https://gohugo.io/) (static site generator)  
- **Theme**: [Stack](https://github.com/CaiJimmy/hugo-theme-stack) by Jimmy  
- **Hosting**: GitHub Pages  
- **Content Format**: Markdown

## Features
- Dark mode toggle
- Clean, minimal navigation: Home, About, Archives, Search, Links


## Installation and Local Development

To run the blog locally:
```bash
# Clone the repository
git clone https://github.com/jhaakansha/your-blog-repo.git
cd your-blog-repo
```


# Install Hugo (follow guide at https://gohugo.io/getting-started/installing/)

# Run the development server
hugo server
then visit http://localhost:1313 in your browser.

## Usage
- Add new posts in `content/posts/` as .md files.
- Use Hugo front matter to define metadata like `title`, `date`, `summary`, `tags`, etc.
- Static pages like About can be added under `content/` .

## Deployment
- Hosted via GitHub Pages.
- Hugo builds the static site. Just push updates to the `main` branch and the site gets rebuilt automatically (if Pages is configured).
