
## GIT Playbook

GIT Playbook is a documentation framework that allows you to create Documentation for your project using Markdown and GH-Pages rapidly. This is the repository for Medium article <a href="xyz" target="_blank">Create compelling Documentation with Github Pages</a> by <a href="https://github.com/thomasreinecke" target="_blank">Thomas Reinecke</a> and <a href="https://github.com/kaiwedekind" target="_blank">Kai Wedekind</a>. 

![image](https://user-images.githubusercontent.com/35994116/45641683-75532e80-bab6-11e8-81a6-f31ad27f1bf4.png)

* <a href="https://thomasreinecke.github.io/git-playbook/#/playbook" target="_blank">Demo environment (on GH pages)</a>
* <a href="https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet" target="_blank">Markdown Cheatsheet</a>

## How to use it
* get started by reading the article <a href="xyz" target="_blank">Create compelling Documentation with Github Pages</a>
* clone repository and follow the "Build Setup" instructions below
* edit "src/pageConfig.json" and change the document structure as you desire
* write your documents in Markdown and put them into the "static" folder
* create yourself a GIT repository and import your playbook trunk
* deploy it with `npm run gh-pages`
* in your GH repository > Settings > scroll down to "Github Pages", make sure it points to your "gh-pages" branch
* find the link to your GH page deployment on that screen aswell
  ![image](https://user-images.githubusercontent.com/35994116/45642352-36be7380-bab8-11e8-95d5-a014c0422f63.png)
* for feedback, enhancement requests or defect reports use "Issues" here on this repo

Enjoy!

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification, also build the search index
npm run build

# build the search index only
npm run build-search

# build for production and view the bundle analyzer report
npm run build --report

# run unit tests
npm run unit

# run e2e tests
npm run e2e

# run all tests
npm test

# deploy playbook to GH pages (includes build and deploy to GH pages)
npm run gh-pages
```

