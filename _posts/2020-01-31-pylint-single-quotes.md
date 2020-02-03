---
layout: post
title: Use Pylint to enforce single quotes in Python 
---

Enforce single quotes for strings in Python

-----

I write Python these days and I use VS code for that. Before you enforce linting rules you should have the following plugin installed for VS code:

[Python extension for Visual Studio Code.](https://marketplace.visualstudio.com/items?itemName=ms-python.python) 

I run my code inside container, you can read more about it at [here.](https://code.visualstudio.com/docs/remote/remote-overview)

Your container or your environment has to have the [pylint](https://pypi.org/project/pylint/) and [pylint-quotes](https://pypi.org/project/pylint-quotes/) packages installed. You can install it via pip or you can have it in the requirements.txt file and your docker configuration can help install it. 

Add the following to your `.devcontainer.json` or `settings.json`:

```
	"python.linting.pylintArgs": [
		"--load-plugins", 
		"pylint_quotes"
	],
```

If all the dependecies are installed you should see single quotes warnings in VS code PROBLEMS tab. 