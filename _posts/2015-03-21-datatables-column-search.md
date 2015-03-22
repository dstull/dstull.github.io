---
layout: post
title: "dataTables Column Search"
category: javascript
tags: [javascript datatables rails tables]
---
____
[dataTables](https://www.datatables.net/) is a nifty jquery plugin for displaying large sets of data.  However, we need to do smarter column based searching.

<!--more-->

###The Problem:
- How, using [dataTables](https://www.datatables.net/), to create a singular search box that can search whole table and for a singular column.  Via leveraging of the [dataTables API](https://datatables.net/api), we are able to customize our own search/filter.  I believe this is far superior to the default search as it allows search by column without putting an ugly search box on each column.

###Framework:
- [Rails 4.2.0](http://rubyonrails.org/) + [jquery dataTables](https://github.com/rweng/jquery-datatables-rails) + [twitter bootstrap](https://github.com/seyhunak/twitter-bootstrap-rails)
  * We will focus on dataTables here and skip the rails specifific tasks (creating app, model, controller, view, etc)
  * This is a very simple rails app, built only to demonstrate in the simplest possible format the dataTables search
     * source code for this blog can be found [here](https://github.com/dstull/dataTables-search).

###The HTML(ERB format):

{% gist 2026eda60e1229910139 %}
 
####Custom SearchBox (lines 6-23):
   * Setup the div leveraging bootstrap classes
   * Due to styling issues with bootstrap dropdowns and addons, we will use a drop down menu here instead of select box
   * We assign a value to each line element. **this is important** The values here will correspond to the column indexes that dataTables uses.
      * dataTables starts at 0 and goes left to right in indexing the columns.  This is how we will determin which column to search if one is selected.
   * The result here will be a drop down search menu with search box and info button for help instructions.
   * Note the assigned id s on some of the elements.  These will be used when we write the javascript.

###The JavaScript:

{% gist 91513aaf4657fcf9a302 %}
