#Javascript

##What is Javascript?

Javascript is the language used for client-side logic. What does this mean?

1. The code is on the user's computer.
2. You could make great effects.
3. You're a few steps away from cutting-edge awesomeness (well, technically, you're already there).

##A quick introduction

    //variable declaration
    var goodPractice = "Hi, I'm a string";
    badPractice = "You don't want to do this";

Many texts say you must use `var` to declare a variable. That's not true. What's true is you should.<br>
In the above, `var goodPractice` makes a **local** variable. `badPractice` is a **global** variable.

There are very few cases where you would make a global variable. Some examples:

<ol>
<li>Represent the logged-in user in your application.</li>
<li>The elegance of your code is lost in you passing some variable everywhere.</li>
</ol>

Lastly, you don't *need* to use a semi-colon at the end, but it is good practice to do it anyways, because Javascript places them where it thinks they should go anyways.

	//functions
	function(a,b) {
	  return a + b;
	}
	
	var addTwoNumbers = function (a,b) {
	  return a + b;
	}
	
	function addTwoNumbers(a,b) {
	  return a + b;
	}
	
	//objects
	
	var cat = {
	  name: "Matz",
	  age: 10,
	  purr: function () {
	    console.log("Meow!");
	  },
	  
	  ageOneYear: function() {
	    this.age += 1;
	  }
	};
	
	cat.purr(); //"Meow!"
	cat.ageOneYear(); //Note, we didn't return anything explicitly, so it returns undefined, but the age is now 11!
	
	var makeCatPurr = cat['purr']; //Hash-like notation is usable.
	makeCatPurr();
	
	cat.favoriteFood = "Fish";
	
	//Constructur functions (Object-orientation in Javascript is a tad odd, eh?)
	function Kitten(name, age) {
      this.name = name;
      this.age = age;

      this.meow = function () {
        console.log(this.name + ' says "meow!"');
      };
    }

    var kitten = new Kitten("Katz", 4);
    
    //All kittens meow though, it is redundant to have the same code for every kitten,
    //so we go to the prototype of the kitten and add it there. This will give all kittens the 'meow'.
    
    Kitten.prototype.meow = function () {
      console.log(this.name + ' says "meow!"');
    }
    
    //I recommend researching how prototype works. In short: Javascript looks through kitten attributes, doesn't find
    //meow, so it looks in kitten.__proto__, where it finds it.

What is `this`? `this` is a special keyword that refers to the object that is the context in which the function was called.
Note, `this` can change, and it will change, and it will cause you headaches. Especially when dealing with callbacks (discussed soon).
What happens is the context changes, so `this` changes, and when you really wanted to reference the object you were originally messing with you can't!
A quick fix, that I don't recommend abusing (there are different styles of calling functions, which helps), but use `var that = this`; 
Then, you could use `that` wherever you wanted to use `this`, so when `this` changes, `that` is the original `this`. 
**Confused? If you run into any of this, I will help!**

##Closures and scope

The **scope** of a method is the set of variables that are available
for use within the method. Scope in JS is nested:

    function sum(nums) {
      var count = 0;
      
      function addNum(num) {
        // `count` here refers to the outside defined count variable.
        // We can access it in here, because at the point where `addNum`
        // is defined, `count` was "in scope".
        count += num;
      }
      
      // run the addNum function for each num
      nums.forEach(addNum);
      
      return count;
    }

The variables available when we call a function include:

0. the arguments
0. any local variables declared inside the function
0. **any variables that were declared when the function was first
   defined**.

Scopes are nested. A new, inner scope is created each time a function
is called. The function can refer to this new, current scope, or
anything from the enclosing scope.

Not only can we access the outside variables, we can even reset
them. This is what lets the `addNum` method modify the outside `count`
variable.

In JavaScript, every function has access to the variables from
enclosing scopes. Functions that use (or **capture**) these variables
(called **free variables**) are called **closures**. `addNum`, which
captures `count`, is a closure.

Notice, in Javascript, we could define functions inside other functions.

Further, for the sake of example, this allows us to create truly private states. Though, I admit I think they're overrated
and there are better ways of going about doing something like this, but:

    function Count() {
      var count = 1;
      
      return function () {
        return count++;
      };
    }
    var counter = Count();
    console.log(counter()); // 1
    console.log(counter()); // 2
    
##Callbacks

A callback is a function that is passed to another function and intended to be called at a later time. The most common use of callbacks is when a result will not be immediately available, typically because it relies on user input.
Why is this important? Javascript is asynchronous, and for good reason. We need to use Javascript for event-handling, knowing when you click your mouse, or a key, or whatever.
Due to this, the fact that javascript is asynchronous helps in heaps. Here's an example of a callback and closure in one.

    function scheduleTaskReminder(task) {
    //remind in one minute.
      window.setTimeout(function () {
        console.log("Remember to do: " + task);
      }, 60 * 1000);
    }
    scheduleTaskReminder("the callback");
    scheduleTaskReminder("code");
    
Here, we pass in an anonymous function (this is common) to `window.setTimeout`, a function that waits some time before happening.
The application of passing callbacks goes very far, but try not to take it too far. It is very easy to get into "callback hell", where you have callbackception. If you run into this, think about refactoring your code, because there is a better way about it.

