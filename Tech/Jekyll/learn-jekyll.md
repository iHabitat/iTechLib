# Welcome

## what is jekyll?

jekyll is a simple, blog-aware, static site generator.

# Quick-start guide

	$ gem install jekyll
	$ jekyll new myblog
	$ cd myblog
	$ jekyll serve
	
	# => browse to localhost:4000
	
# Installation

## Requirements

- Ruby
- RubyGems
- Linx, Unix or Mac OSX

## Install with RubyGems

The best way to install jekyll is via RubyGems.  

	$ gem install jekyll

# Basic Usage

The Jekyll gem makes a `jekyll` executable available to you in terminal. We can use this command in a number of ways:

	$ jekyll build
	# => The current folder will be generated into ./_site
	
	$ jekyll build --destination <destination>
	# => The current folder will be generated into <destination>
	
	$ jekyll build --source <source> --destination <destination>
	# => The <source> folder will be generated to <destination>
	
	$ jekyll build --watch
	# ==> The current folder will be generated into ./_site, watched for changes,
	# and regenerated automatically.
	
***Destination folders ared cleaned on site builds.***

Jekyll also comes with a built-in development server that will allow you to preview.

	$ jekyll serve
	# => A development server will run.
	
	$ jekyll serve --detach
	# => same as `jekyll serve` but will detach from the current terminal.
	# If you want to kill the server, you can `kill -9 pid`
	# Get pid: `ps aux | grep jekyll`
	
	$ jekyll serve --watch
	# => same as `jekyll serve`, but watch for changes and regenerate automatically.
	
There are just a few of the available configuration options. Many configuration options can be specified in a `_config.yml` file at the root of the source directory. For example:

	source: 	_source
	destination: _deploy
	
Then the following two commands will be equivalent:
	
	$jekyll build 
	$jekyll build --source _source --destination _deploy


