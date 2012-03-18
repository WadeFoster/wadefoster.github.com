Date: 2011-3-17
Title: Creating a blog in Python hosted on GitHub using Pelican
Tags: blog, python

Tools don't make a good writer, but good tools can certainly help a writer get more done.

I've constantly struggled with the internal workings of Wordpress and always wanted a blog built on python. I've tried rolling my own blog in Django, but ultimately I don't want to maintain my own blog site.

I want something that is simple, light weight, and will get out of my way.

I think I've found a solution.

## Enter Pelican

Pelican is a static page generator much like ruby's Jekyll. 

It's written and maintained by [Alexis Métaireau][am]. 

Why Pelican? Because I:

* can write blog posts in Sublime using markdown
* don't need an internet connection to write
* can publish posts using git
* have free hosting via [GitHub pages][ghp] that scales with me so if ever hit the front page of [HN][hn] I'm good to go. 

## How to setup a blog with Pelican

Setting up a blog with Python is pretty easy even for someone who hasn't written a ton of code.

First create a repo on GitHub with your repository. Mine is titled wadefoster.github.com.

Next you need to create a directory for storing your blog locally:

	mkdir mysite
	cd mysite

Once you have your directory set up you can optionally choose to create a [virtual environment][ve] though this step isn't necessary.

	mkvritualenv env_mysite
	workon env_mysite

Then run the following set of commands to make sure you initialize git and are able to push to GitHub. Substitute your repo name for mine where applicable. 

	git init
	touch README
	git add README
	git commit -m 'first commit'
	git remote add origin git@github.com:WadeFoster/wadefoster.github.com.git
	git push -u origin master

Congrats. You've successful made your first commit. 

From here we are ready to install pelican and markdown.

	pip install pelican
	pip install markdown

The last thing you need to do is configure your settings file. 

In the root of your directory create a settings.py file. Here's mine as an example:
	
	# File Name: settings.py

	AUTHOR = 'Wade Foster'
	CATEGORY_FEED_RSS = 'feeds/%s.rss.xml'
	DEFAULT_CATEGORY = 'blog'
	DISQUS_SITENAME = 'wadefoster'
	FEED_RSS = 'feeds/all.rss.xml'
	GITHUB_URL = 'https://github.com/wadefoster/wadefoster.github.com'
	GOOGLE_ANALYTICS='UA-30070965-1'
	SITEURL = 'http://wadefoster.github.com'
	SITENAME = 'Wade Foster'
	SOCIAL = (('twitter', 'http://twitter.com/wadefoster'),
	          ('github', 'https://github.com/wadefoster'),)
	TAG_FEED = 'feeds/%s.atom.xml'
	TIMEZONE = "America/Chicago"
	THEME='notmyidea'
	TWITTER_USERNAME = 'wadefoster'

Now you are ready to start writing.

## Writing blog posts with Pelican using Markdown

Pelican has us put all of our posts in a subdirectory within the root of our site.

	mkdir posts

Personally, I call this directory 'posts.' In practice I've seen it called 'content,' 'source,' and a number of other things.

Inside the directory create and open a file called first.md. This is where we'll compose our first post using markdown.

Posts are then formatted using [markdown][md] and you can add a post title, the content of the post, and other meta data like so.

	# File Name: first.md

	Date: 2011-3-17
	Title: My First Post
	Tags: blog, python

	Your content goes here. 

Save your post and you are ready to publish. 

## Publishing blog posts with Pelican

First let's generate our blog.

	pelican posts -o . -s settings.py

We run the pelican command, point it to our posts directory, output the generated content to root and apply our settings file.

Next make sure things are running locally so lets fire up a server to view our blog. This should fire up our site on port 8000. 

	python -m SimpleHTTPServer

Look good? Then we're ready to check in our code and push to master on GitHub for publishing. 

	git add .
	git commit -m 'added our first post'
	git push

In about ten minutes GitHub will build your page at username.github.com. 

## Using a Custom Domain

If you want to use a custom domain, GitHub provides this functionality for free. 

First add a file called CNAME in the root directory of your project with the domain as it's content like so.

	wadefoster.net

Next add an A record to your DNS management for GitHub pointing to `207.97.227.245`.

It could take a while for the DNS changes to propagate, but you should be good to go. 

If you'd like to further customize your pelican blog check out the [pelican documentation][pd].

[am]: https://twitter.com/#!/ametaireau
[hn]: http://news.ycombinator.com/
[ghp]: http://pages.github.com/
[ve]: http://www.doughellmann.com/docs/virtualenvwrapper/#
[md]: http://daringfireball.net/projects/markdown/syntax
[pd]: http://pelican.notmyidea.org/en/2.8/index.html
