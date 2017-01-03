+++
title = "Write and public blog with markdown"
date = "2015-10-22T15:50:00+08:00"
#Modified: 2015-10-22 15:50
categories =  ["Cloud Service"]
tags = ["cloud service", "blogging"]
#Slug: markdown-blogging
#Author: elprup
#Summary: how to write and public a markdown blog website
+++

Write blog and maintain a blog site is trivial work. Wordpress is too heavy. You should open several webpage, keep online to write an article. If you prefer simplier solution to build your blog site, pelican is a good choice.

![pelican](/img/pelican.png)

## Why [Pelican](http://docs.getpelican.com/en/3.6.3/)
Pelican is a static page generator. You write markdown file and pelican use it to generate blog html files. It means your site is totally static, it's safe and can support more visits.

## Prepare: Nginx
Config nginx to get basic static page run.  

1. make static directory, we use /var/www/blog_static  
	`# sudo mkdir -p /var/www/blog_static`  
   for easy visit by your user, change own to your username
   `# chown yourname:yourname /var/www/blog_static`
2. create a config file in `/etc/nginx/site-available` named `blog.mysite.com`

```
    server {
        root /var/www/blog_static;
        index index.html index.htm;

        server_name blog.mysite.com;

        location / {
                # First attempt to serve request as file, then
                # as directory, then fall back to displaying a 404.
                try_files $uri $uri/ =404;
                # Uncomment to enable naxsi on this location
                # include /etc/nginx/naxsi.rules
        }
    }
```

3. Link it to site-enable  
  `# cd ../site-enabled`  
  `# ln -s ../site-available/blog.mysite.com`  
4. Restart nginx, put some html page on your static directory to check if it works

## Deploy pelican
First choose another directory as your working director, in this article, we use `/home/elprup/pelican_workspace`
### python virtual environment
```
$apt-get install python-virtualenv python-dev
$cd /home/elprup/pelican_workspace
$virtualenv env
$source env/bin/activate
(env)$pip install pelican Markdown
```
### start project
```
pelican-quickstart
```
After answer several questions, there's work files created.

### start to write your first blog
All your pages will in content folder. You can write files end with .md, so pelican can detect them. For example, I publish one article in content directory named first.md

```
My super title
##############

:date: 2010-10-03 10:20
:modified: 2010-10-04 18:40
:tags: thats, awesome
:category: yeah
:slug: my-super-post
:authors: Alexis Metaireau, Conan Doyle
:summary: Short version for index and feeds
```

then go back to work space folder, try `make html`

now your generated html files are in output folder. 

### publish your blog
```
pelican content -s publishconf.py
cp output/* /var/www/blog_static
```
or more efficient using rsync
```
pelican content -s publishconf.py
rsync -riz --delete output/ /var/www/blog_static
```

Now your website is online!

## Write markdown easy
If you use mac, [Macdown](http://macdown.uranusjr.com/) is a very nice tool to write markdown and render it in real time.

Happy sharing with markdown!
