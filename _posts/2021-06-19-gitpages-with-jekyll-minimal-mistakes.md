---
title: "Post: Beginners Guide to setting up your site on GitHub pages with Jekyll"
last_modified_at: 2021-06-19
categories:
  - blog
tags:
  - jekyll
  - github pages
  - minimal mistakes
---

This is a very simplistic guide to setting up my own personal blog using github pages.  As a GitHub user you do have one free "user" website which will live at https://username.github.io

Some assumptions:

1. Using a mac; This has been tested on Big Sur 11.4 with an Apple M1 Chip
2. Ability to use homebrew and Git

## Mac Setup

A prerequisite to installing Jekyll is Homebrew and Ruby. 

1. To install `Homebrew`:
   
    ```bash
    mkdir homebrew && curl -L https://github.com/Homebrew/brew/tarball/master | tar xz --strip 1 -C homebrew
    export PATH=$HOME/homebrew/bin:$PATH
    ```
2. Install & configure `Ruby` with `rbenv` 
   
    ```bash
    brew install rbenv
    
    # set up rbenv and integrate it with your shell
    rbenv init
    
    # verify your installation
    curl -fsSL https://github.com/rbenv/rbenv-installer/raw/master/bin/rbenv-doctor | bash
    
    # restart your terminal session
    ```
3. Check [GitPages Dependencies](https://pages.github.com/versions/) to obtain the version of Ruby plus other Gem dependencies
      
    ```bash
    # as of 19/06/2021 the  ruby version required is 2.7.3
    rbenv install 2.7.3
    rbenv global 2.7.3
    echo "export PATH=$HOME/.gem/ruby/2.7.0/bin:$PATH" >> ~/.zshrc

    # verify the version of ruby
    ruby -v
    > ruby 2.7.3p183 (2021-04-05 revision 6847ee089d) [arm64-darwin20]
    ```

4. Install `Jekyll`
  
    ```bash
    gem install --user-install bundler jekyll
    ```
5. Next lets create the repo:
*    go to [new repo](github.com/new) 
*    enter the Repository name: `username.github.io`
*    make the repository public
*    check `Initialize the repository with README`
*    click `Create Repository`
*    goto to your terminal and clone the repo
     ```bash
     git clone https://github.com/username/username.github.io.git
     cd username.github.io
     ```
6. Add the Gemfile 
   
    ```bash
    # Gemfile
    source 'https://rubygems.org'

    gem "jekyll", "~> 3.9.0"
    gem "minimal-mistakes-jekyll"
    group :jekyll_plugins do
        gem "github-pages", "~> 215"
        gem "webrick", "~> 1.7.0"
        gem "jekyll-feed", "~> 0.15.1"
        gem "jekyll-archives", "~> 2.2.1"
        gem "jekyll-paginate", "~> 1.1.0"
        gem "jekyll-sitemap", "~> 1.4.0"
        gem "jekyll-gist", "~> 1.5.0"
        gem "jemoji", "~> 0.12.0"
        gem "jekyll-include-cache"
      end
    ```
7. Add the _config.yml found [here](https://github.com/celtan/jekyll-template/blob/main/_config.yml).  Update this file with your details

8. Copy the remaining files from the repo:
   
      - index.html
      - assets/      # update the bio-photo.png in the images directory
      - _data/       # navigation.yml defines what your navigation will look 
      - _pages/      # pages for navigation
      - _posts/      # this is where all your posts will live
      - _scripts/    # this is where all your scripts that you want to share will live

      ---
      **Note:**
      This template sets up a page using the [Minimal Mistakes Theme](https://mmistakes.github.io/minimal-mistakes/) with three Navigation links -> Posts, Scripts, About Me, like so:
      ![default-jekyll-page](/assets/images/default-jekyll-page.png)
      
      ---
      
      To add additional navigational links:
      - replicate the [scope](https://github.com/celtan/jekyll-template/blob/main/_config.yml#L82-L87) block
      - add the [navigation](https://github.com/celtan/jekyll-template/blob/main/_data/navigation.yml#L6-L7)
      - add in the new [page](https://github.com/celtan/jekyll-template/tree/main/_pages) 

           [scripts.md](https://github.com/celtan/jekyll-template/blob/main/_pages/scripts-archive.md) can be used as an example.

        Create your new posts and save it with this format:
        `YEAR-MONTH-DAY-name-of-post.md` - save this file in the _posts directory
        Create your new scripts and save it with this format:
        `YEAR-MONTH-DAY-name-of-script.md` - save this file in the _scripts directory
      - To test your site locally run:
        ```bash
        # at the root of your repo ie <path-to>/username-github-io
        bundle exec jekyll serve

        # browse to:
        http://localhost:4000
        
        ```

9.  Attaching a domain:
    
    - Create a CNAME file with the following content;  For this case we are going to call this blog.mydomain.com
      ```bash
       blog.mydomain.com
      ```
    - In your DNS Management console add a CNAME record
      - Create the first CNAME record:
          ```
          type: `CNAME`
          name: `www`
          value: `username.github.io`
          TTL: `1/2 Hour`
          ```
      - Create the second CNAME record:
          ```
          type: `CNAME`
          name: `blog`
          value: `username.github.io`
          TTL: `1/2 Hour`
          ```
    
10. Add your Custom Domain    
    
    - Navigate to your repository: [https://github.com/username/username.github.io/settings/pages](https://github.com/username/username.github.io/settings/pages)
        - add your custom domain details:
          - Custom domain: `blog.mydomain.com` 
          - click `Save`
          - It will take a few minutes to verify before you will be able to click `Enforce HTTPS`
      
    - Navigate to `https://blog.mydomain.com` to your personal blog!!
