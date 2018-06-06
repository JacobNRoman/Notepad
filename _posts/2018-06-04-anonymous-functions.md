---
layout: post
published: true
mathjax: true
featured: true
comments: false
title: Anonymous Functions
categories:
  - personal
  - software
tags: '"Anonymous Functions" "Lamba Expressions" "Android" "Java" "Javascript"'
---
## What's in a name?

Lately I've been working my way through the free version of Udacity's [Android Basics](https://www.udacity.com/course/android-basics-nanodegree-by-google--nd803) course. It's a great and extremely newbie friendly path to learning android dev, and I've had a lot of fun with it, although if you have coding experience you may find yourself skipping a lot of sections. In the past couple weeks I've encountered something I haven't really used before: ****Anonymous functions****. They are heavily used when writing/overriding many builtin Android methods, including OnClickListeners, which are used to respond to when a user taps on an element of your app (something you are going to do a lot). Here's an example from my code:

```java
TextView numbersView = (TextView) findViewById(R.id.numbers);
		
numbersView.setOnClickListener(new View.OnClickListener(){
		
	@Override
    	public void onClick(View view){
        	Intent intent = new Intent(MainActivity.this, NumbersActivity.class);
            startActivity(intent);
        }
    }
    );
```

First I find grab a specific TextView from within my xml layout, then I call the setOnClickListener method on that TextView passing in a function definition as a parameter.
        
This left me blinking the first time I encountered it. The instructor said that using an anonymous function like this was more concise than defining a function elsewhere to only use it once, and yes, sure, but it also seemed a lot less readable. Ultimately I just accepted it as a convention and have used it a few dozen times since. But it keeps eating at me. Exactly what are anonymous functions for? It can't just be concision. 

Well, maybe it is. [Oracle's Java Documentation](https://docs.oracle.com/javase/tutorial/java/javaOO/anonymousclasses.html) leads off with "Anonymous classes enable you to make your code more concise." Otherwise an anonymous class seems to be the same as a regular class, only without a name or constructor. Also, in order to further improve concision Java provides [Lambda Expressions](https://docs.oracle.com/javase/tutorial/java/javaOO/lambdaexpressions.html) for anonymous classes that only implement one method. 

So that solves that. But as I continued to code a new hitch came up. 

![declaredfinal.png]({{site.baseurl}}/images/declaredfinal.png)


Anonymous classes introduce scoping issues. When declaring an anonymous class you are often going to want to access a variable from within its containing class, and Java wants to ensure that you aren't going to mutate that variable by forcing you to declare it final. This wasn't particularly an issue in my case, as I was passing in an ArrayList which could be made final without changing its functionality or implementation at all, but its a limitation you should be aware of if you are going to be using anonymous classes. It also got me thinking why. 

Why any of this?

The answer lies in the [Functional Programming](https://en.wikipedia.org/wiki/Functional_programming) paradigm. A complete explanation of functional programming is way outside the scope of this article, but here are some of its basic aspects:

-Functional Programming approaches computatoin as a series of mathematical functions
-Inputs to functions cannot be mutated. Therefore, once a variable is defined, it cannot be re-assigned.
-Functions are often passed to other functions

So already some of these design decisions are starting to become less murky. Java wants us to declare variables passed to anonymous classes final in order to mimic, to some degree, the immutability at the core of functional programming. Anonymous classes weren't available in Java until Java 8, and are born from a different programming paradigm.

To get more granular, this is how Java implements [closures](https://en.wikipedia.org/wiki/Closure_(computer_programming)). A closure binds a function to the value of its variables at the time the closure is created, so that the function can access those values even outside of the scope they were defined in. [In Java](https://stackoverflow.com/questions/4732544/why-are-only-final-variables-accessible-in-anonymous-class), anonymous classes use their autogenerated constructor to copy in the value of any variables that that are used within that class.

Ultimately, using anonymous classes in Java seems to be primarily a question of design and style. A private inner class can always be used in the place of an anonymous class, so it's really what you think will be easier/more readable and how you want to handle managing scope and parameters. They are good to be aware of, and its useful to remember the broader concepts they embody. 
