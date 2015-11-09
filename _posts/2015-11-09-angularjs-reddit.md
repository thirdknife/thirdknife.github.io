---
layout: post
title: AngularJS and Reddit API
---

How to create a Gallery using AngularJS and Reddit API

-----

AngularJS helps to write javascript code in a structured way. Following will be guide how to use angular to consume reddit read only API and make a nice looking image gallery.

# [DEMO](http://thirdknife.github.io/reddit_pics)

Angular provides directives, which is a nice way to isolate code for a specific part of your DOM, let say you have a sidebar on your page then you create a directive and name it sidebar or what ever you want and just write your code for that part of the page.

For my case I am calling it pics. The HTML goes something like this

```
<div class="grid" ng-app="reddit" ng-controller="pics">
	<div class="grid__item" data-size="1280x857" ng-repeat="x in names | nsfw:this">
		<a href="{{ x.data.url }}" class="img-wrap"><img src="{{ x.data.url }}" alt="{{ x.data.title }}" imageonload>
			<div class="description description--grid">
				<h3>{{ x.data.title }}</h3>
			</div>
		</a>
	</div>
</div>
```

and the Javascript part goes something like this

```
app.controller('pics', function($scope, $http) {
	$http.get("https://www.reddit.com/r/pics/new.json?sort=new&limit=100")
	.success(function(response) {
		$scope.names = response.data.children;
	});
});
```

The HTML has to declare the `ng-controller="pics"` so that Angular attaches that part of the DOM with the directive. Once the application fires up it calls the reddit API to get the data. `$scope` is a function scope of your controller or a temporary store of your controller in which you can store data and share across your controller directives or filters. `$http` is to load a remote calling library for your controller, Angular works on the philosophy of dependency injector. As we are telling angular to load the scope and $http library it will load them for us after during execution.

Once the data is loaded, next part is to generate the images divs using a loop. Angular has a `ng-repeat` directive which acts as a for loop on javascript object or array. In our case we have variable called `names` attached to our scope so we iterate over it.

`ng-repeat="x in names | nsfw:this"` tell to iterate over names and put single value in x variable. The pipe symbols tells angular to pass in the filter for data manipulation. In my example I wanted not to show NSFW(not safe for work) images so had to pass in `nsfw` and `this` is used for sending in the scope to the filter.

The filter is defined as 

```
app.filter('nsfw', function() {
	return function(list) {
		if(!list){return};
		var result = [];
		for(i = 0; i < list.length; i++) {
			if(list[i].data.url.indexOf("i.imgur") > -1 && list[i].data.url.indexOf("gif") == -1  && list[i].data.thumbnail != 'nsfw'){
				result.push(list[i]);
			}
		}
		stack = result.length;
		return result;
		}
	});
```

This returns an array which will be used to render the page. I am putting some check here which includes that the image url must be starting with `i.imgur` and is not a gif as well as it should not be a `nsfw`

The biggest challenge was not to render until all the images are preloaded, otherwise it gave a bad ugly image clutter. For that I calculated the length of images which pass the filters and save it in the stack global variable and also a custom directive by the name of `imageonload` which basically binds the load event with the image it self and is being called when every time a single image is loaded. I was incrementing loaded image count inside a global variable by the name of `dataLength` and when they match the length of list passed by filter I call the create grid function `applyGrid` which keeps a nice user experience.

I have not created the gallery effect, I am using this from a demo at [Codrops page](http://tympanus.net/codrops/2015/10/15/effect-ideas-for-image-grids).


