## What JavaScript libraries do you have experience of using? What is your preferred toolset for client-side development?

###Libraries

*   jQuery suite ( jQuery, jQuery-UI, jQuery-Mobile ) / Zepto
*   underscoreJS / Lo-Dash
*   backboneJS
*   requireJS
*   modernizr
*   mustacheJS
*   ( plus: amchart, ammap, three.js and jQuery plugins ...)

###client-side development:

*   grunt.js => task runner application running with node.js
*   jQuery DOM manipulation
*   backboneJS / underscoreJS => MVC pattern in javascript
*   requireJS => OO javascript ( create modules to be loaded when needed only)
*   modernizr => default testing in order to define what capabilities the browser can handle before using something we know it won't ( touch event / css animations / canvas / local storage ...)

## Can you provide examples of where you have formatted mark-up for use with third party templating and page generation systems? What challenges did this pose and how did you overcome these?

###Templating code example:

**IPD Infographic - underscore templating system with backbone views**

    <div class="benchmark-infografic">
    	<p>Benchmark Template</p>
    		<% _.each(list, function(item){ %>
    	<div>
    	<p class="title"> <%= item.title %> </p>
     	<% if( item.portfolio === ""){ %>
    		<p class="val"> <%= item.val %> </p>
      	<% }else{ %>
        	<p class="portfolio">
          		<span>Portfolio :</span>
          		<span> <%= item.portfolio %> </span>
        	</p>
        	<p class="benchmark">
          		<span>Benchmark :</span>
          		<span> <%= item.benchmark %> </span>
        	</p>
        	<p class="relative">
          		<span>Relative :</span>
          		<span> <%= item.relative %> </span>
       		</p>
      	<% } %>
    
    	</div>
    	<% }) %>
    </div>

**Backbone View: BenchmarkView.js**

    define([
		// These are path alias that we configured in our bootstrap
  		'jquery',
  		'underscore',
  		'backbone',
  		'text!templates/benchmarkViewTemplate.html'
	], function($, _, Backbone, benchmarkViewTemplate){


  	var BenchmarkView = Backbone.View.extend({

		el: $(".flow-chart"),

		initialize: function( list ){
			this.render( list );
		},

		render: function( list ){
			// console.log( elem );
			var compiledTemplate = _.template( benchmarkViewTemplate, {list : list } );

			// Append our compiled template to this Views
			this.$el.append( compiledTemplate );
			this.switchDataListener();

			return this;
		},
		switchDataListener : function( ){
			$(".period").on("click", this.switchData );
		},
		switchData : function( e ){
			var btnClassName = e.currentTarget.className,
				btnList = $(".period"),
				btn = e.currentTarget;
			// Check if not displaying the data of this tab
			if( btnClassName.indexOf("is-active") === -1){
		  		console.log( "btn TEXT = ", $(btn).text());
		  	// Update data
		  		if( btnClassName.indexOf("period-1") !== -1){

		  		}else if( btnClassName.indexOf("period-2") !== -1){

		  		}else if( btnClassName.indexOf("period-3") !== -1){

			}else{
				//ERROR!
			}

			// Change the tab active flag
			btnList.each( function( elem ){
				$(btnList[elem]).removeClass('is-active');
			});
			$(btn).addClass('is-active');
		}

	});
	
	return BenchmarkView;
	
	});

The main issues are originally the **model definition**. Being sure that the model ( defined in javascript object or JSON ) will match the client needs and the display requirement ( active states, types of value ). The solution was to test data-sets and user behaviours.
  Next step will be to automatically include Test-driven development ( [TDD][1] ) or at least Behavior-driven development ( [BDD][2] ) but for javascript **JavaScript Unit Testing** using *QUnit / Jasmine / Mocha* ...

Also having **deep nested elements** are more difficult to handle. The easiest way to avoid complexity is to create multiple entry points and split data into smaller pieces.
Here, again, TDD / BDD will help.

[1]:http://en.wikipedia.org/wiki/Test-driven_development
[2]:http://en.wikipedia.org/wiki/Behavior_driven_development

## Do you have any experience of integrating your designs with third party web site engines? If so please describe the experience that most closely relates to this project?

### Third party services:

* Flickr
* Twitter
* Wordpress
* LinkedIn
* Facebook
* Google maps
* ...

In almost every projects I came accross one of them to integrate.

For their default behaviour, it's somewhat easy, or straight forward, **their API are easy to use**.
The complication come when styling these elements are an absolute necessity.
Most of the time we're provided with elements like iframes on which **we don't have our hands on**, or we're restricted because of **"Cross Domain Policy"**. But the most frustrating part is when the service's provider just explicitelly don't allow you to do what you want ( see twitter new embedded [policy][3])

>
>  Embedded timelines are available in light and dark themes for customization
>


Another difficulty is to **keep up with the new version of API and requirement** ( ie: skyron twitter embedded time-line that didn't work for a while because we didn't update it when it was required by twitter).

[3]:https://dev.twitter.com/docs/embedded-timelines



## Describe how you would minimise the load time of pages to users.

How works a browser: http://www.html5rocks.com/en/tutorials/internals/howbrowserswork/

Mainly, a browser does:

  Networking HTTP Requests,
  Render HTML,
  JavaScript interpreter,
  Data storage.

Minimising the load time can be achieve with all the above leverages.

* First of all **minify all our ressources** served ( HTML / CSS / IMG / JS ) knowing that > 50% of ressources on the web are images, using the right tool is crutial ( http://addyosmani.com/blog/image-optimization-tools/ ).

* Browser rendering is synchronous, loading javascript in the header, adding Script tags within the content => **the browser has to wait to execute all the js before going further**.

* Every time the browser find a src attribute ( script, images ) it has to load the ressource before rendering. **Thinking of decorative images** ( social icons, repetitive images...) beforehand and create **a sprite or icon-font** ( all icons on the same image, the request will be sent once, then served from the cache only ).

* For the same reasons, **limiting the number of request to the server** by concatenate stylesheets or javascripts( a browser can handle only an average of 6 requests at a time, so asking for 25 ressources for the page to be complete doesn't make any sense ).

* **Loading responsively** => if I am on a mobile, why serve a retina image for a background that wont be displayed? Or loading modules that are not required by this page ( example: skyron home page and the mini portfolio on mobile views )!

* **Caching the ressources** that we know won't change often ( disclaimers, site-map, static html ... ) using a meta tag cache-control ( http://www.mobify.com/blog/beginners-guide-to-http-cache-headers/ ) or creating a cache manifest file (http://www.html5rocks.com/en/tutorials/appcache/beginner/). now we also can use session-storage or local-storage, avoiding to send a cookie with every requests ( added at the end of img URI, html pages... ) which require more processing.






