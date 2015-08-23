+++
date = "2015-07-16T22:13:36-07:00"
title = "Everything is a Hammer"
HeroImage = "Powershell"
categories = ["Powershell", "Learning"]
tags = ["Powershell"]
url = "/post/2015/07/16/everything-is-a-hammer"
+++

# Where Everything is a Hammer test

When I was teaching a seminar on the use of Powershell to some first time developers. One of the students in the class, asked me if there was anything that could be done to solve a problem he had. Basically he had a problem to where he needed to scrape content from a website and make it available to another process for ingestion into their CMS.

The way he tackled the solution was to create a C# console line and application and use something like RegEx or HTML Agility pack. While the solution did work. It proved to the programmer to be very time consuming and labor intensive. And I imagine, it was also time consuming, in the time he spent cycling ideas to get to the solution.

A common thing I see, at least in my local community in the Microsoft camp is to learn one language and try to use that language everywhere. In our industry this tends to be known as the "Hammer Fallacy".

![I Got that nail](https://d3ui957tjb5bqd.cloudfront.net/images/screenshots/products/7/79/79359/hammer-f.jpg?1393432661)

By the way, this isn't unique to green horn programmers. Many a people in the development camps I frequently visit, always try to solve a solution in their tool of choice. That is why it is rare at least in my community to see people present on things like Python, Go, PowerShell or Ruby. They have built up thier prejeduices by what other people have said.

Anwyway, since I was teaching Powershell the student asked me if there was a way that we could solve that problem with PowerAhell. At this point in the seminar I showed some basic stuff like. Branching, Loop Structures, Redirecting Output Buffers, Piping and how to get help.

While, I knew in my head that there was one, we went down the road and had the group of 5 students assemble and work as a team.

![Team Work in action](http://www.dragonbridgecapital.com/wp-content/uploads/2013/03/Teamwork.jpg)

They brought together a mob programming group, and I had them think about the problem. I got to say it's very rewarding when you can set a task and see a group not of individuals, but of team members. Give it the ol' college try. Anyway, they were able to come up with a solution in about 15 mins, with some minor proctoring from me. But the student who asked the question came to a realization that it would of been much simpler to do it in PowerShell.

The final result was **literly one line of code**. It did it well enough, and fast enough. And makes me realize how in the community that I am part of that most members will reach for that hammer and try to solve a problem that might best well be attempted in another language or high level tool or low level tool for that mater. And hold on and clutch that hammer. By the way, I am not judging those who do. I was once one of those people too. I would literally try to do all my work in C#. It is when you experience the amount of time that can be saved by using a language more appropriate for a domain it makes you start to rethink your approach.

![Good Enough](http://media.tumblr.com/548714fc3ed7a99c8e680a704b48e45d/tumblr_inline_ne9gmaSSZz1qersu1.jpg)

Even, if the solution has to be coded in your tool of choice because of tool constraints or performance constraints. You can still do some exploratory programming with another language and see if you can grok the problem, by using a highlevel language. For those curious here was the final solution we came up to as a group.

This was the first line of code I gave them.
```
  $rawscrape = Invoke-WebRequest "http://www.slidemeister.com/forums/index.php?topic=6985.0"
```

They were able to come up with this to get to the rest of the solution.

```
  $rawscrape.Links | select href | where href -Like *www.youtube.com*
```

The final solution is this.

```
  Invoke-WebRequest "http://www.slidemeister.com/forums/index.php?topic=6985.0" | $rawscrape.Links | select href -unique | where href -Like *www.youtube.com*
```
