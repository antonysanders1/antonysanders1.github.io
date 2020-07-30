---
layout: post
title:      "Learning to Love The pain... of JavaScript"
date:       2020-07-17 04:00:23 -0400
permalink:  learning_to_love_the_pain_of_javascript
---


After completing the fourth *(and in my opinion, most difficult)* module that covers an in-depth look at Javascript here at Flatiron, I had to learned to love the pain of JS.  There was such a learning curve coming from Mod 3 where we previously learned Ruby on Rails - it was dreadful.  After making just about every mistake possible, it finally started to get better, but it wasn't until I was halfway through my final JS project when I actually loved to learn the pain of JS.

JavaScript is one of the most popular and widely used programming languages in the world, and for good reason. It's  used both on the client-side and server-side allowing you to make web pages interactive. Where HTML and CSS are languages that give structure and style to web pages, JS gives web pages interactive elements that helps engage a user. This is what makes JavaScript so exciting to me -- I was finally seeing how interactive you can make a webpage without a single page reload!

**My Project**

At first I was struggling with a concept for my project because the focus was mainly to create a single page web application that retreives data from an API *(which we also had to build)*, and make interactive updates to the Document Object Model *(or DOM)* without reloading the webpage. With fewer requirements than my previous projects, my mind was running wild with possibilities. It was important to me not to overthink the project attempting to make something really cool but probably extremely difficult (as I usually do). Choosing a passion was also as equally important and after brainstorming for a few days I landed on the Idea of creating a web app that allows aspiring song writers to share their lyrics with the song-writing community, recieve and give feedback.

**Getting Started**

Getting started with my JavaScript project was definitly the most intimidating part for me -- its like giving an artist a blank slab and telling him to return with a masterpiece. The question always asked is "Where should I begin?".. My first coarse of action was create the main folder for the project, then creating two directories within it that would house my frontend (all of the HTML, CSS, and JS) and backend (a Rails API). I first created the frontend folder, as the backend-api folder would need to be generated with:
```
$ rails new backend_api --api
```
This will auto populate most of what you'll need to create your API. After adding in the Cross-Origin Recource Sharing & Active Model Serializers gems, setting up your database, models and controllers, your API is ready to use!

Next I created a GitHub repository that I would push and house all of my final code changes.  I learned the hard way that you have to be careful when pushing to GitHub because if you push to GH from an inner directory (i.e the backend_api folder), you'll endup creating a locked repo inside your main repo and that you can't access and it's a headache to fix. 

**The Meat and Potatoes**

Once the preliminary stuff was all setup and out of the way, It was time to start working on the most imprtant part, the frontend. Within the Frontend I needed to create the `index.html` file for document structure, a `styles.css` file for *(you guessed it)* styling, and all of the script files *(Javascript)* like `index.js` that will and work with both the HTML and CSS files to handle visibilty, styling, and interactivity. 

`Note: Its best practice to place all of these files in their respective directories and sub directories to help seperate concerns. Below is a quick look at my file tree which could be deemed a recommended structure:`
```
˅ frontend
|  ˅ src
|  |  ˅ adapters
|  |  |  api_adapters.js
|  |  ˅ components
|  |  |  comments.js
|  |  |  songs.js
|  |  ˅ index.js
|  ˅ styles
|  |  style.css
|  index.html
|  show_clear.js
```

Another awesome thing that javascript can do is make fetch requests to the API. *Now we're actually connecting our frontend and back end!* Fetch requests are asynchronous functions that allow us to make GET, POST, UPDATE, and DELETE requests to our backend API. In simple talk this means that we can either get or edit the data from out API and later do something else with it like diplay it in the DOM. All asynchronous means is that while our request is running in the background *(because getting a lot of data can take some time)*, the rest of our code can still execute to help decrease load times. This is possible because fetch requests always return a promise, *meaning once the data has been retrieved or "resolved"* 

`Note: you always get something back whether it be a stream of data or an error status.` 

Chaining `.then()` methods onto our fetch requests allow us to move on and *then* handle expected responses by looking at the soon to be resolved promise, and *then* convert that expected stream of data into an object. The chained `.then()` methods will then execute once the promise has resloved. Now we can use these objects to update the DOM!

`Note: Its best practices that you ensure that your js fils are handling only one thing. For example DOM related functions, class functions and fetch requests should be handled in three seperate js files.`

It was interesting, thrilling, and even frustrating at times when making chages to the DOM via my JS files. I quickly learned that you have to be very calculated with what you decide to manipulate because it could affect how the entire web page looks and/or operates -- and the more functional you want your app, the more calculated to have to be. There's nothing worse than having tiny bugs in your code and not being able to find and debug them easily. Once you've got everything working, it's now a great idea to DRY up your code, removing any duplicate code to make cleaner synatx.

**In Walks Debugging**

If its one thing you'll need as a fresh JS developer it's the good ol' `console.log()`. The console.log is a great tool to assist developers debug their code. If something isn't quite working the way you want, you can add console.log() between lines of code where you think you have an error. You then pass in whatever it is you wish inside the console.og parenthesis *(usually the thing you're trying to debug)*. I can say that logging to the console definitly got me through to the end of my project.

**Conclusion**

For as simple as this project's requirements were, It came with so much to cover - so much that I could turn this 5 minute read into a 20 minute read, but for the sake of your time, I won't do that. I am however incredibly appreciative for this module and everything I learned about JS, even though there were bumps and bruises along the way. I learned how to create an API, fetch data from it, use JS functions to manipulate the DOM, and seperate concerns -- Now I'm that much closer to being a full stack developer!


