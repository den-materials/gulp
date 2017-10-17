<!--
Creator: Zeb Girouard
Market: DEN
-->

![](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png)

<!--11:20 actually-->

<!--10:32 WDI4 -->
<!--11:06 WDI3 -->
<!--WDI5 9:41 -->
<!-- 11:10 5 minutes -->

<!--Hook: So we just talked about Sass and how it can make our lives easier.  We also know that ES6 can make our JS applications more modern.  We also know about minification which can speed up our web applications.  But every time we try to convert Sass to CSS, ES6 to ES5, or big JS files to minified JS files, we have to copy our code into a converter somewhere.  How annoying!  With Gulp, we can get that done automatically, every time we change one of our files. -->

# Gulp

### Why is this important?
<!-- framing the "why" in big-picture/real world examples -->
*This workshop is important because:*

Gulp is a useful tool for automating tasks in order to increase productivity. Today we'll specifically use it mostly for *transpiling* ES6 to ES5 and Sass into CSS.

### What are the objectives?
<!-- specific/measurable goal for students to achieve -->
*After this workshop, developers will be able to:*

<!--Get student to read aloud -->

* **Write** a Gulp task and run it
* **Leverage** pre-built Gulp plugins

### Where should we be now?
<!-- call out the skills that are prerequisites -->
*Before this workshop, developers should already be able to:*

* **Implement** client & server-side JavaScript.

<!--WDI3 11:08 -->
<!--10:34 WDI4 -->
<!--11:15 5 minutes -->

## Introducing Gulp

<!--Get student to read aloud -->

* Gulp is a software built on Node.
* It runs **tasks** that manipulate files on your system.
* It is an active, open-source project.
* There are many community-built plugins built to work directly with gulp.
* Among other things, it is commonly used for transcompilation and minificiation to automate your workflow.

<!--11:11 after demoing and turning over to devs -->
<!--11:20 10 minutes -->

## Installing Gulp

1. First, create a `gulp-class` folder, `cd` into it, and `npm init -y` your project
1. You'll want to install gulp globally. You can do this by running the `npm install gulp -g` command.
1. We also need to include gulp in our project. To do that, we will run `npm install gulp --save-dev`. Let's use `--save-dev` (a production environment should not be concerned with build tools).
1. Now, run `gulp`!

```bash
No gulpfile found
```

<!--WDI5 9:48  -->

Oops, we ran into an error. Gulp requires that we store our tasks in a `gulpfile.js`. This should be in the same directory as your `package.json`. Make that file then run Gulp again.

```bash
Using gulpfile ~/path/to/gulpfile.js
Task 'default' is not in your gulpfile
Please check the documentation for proper gulpfile formatting
```

Another error! Ok, let's solve this problem by defining a Gulp task.

<!--11:30 5 minutes -->

## Defining a Gulp Task

* What is a task?
* A task is something we must do to achieve a result.
* In Gulp, we create tasks to perform tasks that can transform our code.
* A task may perform one job; it may also perform many at once.

In our `gulpfile.js` we need to include the `gulp` module. To do this, we should define a variable: `var gulp = require('gulp');` This will allow us to call upon Gulp to **create a task**.

<!--11:35-->

<!--WDI3 11:20 when turning over to devs -->
<!--11:35 10 minutes -->

### First (Default) Task

After declaring our `gulp` variable, we should create our first task. This will require us to call upon Gulp to define a task. We must also have a name for our task. By *default*, Gulp requires a `default` task. It is the first task that Gulp will look for when reading your `gulpfile.js`. Let's define our first (default) task:

```javascript
var gulp = require('gulp');

//define a task with the name of 'default'
// and a callback to perform when the task is ran
gulp.task('default', function() {
  console.log('I am the default task. Hear me roar');
});
```

In your terminal, run `gulp`. This will have the library look for a `default` task in your `gulpfile.js`. It will then execute the callback that you define for your task. The output will appear as follows:

```bash
Starting 'default'...
I am the default task. Hear me roar
Finished 'default' after 144 μs
```

<!--WDI5 9:54  -->
<!--WDI4 10:43 turning over to devs -->
<!--11:38 -->

<!--11:24  WDI3 -->
<!--11:45 15 minutes -->

### Compiling Sass

Now let's compile some Sass to CSS.  We need to start by requiring the `gulp-sass` plugin for Gulp.

```bash
npm install --save-dev gulp-sass
```

Then we can require it in our `gulpfile.js`.

```js
//gulpfile.js
var gulp = require('gulp');
var sass = require('gulp-sass');
```

Define a `styles` task that looks in a `/sass` directory for all `.scss` files, logs any errors that occur, and then compiles the result into a `/css` directory.

```js
gulp.task('styles', function() {
    gulp.src('sass/**/*.scss')
        .pipe(sass().on('error', sass.logError))
        .pipe(gulp.dest('./css/'));
});
```
Create a `/sass` directory and `touch` a `main.scss` file inside.

Run `gulp styles` and it will find your `.scss` file and output a corresponding `.css` file for you.

Lastly let's add the task we just created to gulp's `default` task and have it run the task upon file changes by watching the file. Now every time you save, your `.scss` will trigger the `styles` gulp task and compile the changes for you!

<!--WDI5 10:08  -->
<!--11:32 when turning over to devs -->
<!--11:00 WDI4 coming back -->

To do this, at the bottom of your file add:

```js
gulp.task('default',function() {
    gulp.watch('sass/**/*.scss',['styles']);
});
```

Now we can run our Sass compilation with just `gulp`.

<!--WDI5 10:15  -->
<!--WDI4 11:03 turning over to devs -->
<!--WDI4 11:07 coming back -->
<!--12:00 15 minutes -->

### Compiling ES6

Now let's talk about ES6, aka ECMAScript 2015, the next iteration of JavaScript. Let's write some ES6 code and compile it to ES5, which is universally supported.  Again, we need some Gulp plugins.

Require all the modules we'll be using.

<!--12:15-->

```bash
npm install --save-dev gulp-babel babel-preset-es2015 babel-core
```

Inside `gulpfile.js`, do something similar to what we did to transpile our sass. Instead now we'll be using `gulp-babel` to help us transpile our ES6 to ES5.

Here we'll be creating a task called `scripts`. It will read in all our ES6 `.js` files living inside the `src` directory, pipe them through our babel to transpile them to ES5 and then send the output to corresponding files in the `dist` directory.

At the very bottom, just like previously, we're creating a default watch task that will execute the `scripts` task every time any `.js` file is saved inside our `src` directory. Pretty neat!

```js
var gulp = require('gulp');
var babel = require('gulp-babel');

gulp.task('scripts', function () {
	return gulp.src('src/**/*.js')
		.pipe(babel({
			presets: ['es2015']
		}))
		.pipe(gulp.dest('./dist/'));
});

gulp.task('default',function() {
    gulp.watch('src/**/*.js',['scripts']);
});
```

<!--12:02 when turning over to devs WDI3 -->

>Note: Babel is the transpiler we'll be using to convert our ES6 to ES5. `babel-preset-es2015` is one of the preset collection of [plugins](https://babeljs.io/docs/plugins/) we can use for it. It contains all the features for ES6 packed together, but in theory you could only require `es2015-arrow-functions` or `es2015-classes`, for example, if you wish to be more specific.

Let's try out some ES6. Below find some working ES6 code, take a moment to look it over. What are your thoughts?

<!--12:08 WDI3 -->

```js
// define an ES6 class called Person
class Person {
  // define the method to run for each instantiation
  constructor(name, age, type="person") {
    this.name = name
    this.age = age
    this.type = type
  }
  // define a greet method
  greet() {
    return `Hi I'm ${this.name}!`
  }
}

// export the Person class
module.exports = Person
```

<!--11:16 turning over to devs WDI4-->
<!--11:21 WDI4 coming back -->

Create an empty `app.js` file in your `src` directory and save it. Then run `gulp` in your project's root. Finally, copy the code above into `app.js`. What happened?

<!-- Go into dist/app.js and create a `new` Person for me.  console.log(zeb.greet()) and run app.js with node -->

<!--12:15 10 minutes -->

<!--Based on timing, this should probably move to the following day -->
<!--12:31 actually WDI2-->

<!--At 12:14 in WDI3 ran through "Additional Notes" then let them work through ES6 quiz for rest of class -->

## More ES6

Exciting stuff! Here's a quick look at some of the [new ES6 syntax](https://github.com/lukehoban/es6features). Take a moment to review it and then try out this [online quiz](http://tutorialzine.com/2015/11/think-you-know-es6-prove-it/) (open book is fine). It will introduce you to some new ES6 concepts. Feel free to jot down anything that's surprising and we'll discuss it. Feel free to try any of this fancy ES6 stuff out in your `src/app.js` file.

<!--12:25 5 minutes -->

## Additional Notes

* You will need to re-run the `gulp` command every time you make a change to the `gulpfile.js`.
* There is `nodemon` [support](https://github.com/JacksonGariety/gulp-nodemon) for gulp.
* As mentioned previously, you can have `gulp` **watch** files for changes using [gulp-watch](https://www.npmjs.com/package/gulp-watch).
* You can bundle your multiple files into one output file with [gulp-concat](https://github.com/contra/gulp-concat).
* And also minify [JS](https://github.com/terinjokes/gulp-uglify) or [CSS](https://github.com/scniro/gulp-clean-css)!
