---
layout: post
title:  "Getting started with Webpack."
date:   2015-10-22 17:02:03
categories: Posts 4
permalink: /getting-started-with-webpack
---

Welcome to 'Getting started with [Webpack]'. This will be a short series of posts about how to utilize an awesome build tool called [Webpack]. If you are familiar with tools such as Gulp or Grunt, you're going to love [Webpack] (I do).

What is it?
[Webpack] is not just another build tool for cleaning up your code before deployment, and then concatenating it all into one file (it can do this, very well) ready to be served up. [Webpack] will take your modules and any dependencies and generate static assets defining said modules. [Webpack] chunks your code and serves it up when it is needed. This can dramatically increase the load times of your applications. Something to note though, code splitting is an opt in feature which we will get into later in the series. For now I would just like to show you how easy it is to set up a development environment for front end development.

I won't go into the details of creating a new project directory, as this one will be very simple. However you will need [Node] installed on your system so we can install [Webpack] and other modules via npm (node package manager), there are plenty of good tutorials on the web to help with this.

From your new folder, perform an 'npm init' follow the instructions. Once this is done, install all the dependencies needed for this tutorial with this command. Optionally you can clone the repo from https://github.com/philipjc/webpack-blog-post.

{% highlight js%}
npm i -D webpack webpack-dev-server webpack-merge html-webpack-plugin node-libs-browser style-loader css-loader webpack-merge

{% endhighlight %}

Inside your project directory, create another folder named app (or whatever you like) and create a javascript file, I named mine main.js. Also, create another folder inside app to represent your styles and create a .css file, might as well give the page some colour!

{% highlight js%}
Project dir
  /app
    main.js
    /styles
      -main.css

{% endhighlight %}

Place this code inside your main.js. Just some simple DOM manipulation to add a h1 element to the page.
{% highlight js%}

function () {
  var header = document.createElement('h1');
  header.innerHTML = 'Welcome to Webpack';
  return header;
}

var body = document.querySelector('body');
var header = document.createElement('div');

header.className = 'header';
document.body.appendChild(header);

var headerElement = document.querySelector('.header');
headerElement.appendChild(Header());

{% endhighlight %}

Now we have these things set up, it's time to add [Webpack]. Inside your Project directory along with node_modules and the package.json, create a file named webpack.config.js. At the top of this file we will include the needed dependencies, so using commonJS syntax;

{% highlight js%}

var path = require('path');
var HtmlwebpackPlugin = require('html-webpack-plugin');
var webpack = require('webpack');
var TARGET = process.env.npm_lifecycle_event;
var ROOT_PATH = path.resolve(__dirname);

{% endhighlight %}

I will go into more detail about the other modules as and when we use them incase you are not familiar.
We define [Webpack] with a simple object structure. Inside this we include what is known as loaders. If we want to process our code through [Babel] for example for ES6 transpiling or preprocess our CSS into [Sass] we run it through a loader.
Another nice benifit of [Webpack] is its development server.

{% highlight js%}

var path = require('path');
var HtmlwebpackPlugin = require('html-webpack-plugin');
var webpack = require('webpack');

var ROOT_PATH = path.resolve(__dirname);

module.exports = {
  entry: path.resolve(ROOT_PATH, 'app/main.js'),
  output: {
    path: path.resolve(ROOT_PATH, 'build'),
    filename: 'bundle.js'
  },

  module: {
    loaders: [
      { test: /\.css$/, loaders: ['style', 'css'], include: path.resolve(ROOT_PATH, 'app') }
    ]
  },

  devServer: {
    port: 4000,
    colors: true,
    historyApiFallback: true,
    hot: true,
    inline: true,
    progress: true
  },

  plugins: [
    new HtmlwebpackPlugin({ title: 'webpack' }),
    new webpack.HotModuleReplacementPlugin()
  ]
};

{% endhighlight %}

From the top. We require path so we can handle transforming file paths rather than using __dirname.
We require the html-webpack-plugin, this will automatically create our index.html with a html5 doc and place our bundle.js file in it. Then of course require webpack its self. Following this, save our root path utilising the 'path' module, into a variable named ROOT_PATH to use in the code.

we use module.exports so the CommonJS module system knows what to pass to the outside world.
The entry property shows the path of the files we want [Webpack] to run over, here we have specified app/main.js, but to cover an entire app just remove the main.js.
The output property describes the path to place a directory name build, to store a file named bundle.js that references all of our packaged modules.

{% highlight js%}

module: {
  loaders: [
    { test: /\.css$/, loaders: ['style', 'css'], include: path.resolve(ROOT_PATH, 'app') }
  ]
},

{% endhighlight %}

This is the loaders we pass our code through. Currently we are just processing styles, but in future posts I will extend this to process ES2015 using [Babel], [Sass] and more. I'll describe each property.
test, is a regular expression to match file extensions.
loaders, are the module you can install as dev dependencies to run your code through.
include, is all the file path to your styles. You can also ass an include property to ignore directories.


{% highlight js%}

devServer: {
  port: 4000,
  colors: true,
  historyApiFallback: true,
  hot: true,
  inline: true,
  progress: true
},

{% endhighlight %}

Here is the Dev server that is very useful. This is a small [Node] server using [Express] framework. It uses the [Webpack] middleware to serve the [Webpack] bundles.

{% highlight js%}

plugins: [
  new HtmlwebpackPlugin({ title: 'webpack' }),
  new webpack.HotModuleReplacementPlugin()
]

{% endhighlight %}

Thanks for reading and I hope you find as much enjoyment with using [Webpack] as I do.
Find the complete Source code on Github: https://github.com/philipjc/webpack-blog-post


[Webpack]: http://webpack.github.io/
[html-webpack-plugin]: https://www.npmjs.com/package/html-webpack-plugin
[Node]: https://nodejs.org/en/
[Express]: http://expressjs.com/
[Babel]: https://babeljs.io/
[Sass]: http://sass-lang.com/
