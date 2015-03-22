---
layout: post
title: "dataTables Column Search"
description: ""
category: 
tags: []
---
{% include JB/setup %}

###The Problem:
- How, using [dataTables](https://www.datatables.net/), to create a singular search box that can search whole table and for a singular column

###Framework:
- [Rails 4.2.0](http://rubyonrails.org/) + [jquery dataTables](https://github.com/rweng/jquery-datatables-rails)
  * We'll focus on dataTables here and skip the rails specifific tasks (creating app, model, controller, view, etc)

###The HTML(ERB format):

{% highlight ruby linenos %}
<div class="page-header">
  <h4>
    <span>Cars</span>
  </h4>
</div>
<div class="input-group input-group-sm col-lg-2 pull-right" id="car-search">
  <div class="input-group-btn">
    <button type="button" class="btn btn-default dropdown-toggle" data-toggle="dropdown">Search All 
    	<span class="caret"></span>
    </button>
    <ul class="dropdown-menu" id="column_header" role="meanu">
      <li value='-1' class="active"><%= link_to 'Search All','#' %></li>
      <li value='0'><%= link_to 'Make','#' %></li>
      <li value='1'><%= link_to 'Model','#' %></li>
      <li value='2'><%= link_to 'Year','#' %></li>
      <li value='3'><%= link_to 'Color','#' %></li>
    </ul>
  </div>
  <%= text_field_tag 'search',nil, { class: 'form-control', id: 'searchbox'} %>
  <span class="input-group-addon" id="search-info" data-container="body" data-toggle="popover" data-placement="top" data-trigger="focus" tabindex="0" data-content="search help info explaining how to smart search">
  	<i class="glyphicon glyphicon-info-sign"></i>
  </span>
</div>
<table class="datatables">
  <thead>
    <tr>
      <th>Make</th>
      <th>Model</th>
      <th>Year</th>
      <th>Color</th>
    </tr>
  </thead>

  <tbody>
    <% @cars.each do |car| %>
      <tr>
        <td><%= car.make %></td>
        <td><%= car.model %></td>
        <td><%= car.year %></td>
        <td><%= car.color %></td>
      </tr>
    <% end %>
  </tbody>
</table>

<br>

<%= link_to 'New Car', new_car_path %>
{% endhighlight %}

{% highlight js %}
var Cars = {
    init: function() {
        Cars.setup_datatables();
    },
    setup_datatables: function() {
        $(".dropdown-menu li").on("click", function() {
            Cars.search_table('', oTable); //clear filters
            $(this).parent().children('li').removeClass('active');
            $(this).addClass('active');
            $(this).parents(".input-group-btn").find('.btn').html(
                $(this).text() + " <span class='caret'></span>"
            );
            $("#searchbox").val('');
        });
        if ($('.datatables').length) {
            var oTable = $('.datatables').DataTable({
                "sDom": '<"top"l>rt<"bottom"ip><"clear">' //this explicitely excludes the default search
            });
            $("#searchbox").keyup(function() {
                Cars.search_table(this.value, oTable); //search column or whole table
            });
            $(function() {
                $('#search-info').popover(); //popover tip
            });
        }
    },
    search_table: function(search_string, table) {
        var column = $('#column_header').find(".active").val();
        if (column == -1) {
            table
                .search(search_string, true, true)  //regex and smart search true
                .draw();
        } else {
            table
                .column(column)
                .search(search_string, true, true)
                .draw();
        }
    }
};
$(function() {
    Cars.init();
});
{% endhighlight %}
