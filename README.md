# Hugo Commands

## Deploy with draft posts
`hugo serve -D`

## Create new post
`hugo new posts/post-title.md`

## Build static pages
`hugo` add `-D` flag to build draft sites. 
Command if stuff not updating `hugo --ignoreCache --cleanDestinationDir --destination ~/Desktop/tuna/code/git/pratyakshajha.github.io/ -v`

#  starting up
After cloning, add theme: `git submodule add https://github.com/dillonzq/LoveIt.git`

# Demo mp4 to gif
`ffmpeg -i demo.mp4 -vf "fps=20,scale=1000:-1:flags=lanczos,split[s0][s1];[s0]palettegen[p];[s1][p]paletteuse" -loop 0 demo.gif`