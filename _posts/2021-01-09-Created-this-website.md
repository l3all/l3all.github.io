---
layout: post
title: "Creation process to this website"
subtitle: "Because I can!"
background: '/img/posts/Creation-process/clement-helardot-95YRwf6CNw8-unsplash.jpg'
---

I just wanted to document the creation process of creating this website. The goal of this site is to share what I do with you. I will be promoting my woodworking projects and everything I deem showable of the free time stuff going on here.

So I needed a simple vessel to present that. Here we are with the technicals:

<br />
#### Installing brew
Brew is a package manager for macOS. It is immensly useful to install things and keep them up to date. [brew.sh](https://brew.sh)

    xcode-select --install
    /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

Enter your macOS password

The installation should perform without any errors.

<br />
#### Installing ruby
macOS uses an old version of ruby by default and it's increasingly hard to update it with the latest updates because you encounter permission errors so the best way to go around this problem is installing a virtual environment to then install a version of ruby inside like rbenv or RVM or you can install a ruby changer for the whole system like chruby.
In my case I chose chruby and its friend ruby-install. Ruby-install will install from source any version of ruby so chruby can select the one to use system wide.

    ruby -V   #Will output your current version of ruby
    brew install churby ruby-install -y
    chruby -V   #To see if it's installed properly
    ruby-install ruby 2.7.2   #Will install ruby 2.7.2 from source in chruby
    chruby ruby 2.7.2   #Set ruby 2.7.2 system wide
    ruby -V   #Should now be 2.7.2. Ready to go!

<br />
#### Installing Bundler and Jekyll
As per jekyll website [jekyllrb.com](https://jekyllrb.com), you need bundler and jekyll to be on the right track.

    gem update --system   #to make sure your gem is up to date
    gem install bundler jekyll
    echo 'export PATH="$HOME/.gem/ruby/2.7.0/bin:$PATH"' >> ~/.zshrc

<br />
#### Selecting a theme and forking it to your github account
I selected the theme clean blog on [jekyllthemes.org](http://jekyllthemes.org/themes/clean-blog/). Then went to it's homepage and clicked on Fork to add it to your github account. You need to have one beforehand. Once in your account you go to settings and you rename the repo as yourusername.github.io then you save that. if you want to use a custom domain name we will cover that later.

    brew install git -y
    git config --global user.name “your_github_username”
    git config --global user.email "your_email@github.com"
    git config --global core.editor "atom --wait"

Now create a Dev folder on your macOS to keep things organised. In your new yourusername.github.io repo you have to follow the Using Core Files guide.

    cd ~
    mkdif Dev
    cd Dev
    git clone https://github.com/yourusername/yourusername.github.io.git  #This will copy the repo to a new directory in your Dev directory.
    cd yourusername.github.io

<br />
#### Using atom as the IDE
Install atom [atom.io](https://atom.io)

    brew cask install atom -y
    cd ~/Dev/yourusername.github.io
    atom .  #This tells atom to open the current directory.

Now you need to go in the menu bar and install Shell commands. Then close atom and reopen it in the terminal. You can now update the _config.yml file with your info.

<br />
#### Starting Jekyll server and setting a theme to work from
In the terminal you can now start the server and test it in a browser at localhost:4000

    cd ~/Dev/yourusername.github.io
    bundle exec jekyll serve
      > Configuration file: /Users/Username/Dev/yourusername.github.io/_config.yml
      > Source: /Users/Username/Dev/yourusername.github.io
      > Destination: /Users/Username/Dev/yourusername.github.io/_site
      > Incremental build: disabled. Enable with --incremental
      > Generating...
      > Jekyll Feed: Generating feed for posts
      > done in 5.524 seconds.
      > Auto-regeneration: enabled for '/Users/Username/Dev/yourusername.github.io'
      > Server address: http://127.0.0.1:4000/


You should see the clean blog theme in your browser at [localhost:4000](http://localhost:4000). Now it's time to customize it the way you want. Time to learn how jekyll, bootstrap, css, html works. let's put [youtube.com](https://youtube.com) to the task!

Here are some videos I found helpful

* Youtube channel [Dataslice](https://www.youtube.com/watch?v=wCOInE7-E0I)
* Youtube channel [Grafikart.fr](https://www.youtube.com/watch?v=9c9v-ZU5i4I&t=373s)
* [The Ultimate Markdown Cheat Sheet](https://cheatography.com/lucbpz/cheat-sheets/the-ultimate-markdown/)

<br />
#### Using github.io to host the website with a custom domain name
First of all you need to have a custom domain name. You have to go to a domain name registrar and buy one.

In your yourusername.github.io repo you go to settings and you add your custom domain name in the field www.mysite.com and you save and click on [learn more](https://docs.github.com/en/free-pro-team@latest/github/working-with-github-pages/configuring-a-custom-domain-for-your-github-pages-site). There you go to Managing a custom domain for your GitHub Pages site. (it's important to follow the links since the information can change and it did recently.) There you have to make a choice. Read carefully. I chose to be safe and use the www.site.com way for the moment.

<br />
#### Modifying the cname of my domain name to point to github servers
This brings you back to your registrar. You have to update the info as per your choice to use the www.site.com (subdomain way) or the site.com (apex domain way). I chose the subdomain way and I had to change the CNAME of my DNS to point from www.site.com to yourusername.github.io.

    dig www.site.com +nostats +nocomments +nocmd
      > ; <<>> DiG 9.10.6 <<>> www.site.com +nostats +nocomments +nocmd
      > ;; global options: +cmd
      > ;www.site.com.			IN	A
      > www.site.com  .		21599	IN	CNAME	yourusername.github.io.
      > yourusername.github.io.	3599	IN	A	185.199.109.153
      > yourusername.github.io.	3599	IN	A	185.199.111.153
      > yourusername.github.io.	3599	IN	A	185.199.110.153
      > yourusername.github.io.	3599	IN	A	185.199.108.153

<br />
We are here now: Creating this page to document the whole thing
