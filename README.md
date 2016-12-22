# Carousel for B12 coding challenge

> Carousel challenge from B12

In this project you'll find a basic [Vue.js](https://vuejs.org/) component called Carousel, which accepts an array of image urls.

## Reading the code

The important code in this project is within `./src/components/Carousel.vue`. This contains the entire Carousel component. You will also find sample data laid out in `./src/js/sample-images.js`.

## Viewing this project

Navigate to `./dist/` in your terminal, then run a webserver for this directory (for instance `python -m SimpleHTTPServer 8000` on a mac).  Then, point your browser to: `http://localhost:8000`.

If you want to run the project in a dev environment, follow the instructions for building at the bottom of this document.



## Tools used

* A [Vue.js template](https://github.com/adamvanlente/sass-webpack-vue-template) I've been experimenting with recently, which made it easy to get a production-ready app up in minutes
* This template comes with webpack/babel/sass baked in, so those tools are used here.  It also lints with slightly modified AirBnB standard.

## Things I didn't do

* Didn't put much into making it responsive for mobile.
* Didn't include a swipe library

## Requirements for project

* Use JS (ES2015 preferred)
* Modern framework (React, Angular)
* Be sure to explain expected data structure
* Use of build tools encouraged
* Include HTML/CSS needed for working example
* Include an HTML file at the root of project

## Properties of the Carousel

* For a given webpage, you can embed some JavaScript on that page and several images will display one after the other
* Left and right buttons let you advance through images.
* Indication of which image you are viewing, in context of full list
* (some carousels advance automatically)

## Data structure

Expected data structure for images is as follows:

    // Array with max length of 10 items
    [
      "imageUrl",
      "imageUrl",
      "imageUrl",
      ...
    ]

## Build this project

``` bash
# Prereq
npm install -g vue-cli

# install dependencies
npm install

# serve with hot reload at localhost:9898
npm run dev

```

For detailed explanation on how things work, checkout the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).
