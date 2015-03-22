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

###HTML(ERB format):

{% gist 2026eda60e1229910139 %}
 
####Custom SearchBox (lines 6-23):
   * Setup the div leveraging bootstrap classes
   * Due to styling issues with bootstrap dropdowns and addons, we will use a drop down menu here instead of select box
   * We assign a value to each line element. **this is important** The values here will correspond to the column indexes that dataTables uses.
      * dataTables starts at 0 and goes left to right in indexing the columns.  This is how we will determin which column to search if one is selected.
   * The result here will be a drop down search menu with search box and info button for help instructions.
   * Note the assigned id s on some of the elements.  These will be used when we write the javascript.

###JavaScript:

{% gist 91513aaf4657fcf9a302 %}

####Setup basic dataTable with search box (lines 15-25):
  * lines 16-18: Setup the table and specifically set the sDom for length, information, and pagination excluding the filter(f) attribute.
     * We could also not include the sDom setting and just hide the specific dataTables filter class.
  * lines 19-21: Search the table on keyup from the searchbox id
  * lines 22-24: Enable the popover tip functionality of bootstrap for our info button

####Dropdown column switching (lines 6-14):
- line 7: First clear the search filters
- lines 8-9: Remove the active class from all and then apply it back to the one that was clicked on
- lines 10-12: Find the selector button and replace the text of it with the current clicked on item.  Append back the caret.
- lines 13: Clear out the search text box

####Perform Search using dataTables API (lines 27-39):
- line 28: Get the value of the active selection
- lines 29-32: If the column index is Search All(-1), then perform the search against the whole table and redraw it.
- lines 33-37: Else if a specific column is selected, perform a column search and redraw the table.
- Note: We have set the options to true for the search in each case regex and smart search.
   * More can be read on what these options mean [here](https://datatables.net/reference/api/search()).

###CSS:

{% gist 51c7e00b675bfad2d58b %}

- Here we are simply aligning the dataTables length with our custom search box.  Effectivily putting our search box in the place where the default one was.
   * Note: This is due to the dataTables dom setup.
