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

[setTimeout()](http://www.w3schools.com/jsref/met_win_settimeout.asp) *executes once* the function passed as first parameter after the time setted in milisecond as the second parameter.
On the other hand,[setInterval()](http://www.w3schools.com/jsref/met_win_setinterval.asp) executes the function every x miliseconds, just what I needed.

```javascript
var time = 30;

setInterval(function(){
  time--;
},1000)
```
With this code, as soon as the page loaded the timer started a countdown