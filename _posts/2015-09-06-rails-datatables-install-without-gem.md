---
layout: post
title: "rails dataTables install without gems"
category: javascript
tags: [javascript datatables rails tables]
---
____
[jquery datatables rails](https://github.com/rweng/jquery-datatables-rails) is a gem that conveniently wraps the datatables library in a gem for rails.  However, using that gem adds an unnecessary layer between your app and datatables library.  This can mean you will have to wait for the gem to be updated to use latest datatables updates.

<!--more-->

###The Problem:
- Datatables has a new revision, but the rails gem for datatables has not been updated.  The current version of the gem(4.0.5), has a version of datatables that contains a bug exposed by the new version of jquery that causes errors on the search filter.  That coupled with not being up to date with the latest datatables API can be frustrating, and uneccessary for how simple it can be to use datatables in rails with out this gem.

###Framework:
- [Rails 4.2.3](http://rubyonrails.org/) + [DataTables](http://datatables.net/) + [twitter bootstrap](https://github.com/seyhunak/twitter-bootstrap-rails)
  * This is a very simple rails app, built only to demonstrate the install of datatables
     * source code for this blog can be found [here](https://github.com/dstull/dataTables-search/tree/manual_install).

###Install Steps
   * download the latest datatables [zip](http://datatables.net/download/packages)
   * find all the *.js files and copy them to your app under vendor/assets/javascripts
   * find all the *.css files and copy them to your app under vendor/assets/stylesheets
   * go to the images directory and copy all of those files to your app under vendor/assets/images
   * add the javascript libraries you want loaded to app/assets/javascripts/application.js like below (lines 16-17 - required, 18-20 extras):

   {% gist 26dcfcaf1e935e93f473 %}

   * add the css files you want loaded to app/assets/stylesheets/application.css like below (lines 14-15 - required, 16-20 extras):

   {% gist 4a228be56fd65b93b37e %}

