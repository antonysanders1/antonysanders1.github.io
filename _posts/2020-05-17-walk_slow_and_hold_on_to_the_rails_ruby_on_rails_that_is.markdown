---
layout: post
title:      "Walk Slow and Hold on to the Rails, Ruby On Rails that is…"
date:       2020-05-17 14:28:15 +0000
permalink:  walk_slow_and_hold_on_to_the_rails_ruby_on_rails_that_is
---


When being introduced to Rails I was so intrigued to hear about "the magic of Rails". If you don't know about what that is, let me clear that right up. Consider "Rails magic" to be like a bunch of awesome macros and other syntactic short cuts - short code snippets that you write that prevent you from having to write a ton of lines of code to get the same functionality. You can even build entire websites with just a few lines of migration code, but it's generally not the best practice. While working on my Rails project I learned that building a great dynamic website is a tedious task, and as a developing rubyist, here are a few things I learned.

**Planning is everything** - Most dynamic websites have multiple models(objects) that have relationships with other models (i.e Users have many Blog Posts). Both the user and blog posts are considered objects with database relationships called Active Record Relationships. Now it's easy to think about how we might set this up this simple relationship, but what happens when you add one, two or even three models to your project? It can get hectic and confusing fast when thinking about how all the objects could relate to one another. This is why planning ahead can help keep you from making mistakes or working backwards. I suggest that writing out not only your idea but how the objects/models will relate. Below is an example of my Models' Active Record Relationships:

* **User:**
*    has_many :items
*    has_many :stores through: :items
* 
* **Item**:
*    belongs_to :user
*    belongs_to :store
* 
* **Store:**
*    has_many :items
*    has_many :users through: :items

**How to W.I.N** - Whether it's an error or area of interest, it's easy to get distracted when developing a project. If working in my Rails project has taught me nothing else, it's that focusing on the tasks at hand is the most important, and that's kind of a struggle when you find out how robust rails is. That why I refer to W.I.N. - a personal acronym for "What's Important Now". When focusing on what the most important features or tasks are, you are sure to "win" and achieve your goal more efficiently. 

**When you're in a bind** - When learning first ruby, I came in contact with a very helpful debugging tool called pry.  Its almost inevitable that when coding you'll hit a snag, and pry is a good tool to help you see into the situation. Inserting "binding.pry" into your code allows you to stop your code at that certain point so that you can run tests for a better understanding on why your code may be breaking. Pry won't solve every issue, but luckily Rails also has extensive guides to help steer you in the right direction if you're not quite sure how to make something work. The Rails community is also vast, so you can even at times rely on the help from other rubyists.

There were many learning curves when building my project, however it was one of the most rewarding things i've done so far relating to software engineering, and needless to say I did get to see some of that rails magic up close. Some of my favorite things to learn and do were generating migrations *(which can create databases, MVCs, and even entire websites with just a few lines of code in the terminal)*, discovering new gems, and setting up third party authenticators like Google and Facebook. I look forward to creating more apps/sites with ruby in the future and growing as a software engineer.

**<% end %>**

Thanks for reading,
Antony Sanders

