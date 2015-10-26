#The pomodoro clock project
This post is my solution for the Basic Front End Development Project from FreeCodeCamp [Build a Pomodoro Clock](http://www.freecodecamp.com/challenges/zipline-build-a-pomodoro-clock). This is a walk through of the steps I made and decitions I took during its development. The aim of this post is helping other fellow campers, give ideas and, overall, serve as a log to myself.

##Making our clock ticking
Isn't strange that the first thing I thought about when I was aproaching this exercise was how to decrease continuosly a variable each second. For that purpose I made a research and I found out these 2 functions:
```javascript
setTimeout(function, miliseconds);
```
and

```javascript
setInterval(function, miliseconds);
```

[setTimeout()](1) *executes once* the function passed as first parameter after the time setted in milisecond as the second parameter.
On the other hand,[setInterval()](2) executes the function every x miliseconds, just what I needed.

```javascript
var time = 30;

setInterval(function(){
 if(time >= 0){
  time--;
 }
},1000);
```
With this code, as soon as the page loaded the timer started a countdown. However I needed to be able to choose when the timer should start and when it should stop.

The way of stopping an interval is by calling the function `clearInterval()` which takes as a parameter the function name where the `setInterval()`is defined. [Further reading](1)

```javascript
var time = 30

//The interval starts
var timer = setInterval(function(){
 if(time >= 0){
  time--;
 }
},1000);

//The interval stops
clearInterval(timer);
```
###The *this* problem

Once I had the timer ticking, the point was to be able to start it and stop it with the user interaction. I could just have had some functions and a global variable for the time. However, due to my Java experience for my last year of vocational training, I have this mindset of having everything encapsulated inside objects. Thus, I came with this structure for the timer:

```javascript
var timer = {
  time: null,         //the time set by the user
  timeHandler: null,  // variable which hosts the setTimer func

  startTimer: function(){},

  stopTimer: function() {}
  };

  //Controls using jQuery
  $('#start').click(function(){timer.startTimer();})
  $('#stop').click(function(){timer.stopTimer();})
}
```
 At this point I had my first big trouble and was related with the scope and the *this* keyword. This was my first attempt to implement the startTimer method

 ```javascript
var timer = {
  //...

  startTimer: function(){
    this.timeHandler = setInterval(function(){
      this.time--;
    }, 1000);
  },

  stopTimer : function() {
    clearIterval(this.timeHandler);
  }
}
 ```



[1](http://www.codenewbie.org/podcast/the-pragmatic-programmer-part-ii)
[2](http://www.w3schools.com/jsref/met_win_setinterval.asp)
