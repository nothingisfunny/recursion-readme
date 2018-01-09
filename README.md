# Recursion

## Objectives

* Learn terminology like base case and recursive call  
* Understand how the computer executes a recursive function
* Develop a technique for deriving recursive solution

### Introduction and Terminology

Recursive functions are functions that call themselves.  

For example the function below is recursive:

```js

function sayDownFrom(n){
  console.log(n)
  if(n > 1){
    sayDownFrom(n -1) // recursive call
  } else {
    return true // base case
  }
}

sayDownFrom(5)
// 5
// 4
// 3
// 2
// 1
```

You can see that in the body of the function, that the function calls itself.  That part of the function is called the **recursive call**.  You can also see that the function has a stopping point, which we call the **base case**.  If there were no stopping point, the function would just keep going on forever.  The function sayDownFrom would blow right past the number one and begin printing negative numbers.

We teach recursion for two reasons.  First, you will find your brain thinking of recursive solutions, so then being able to translate this thinking into code is an important tool.  Second, some popular algorithms that you will learn later on have recursive solutions, so we want you to be able to understand these common recursive solutions to various problems.

### An Initial Problem

Let's consider a function called sumUpToFive that adds up all of the numbers up to five.  To be explicit, it would look like the following.

```js
function sumUpToFive(){
  return (1 + 2 + 3 + 4 + 5)
}

```

which equals

```js
function sumUpToFive(){
  return (1 + 2 + 3 + 4) + 5
}
```

Consider that set of parentheses around one and four.  Isn't that really the same thing as sum up to four?  This implies that we can rewrite sumUpToFive as the following:

```js
function sumUpToFive(){
  return sumUpToFour + 5
}

function sumUpToFour(){
  return (1 + 2 + 3) + 4
  // note (1 + 2 + 3) is the same as sumUpToThree
}
```

At this point, if we asked you to write a function called, sumUpTo(n), you may agree that the we can say the definition of the sum up to a number n, is equal to us adding together

  * sum up to n -1
  * and n

Right? Take a look.

```js
function sumUpToFive(){
  return sumUpToFour + 5
}

function sumUpToFour(){
  return sumUpToThree + 4

}

function sumUpToThree(){
  return sumUpToTwo + 3
}

function sumUpToTwo(){
  return (sumUpToOne + 2) 
}

function sumUpToOne(){
	return 1
}
```

If that's true, then we can rewrite our function sumUpTo(n) as the following:

```js
  function sumUpTo(n){
    sumUpTo(n - 1) + n
  }

```
However if just keep the function above, we will call this function forever.  Let's stop when n is one.  As you see above, the sum up to the number one is one, so we can just write our function as the following:

```js
function sumUpTo(n){
  if(n > 1){
     return sumUpTo(n - 1) + n
  } else {
    return 1
  }
}
```

### How the computer executes a recursion

Let's see what happens if we pass the number five into this function.

```js

function sumUpTo(n){
  if(n > 1){
    return sumUpTo(n - 1) + n
  } else {
    return 1
  }
}

sumUpTo(5)
  // this translates to
  sumUpTo(4) + 5
    // then JavaScript translates sumUpTo(4) as
    sumUpTo(3) + 4
      sumUpTo(2) + 3
        sumUpTo(1) + 2
          // 1
```
So at this point, all we showed is JavaScript breaking down, or reinterpreting our function.  What is the sumUpTo(5)?  Well it's the sumUpTo(4) + 5.  What is the sumUpTo(4)?  It's the sumUpTo(3) + 4.  All of these sumUpTo are unsolved puzzles for JavaScript until we get to sumUpTo(1).  Our function says that sumUpTo(1) returns 1 (by virtue of the `else` block.  

So now Javscript can start filling in these function calls.  Because sumUpTo(1) equals one, and sumUpTo(2) equals sumUpTo(1) + 2 the sumUpTo(2) equals 3.  In other words JavaScript performs the following.   

```js
    // 1
  sumUpTo(1) + 2 // 3
    sumUpTo(2) + 3 // 6
      sumUpTo(3) + 4  // 10
        sumUpTo(4) + 5 // 15
          sumUpTo(5)  // 15
```

So when we write a function like

```js
function sumUpTo(n){
  if(n > 1){
    return sumUpTo(n - 1) + n
  } else {
    return 1
  }
}
```
There are really two steps involved.  First, JavaScript repeatedly calls the sumUpTo function until it reaches the stopping point (the base case), and then once it hits the base case, it can begin to resolve the other function calls.  Believe it or not, you've seen this process before.  You've seen it every time a function calls another function.  For example,

```js
  function sumUpToFive(){
    sumUpToFour + 1
  }

  function sumUpToFour(){
    1 + 2 + 3 + 4
  }
```

In the above code, for JavaScript to evaluate the function call sumUpToFive it must first evaluate sumUpToFour.  Just like in our recursive solution to evaluate sumUpTo(5) it must first evaluate sumUpTo(4).  

So JavaScript is in the middle of all of these function calls until the base case is resolved.  Then once it solves that sumUpTo(1) equals one, it can begin to resolve sumUpTo(2) and so on.

### Discovering a recursive solution

Ok, so now we understand that when we see a function like sumUpTo we know that two steps are involved, breaking the function down into it's recursive calls until the base case is reached.  And then once the base case is resolved, then the other recursive calls are solved.  

```js
function sumUpTo(n){
  if(n > 1){
    return sumUpTo(n - 1) + n
  } else {
    return 1
  }
}
```

Now, let's try to disentangle how we get to a recursive solution.  We do so not by going through this breaking down and building back up process.  Rather we do so by a rewording.  Here are the specific steps.

1. We are given the problem sumUpTo(n) and so we solve it with an example.  

  ```js
  sumUpTo(5)
    // 1 + 2 + 3+ 4 + 5
  ```

2. Then we ask ourselves, *Can we reword the solution with the name of our function?*

  So in this case we say, well 1 + 2 + 3 + 4 + 5 is really sumUpTo(4) + 5.  What does it mean to print out all of the numbers down from 5?  Well it means print out 5, and then print out downFrom(4).  This leads to our recursive call of `downFrom(n - 1)`.  
  
3. Now the only thing left to do is look for a base case.  This is the case when there is really no more breaking down of the problem, so we can just return the solution for that case.  Here, sumUpTo(1) equals 1.

### Summary

In this lesson we learned two things: how to find the recursive solution to a problem, and how JavaScript evaluates a recursive solution.  We find the recursive solution by choosing an example, solving the example and then reword our solution by using our function.  For example we said that 1 + 2 + 3 + 4 + 5 can be reworded as the sum up to four plus five.  Then we find our base case, by finding the stopping point or the point where it is trivial to provide a return value.  Second, we also discussed how JavaScript evaluates our recursive solutions.  We said that a recursive solution is evaluated by making multiple recursive calls until the base case is reached.  No recursive call can be evaluated until the base case provides a solution.  Then the other recursive calls can be solved, one by one until we have arrived at our solution.
