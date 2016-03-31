**I'm awaiting Ghost's Apps/Plugins which is currently in their backlog to resolve some major issues GhostHunter has with Ghost. Please only use this if you have a small blog.**

#GhostHunter
A Ghost blog search engine

==============

GhostHunter allows any theme developer to add search capabilities right in their blog without having to resort to any third-party solutions. 

GhostHunter has at its core [lunr.js](http://lunrjs.com). And thanks to this powerful search engine, GhostHunter provides full text searching.

Although GhostHunter is designed specifically for the Ghost blogging platform, it will work on any system which uses an rss feed.

If you find it useful, I'd love to hear how you used it, what challenges there were and how I could make it better: jamal@i11u.me

Add little features such as max length of description and category separator by @dreampiggy

##Usage

After including jQuery, include ghostHunter:

```html
<script src="js/jquery.ghostHunter.min.js"></script>
```
  
In your theme you'll need an `<input>` for the search query, the input should be in a `<form>` and you can use the form's submit functionality to call the search. This will work whether you have a standard submit button or if you use submit() in javascript.

```html
<form>
  <input id="search-field" />
  <input type="submit" value="search">
</form>
```

You'll also need an area for the search results to show up:

```html
<section id="results"></section>
```
   
Now we can turn on the plugin, and that's all there is to it:

```js
$("#search-field").ghostHunter({
  results   : "#results"
});
```

##How it works

GhostHunter will attach itself to your search input field and wait for it to be focused on. Once focus has gone to the field, the engine quickly gets to work building an index of your rss feed that can easily be searched. When your visitor submits the form, GhostHunter will use either a default template or one provided by you to fill in your results element.

##Advanced features

GhostHunter can be customized to a certain extent using very simple templating. 

If you'd like to customize the html of the results there are two options:

###Having search results appear on key up

You can have the search results appear "as you type". Simply pass the onKeyUp parameter as true

	$("#search-field").ghostHunter({
		results   		: "#results",
		onKeyUp 		: true
    });

###Adding callbacks

You can have Ghost Hunter call your callback funciton at two points. The first is right before it renders the information onto the page using the "before" option:

```js
$("#search-field").ghostHunter({
	results   		: "#results",
	before 			: function(){
		alert("results are about to be rendered");
	}
});
```

The other callback is "onComplete" and gets called when the results have been rendered. It also provides an array of all the results:

```js
$("#search-field").ghostHunter({
	results   		: "#results",
	onComplete		: function( results ){
		console.log( results );
		alert("results have been rendered");
	}
});
```

###Clearing the results

You can use ghostHunter to clear the results of your query. ghostHunter will return an object relating to your search field and you can use that object to clear results.

```js
var searchField = 	$("#search-field").ghostHunter({
						results   		: "#results",
						onKeyUp 		: true
				    });
```

Now that the object is available to your code you can call it any time to clear your results:

```js
searchField.clear();
```

###Hiding the search info

If you don't want to show the search info at all you can pass this option

```js
$("#search-field").ghostHunter({
	results   			: "#results",
	displaySearchInfo 	: false
});
```

###Hiding the search info when the result is zero

If you don't want to show the search info when the results are zero you can pass this option

```js
$("#search-field").ghostHunter({
	results   			: "#results",
	zeroResultsInfo 	: false
});
```

###Set max length of description and category separator

If you want to limit the length of your post description from the search result, or set the seperator between multiple category (tags) you can pass these options

```js
$('#search-field').ghostHunter({
    results: '#search_results',
    infoMaxLength: 120,
    categorySeparator: "&nbsp;"
});
```

###Customizing the html template

The **result template** has access to these variables: `title`, `description`, `link`, `pubDate`, `category`.

If you'd like to create your own you can use double curly brackets and pass the "results_template" option:

```js
$("#search-field").ghostHunter({
	results   		: "#results",
	result_template : "<a href='{{link}}'><p><h2>{{title}}</h2>{{description}}</p><p><h4>{{pubDate}}</h4><h4>Tags: {{category}}</h4></p></a>"
});
```

The **info template** has one variable: amount.

If you would like to modify the wording at the top, or have it say nothing you'll need to pass in the "info_template":

```js
$("#search-field").ghostHunter({
	results   		: "#results",
	info_template	: "<p>Number of posts found: {{amount}}</p>"
});
```

And of course, both can be included together:

```js
$("#search-field").ghostHunter({
	results   		: "#results",
	info_template	: "<p>Number of posts found: {{amount}}</p>",
	result_template : "<a href='{{link}}'><p><h2>{{title}}</h2>{{description}}</p><p><h4>{{pubDate}}</h4><h4>Tags: {{category}}</h4></p></a>"
});
```

If the rss feed on your website is different than a standard ghost installation `\rss` you can specify that in the options:

```js
$("#search-field").ghostHunter({
	results   		: "#results",
	rss 			: "/path/to/rss.xml"
});
```