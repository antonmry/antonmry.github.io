+++
date = "2016-06-16T20:49:50+02:00"
draft = false
title = "Time to start a Golang blog with Hugo"
tags = [ "Go", "Hugo", "Blogging" ]
categories = [ "Blogging" ]

+++

Well, this is embarrassing. I've started another blog!. Why?. I've started some time ago to write a blog, it started as a corporate thing but later it has become a place where to write or document interesting things I do in my work, most of them related to Java, Groovy and Oracle technologies. The blog is generated with [JBake](http://jbake.org/) with the [gradle plugin](https://github.com/antonmry/jbake-gradle-plugin) and I'm quite happy with the result, but to be honest, I don't write frequently.

Six months ago I've started to learn and use [Golang](https://golang.org/). I was very impressed with the simplicity of the language and I've started to read a lot of articles, books, etc., watch videos and develop some things. I discovered [Hugo](https://gohugo.io/), a site generator similar to JBake but written in Go. I was tempted to migrate the current blog, but today, it's simpler to open a new one and I can't keep things separated. In another post I will write more about Go and why I found it so interesting. 

As the first post, let's see how to configure [Hugo](https://gohugo.io/). I've to say it was really easy, I really impressed with piece of software. I can comparey with other site generators as Jekyll, Hexo or JBake, and I have to say Hugo is my favourite.

First of all, I've created a new repo in my Gihub account and cloned it in my laptop:

```sh
git clone git@github.com:antonmry/antonmry.github.io.git
cd antonmry.github.io 
```

Because I'm going to use my github user page, the source code of the page has to be in master branch. For your Hugo code, you can have a separate repo or just a different branch. I choose the second option with a branch named source:

```sh
git checkout -b source
git push -u origin source
```

Inside the source code folder, we are going to create a folder linking to the master branch:

```sh
git subtree add --prefix=public git@github.com:antonmry/antonmry.github.io.git master --squash
git subtree pull --prefix=public git@github.com:antonmry/antonmry.github.io.git master
```

It's time to generate the site. First you have to install Hugo, plenty of option in [the Hugo website](https://gohugo.io/overview/installing/). I've chosen the Fedora package. 

```sh
hugo new site antonmry.github.io
mv antonmry.github.io/* .
rm -r antonmry.github.io
```

Choose your theme, there are very nice options:

```sh
cd themes
git clone https://github.com/halogenica/Hugo-BeautifulHugo.git beautifulhugo
cd ..
echo "theme = \"beautifulhugo\"" >> config.toml
```

Create your first blog and start the server: 

```sh
hugo new post/hugo-site-created.md
hugo server --buildDrafts
```


Open in your browser http://localhost:1313/ and enjoy your first blog with Hugo ;-)

Let's move the post from draft to publised, edit the file content/post/hugo-site-created.md and change the draft line from true to false.

Now it's time to upload to Gihub the first version of Hugo and make it public:

```sh
git add -A
git commit -m "Initial version"
git push
git subtree push --prefix=public git@github.com:antonmry/antonmry.github.io.git master
```

Oh, really, can it be so easy?. Just go to https://antonmry.github.io (or your equivalent site)

I was thinking in create a travis job to do the publishing in master (I did it before for jbake) but to be honest, this method is quite simple. I've added to my .bash_alias the last command and that's all I need.

Enjoy!
