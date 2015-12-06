
# Claudia Sikorski's Custom Personal Blog  

My personal blog built using Jekyll and hosted for free on Github pages!

It's my comprimize for a minimalistic yet fully featured file based content management system.

The site is a static html site genrated based on markdown posts found in `./_posts`.  

##Features:
* fully responsive
* latex enabled
* syntax highlights 
* optimized for [Github Pages'](https://pages.github.com/) deployment
* favors beautiful html output over jekyll code formatting 
* simple source code and structure that can be modified or extended easily

I need to add the rest of the features!

##Dependencies:
* [Jekyll](http://jekyllrb.com/)    

I need to add the rest of the dependencies!

##Usage
To run locally:
First install [Jekyll](http://jekyllrb.com/).  This requires both [Ruby on Rails](http://rubyonrails.org/) and [Node.js](https://nodejs.org/)

Then clone the repo and run:       
  bundle install    
    
After ther equired gems are installed, run:     
  jekyll Serve     

browse blog at [localhost:4000](http://localhost:4000)
1. Create new post 

		rake post title="A Title" [date="2012-02-09"] [tags=[tag1,tag2]] [category="category"]

2. Create new page 

		rake page name="about.html"

3. Running Jekyll

	run in directory: `bundle exec jekyll serve -w`    

### Enjoy!