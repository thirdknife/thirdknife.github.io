---
layout: post
title: Integrating Dropbox Chooser in Meteor Application
---

This blog post explain how to integrate Dropbox handy tool called Chooser into your Meteor Application.

-----

Before starting out, to integrate Chooser into your application you need to create an application using this [link](https://www.dropbox.com/developers/apps/create). If you are working on you project locally you can add localhost as address in dropbox application otherwise use the live url.

Once above step is done you need to add this markup into your head tag.

	<script type="text/javascript" src="https://www.dropbox.com/static/api/2/dropins.js" id="dropboxjs" data-app-key="YOUR_APP_KEY"></script>

Create a template by any name. I am calling it dropbox, and add a button to your template markup.
	
	<template name="dropbox">
	  <button type="button" id="dropbox" class="btn btn-default">Click here to Choose Dropbox Files</button>
	</template>

In your client javascript file(in most cases it's project_name.js) register events for the dropbox template as
 
	Template.dropbox.events({
		'click #dropbox' : function(e){
			e.preventDefault();
			Dropbox.choose({
				linkType: "direct",
				multiselect: false,
				success: function(files) {
					Session.set('files',files);
				},
				cancel:  function() {}
			});
		}	
	});

Once done if you click the button it will open Dropbox Chooser dialog box which allows to choose single file (if multiselect is false) or multiple files. Once choosen it will return a JSON based response, writing success callback you can maipulate the data however you want to. I prefered to save it using Session global object on the client. Once stored in Session you can use it to render it using other template.


