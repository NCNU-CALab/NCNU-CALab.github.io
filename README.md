# SSYU 的書們

## Structure
| Purpose | Layout        | Path          |
|:-----| :-------------: |:-------------|
| C 語言程式教學 | page | source/c |
| Java 語言程式教學 | page | source/java |
| 資料結構與演算法程式 | page | source/dsa |
| 題庫（草稿） | draft | source/_drafts |
| 題庫 | post | source/_posts |

## Installation
You'll need Node.js and Git installed as pre-requisites.

### Installing the Hexo CLI
Install the Hexo command line interface (CLI) globally with the Node.js package manager (NPM):

```
$ npm install -g hexo-cli
```

### Clone this repo and install dependencies
```
# clone repo
$ git clone https://github.com/NCNU-CALab/NCNU-CALab.github.io.git

# install dependencies
$ cd NCNU-CALab.github.io
$ npm install

# install theme
$ git submodule update --init --recursive

# install theme dependencies
$ cd themes/doc
$ npm install --prod
```
## Writing

### Creating question page
```
$ hexo new draft "Question Title"
# creates -> ./source/_drafts/Question-Title.md
```

**meta data**

| Variable | Purpose|
|:-----:| :------------- |
| title | question display title |
| date | question published time |
| category | question source |
| tags | just tags |

### Starting the server
To view newly created site in a browser, start the local server:

```
$ hexo server --draft
# will run at http://localhost:4000.
```

If you get ENOSPC Error when run hexo server on Linux (e.g. Ubuntu), you may need [this](https://hexo.io/docs/troubleshooting.html#ENOSPC-Error-Linux)

### Publishing drafts
When it's time to move the draft to a "live" post for the world to see, use the hexo publish command:

```
$ hexo publish My-First-Blog-Post
# updates published time and moves from ./source/_drafts/ to ./source/_posts.
```

Then just commit and push, Travis CI will build and deploy automatically.
