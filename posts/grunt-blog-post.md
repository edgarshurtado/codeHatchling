![Grunt automatization](https://www.smashingmagazine.com/wp-content/uploads/2013/10/grund-js-opt.png)

[As every programmer should](http://threevirtues.com/), I'm lazy. I'm lazy as hell for any action I have to repeat several times. And overall, I'm lazy for doing long tedious tasks that don't imply any mental effort. The bad thing about programming is that there are a lot of this time-consuming-not-enjoyable tasks. Fortunately, we can make our life easier by using some automation scripts. What's the point in being a programmer if not for making computers do our work? That's why I'm so into bash aliases that runs long scripts, configuring my computer to open all my needed programs on startup, and using a task manager to avoid the repetitive tasks while developing a new web page. This last point is the one I cover in this blog post. So, if you want to save time and improve the quality of your projects grab a drink and continue reading.

> TL;DR :
> * Install at your computer [ImageMagick](https://www.imagemagick.org/script/index.php)
> * Copy and paste at your projects folder this [package.json](https://gist.github.com/edgarshurtado/38aa19212a692ce37885d5cd0c056ad8) and [Gruntfile.json](https://gist.github.com/edgarshurtado/6ed2fae56b300386b81f2c992a2da8ab) files.

[Grunt](https://gruntjs.com/) is the task manager of my choice for automatizing processes while I've been developing my portfolio. I know there are a lot of different possibilities for doing so, but since I started to use it because of an Udacity course about [responsive images ](https://www.udacity.com/course/responsive-images--ud882) and liked it, I kept using it. I assume you know the basics of Grunt in this blog post. If it's not your case, visit the [getting started](https://gruntjs.com/getting-started) section at the Grunt web page.

Starting to use it is not that intuitive, though. Or at least not for what I consider is the minimum useful configuration. That's the motivation for this blog post. To have all this information I've had to search through the internet in a single place. I hope this is of some use to you 😁 . With the configuration below, you'll have:

* ES6 to ES5 compilation
* Scss to CSS compilation
* All vendor CSS prefixes added to your CSS files
* Minify JS and CSS files
* Image compression
* AND! My favorite, Auto-refresh of the web browser. No more CTRL + F5 (CMD + R)

Let's begin!

# Installing dependencies

These are all the dependencies you're going to need for this walkthrough.

Node dependencies:

```
npm i -D babel-eslint babel-preset-es2015 grunt-babel grunt-cli grunt-contrib-clean grunt-contrib-copy grunt-contrib-sass grunt-contrib-watch grunt-mkdir grunt-postcss grunt-responsive-images load-grunt-tasks
```

Other dependencies:

* [ImageMagick](https://www.imagemagick.org/script/index.php)

# Easy grunt tasks loading

Grunt, in order to do its stuff, needs to load tasks, tasks that we previously have installed via npm. This can be tedious as well, though. Tedious and verbose 😖 . But! `load-grunt-tasks` is here to help.

This dependency for grunt automatically imports all the other modules you have installed in your project. So for example, instead of having to declare:

```
module.exports = function(grunt){
    grunt.loadNpmTasks('grunt-contrib-sass')
    grunt.loadNpmTasks('grunt-contrib-clean')
    grunt.loadNpmTasks('grunt-contrib-copy')
    grunt.loadNpmTasks('grunt-contrib-sass')
    grunt.loadNpmTasks('grunt-contrib-watch')

    // grunt config
}
```

You can simply do:

```
module.exports = function(grunt){
    require("load-grunt-tasks")(grunt);

    // grunt config
}
```

Great! Isn't it?

# Convert ES6 to ES5 with Babel

Babel is a post-processor for your JavaScript. The following configuration is which I use for converting my ES6 files to ES5. The importance is that by doing this you are allowed to use all the cool new features of ES6, without having to worry about web browser support. In the end, what the web browser will receive is ES5 which is deeply implanted already.

For this module to work you need to have installed `babel-preset-es2015` through npm and then configure at your `package.json` this bit:

```
"babel": {
    "presets": [
        "es2015"
    ]
}
```

Finally, add to your Gruntfile:


```
module.exports = function(grunt){
    require("load-grunt-tasks")(grunt);

    grunt.initConfig({
    "babel": {
        options: {
            sourceMap: true  // Generate .map files at destination
        },
        dist: {
            files: {  // 'destination_file': 'source_file'
                'public/app.js': 'js/app.js',
                'public/other-example.js': 'js/other-example.js'
            },
            options: {
                minified: true  // Minify files
            }
        }
    }
    grunt.registerTask("babel-task", ["babel"]);
}
```

Besides converting ES6 code to ES5, this configuration creates `.map` files at the destinations and minifies each file.

Something that seemed weird to me at first was that the destination path is the key for the `files` config object, whereas the source path is the value for that key. Instinctively I'd thought in a `source → destination` but is the contrary. Sure they have their reasons but took me a little to realize of this.

# Convert Scss to CSS and add vendor prefixes

The following Grunt configuration allows using `scss` files for the project styles instead of plain CSS. You know, all that about being 'sassy' with your CSS from CodeSchool 😜. But there's more! In addition, this configuration will add all the needed prefixes for your css rules to make your styles work in as many browsers as possible. Personally, I don't know how I've lived without this for so long 🙃 ...

First, the dependencies:
* `autoprefixer`
* `grunt-contrib-sass`
* `grunt-postcss`

The workflow for the styles is: `grunt-contrib-sass` converts scss to css, then  `grunt-postcss` takes that css and using `autoprefixer`, adds the browser vendors.

But there's one more dependency, something you have to add to the `package.json` file:

```
"browserslist": [
    " > 5%",
    "last 2 versions"
],
```

Browserlist isn't an autoprefixer nor grunt-postcss thing. Browserlist is actually a web page ([browserl.ist](http://browserl.ist/)) that offers an API to query for web browsers names. And then, some modules as autoprefixer uses the result of the query for their configuration, [something that makes our lives easier](https://css-tricks.com/browserlist-good-idea/). But what does the configuration above do?:

* "> 5%" → Return all browsers' names with a minimum usage of 5%
* "last 2 versions" → Return the names for the last 2 versions of every browser

 You can go you to the [browserl.ist webpage](http://browserl.ist/) and play with its query search bar and see the results. Actually, I really encourage you to play a bit with it, because some useless criteria could've been added otherwise.

For example, while writing this blog post I did play with [browserl.ist](http://browserl.ist/) and discovered that there's no use in mixing the above two criteria. This is due to the searcher returning any browser name that matches at least one of the different rules provided. This means that the search looks for `criteria1 OR criteria2` and since all the browsers with a usage greater than 5% are a subset of all the last 2 versions of every browser, I could remove the first criteria getting the same result. This is true for now, though. With the following browser releases, who knows if everyone will instantly update.

> By the way, I found curious that browserlist uses [Can I Use](http://caniuse.com/) data.

Well, once we have browserlist with our wanted specifications, there's just the Gruntfile config left:

```
module.exports = function(grunt){
    require("load-grunt-tasks")(grunt);

    grunt.initConfig({
        // ... Babel config

        "sass": {
            dist: {
                files: {
                    'public/styles.css': 'scss/main.scss'
                },
                options: {
                    style: 'compressed'
                }
            }
        },
        "postcss": {
            options: {
                map: true,
                processors: [
                    require('autoprefixer')({browsers: ["> 5%"]})
                ]
            },
            dist: {
                src: 'public/*.css'
            }
        }
    }
    // ... babel task

    grunt.initConfig({
    grunt.registerTask("postcss-task", ["postcss"]);
}
```

Notice that, same as with Babel, we use an option to get the resulting CSS compressed. The `destination → source` fashion is kept in `gulp-contrib-sass` as well. However, `grunt-postcss` only needs the *src* because it modifies the **css file** itself

There's just one more thing regarding CSS files, but it's just a recommendation. I always tend to reduce the number of requests my pages need. That's why I use a `main.scss` file where I import all the app scss files. this way I have all my styles in a single file.


```scss
@import '_resources.scss';
@import '_layout.scss';
@import '_brand_styles.scss';
@import '_cv_styles.scss';
@import '_projects_styles.scss';
@import '_contact_styles.scss';
```
> Example from my portfolio page main.scss file


I suppose there are cases where is better to have the css load modularized, but that's an issue to address in big projects. For every project I've made so far, this approach has been good enough.

# Generating responsive images

It's not a discovery that nowadays, most websites are accessed via mobile devices. Smartphones and tablets are the main window for internet for so many people. That means that, probably, quite a lot of your visitors are using their mobile data plans for accessing your site. And those are often limited in how many GB they can download per month. For this reason is really important that as an act of respect for all of them, we developers try to compress our sites. Optimizing them for being mobile data friendly 😁 . And the biggest data impact for a web page is images.

The following configuration will automatically compress your images with 2 different resolutions. The process will let your originals intact, dumping the result files in a different folder.

For this process, we need as dependencies:

* `grunt-responsive-images`
* `grunt-contrib-copy`

Then, add the following configuration to the `Gruntfile.js`

```
module.exports = function(grunt){
    require("load-grunt-tasks")(grunt);

    grunt.initConfig({
        // ... babel and css configuration

        responsive_images: {
            jpeg_images: {
                options: {
                    engine: 'im',
                    sizes: [
                        {
                            name: '2x',
                            width: 1600,
                            quality: 30
                        },
                        {
                            name: '1x',
                            width: 800,
                            quality: 30
                        }
                    ]
                },
                files: [{
                    expand: true,
                    src: ['*.{gif,jpg}'],
                    cwd: 'images/',
                    dest: 'public/images/'
                }]
            }
        },
        copy: {
            main: {
                files: [{
                    expand: true,
                    cwd: 'images/',
                    src: ['*.svg', '*.png'],
                    dest: 'public/images/'
                }]
            }
        }
    }
    //...babel and css tasks

    grunt.registerTask("responsive_images-task", ["responsive_images", "copy"]);
    grunt.registerTask("copy-task", ["copy"]);
}

```

In the `options` section, we use the compression engine 'im' which stands for `ImageMagick`. This is a software you have to get installed in advance for using this compression process (See the very beginning of this post).

Following the engine specification, there's the sizes property. This is an array of all the different versions we want for each image. In my case, I have 2 sizes with a `width` of 1600 and 800 respectively. The `quality` in both cases will be of 30 (more than enough for any image). And finally, each file will have a **suffix** pointing out its version (which we configure in the `name` property).

At the `files` configuration, again we have an array. This is useful in case we have several data origins, destinations or such. The `cwd` is the **source directory** for the files. Whereas in the `dest` object key, we set the **destination** for the processed images. Finally, at the `src` property, we have an array with all the **source files**. Noticed that we can set patterns to match instead of full files names. In the configuration I'm showing in this post, the target for the **responsive_images_process** are **gif** and **jpg** images.

Finally, with the `copy` process, all the `svg` and `png` files are moved together with the ones affected by the `responsive_images` configuration. The SVG files are the better optimized for the web, so we have nothing to do here. And the `png` ones ... Well, I didn't manage to make **ImageMagick** to compress them 😅 . Lucky me, they're usually pretty light.

# The build process.

To get everything working together properly (and not having to type a bunch of tasks every time), I have a build process that first prepares my assets folder and then launches the processes for my **javascript**, **scss** and **images** files.

This task dependencies are:
* `grunt-contrib-clean`
* `grunt-mkdir`

And the grunt file configuration:

```
module.exports = function(grunt){
    require("load-grunt-tasks")(grunt);

    grunt.initConfig({
        /... babel, css and responsive images configuration

        mkdir: { // Creates folder in case doesn't exists
            dev: {
                options: {
                    create: ['public/images']
                },
            },
        },

        clean: {
            dev: {
                src: ['public'],
            },
        }
    }

    /... babel, css, and responsive images tasks

    grunt.registerTask("clean-public", [ "mkdir" ,"clean"]);
    grunt.registerTask("build", ["clean-public", "responsive_images-task", "sass-task", "babel-task"]);
}
```

First I set the `clean-public` task that I need. ThenI registered a task named as `build` to launch, with a single command, all the assets related tasks.

# Watch task. Apply changes automatically and refresh the browser

As I said at the blog post introduction, this is my favorite task by far. It was, indeed, the whole point for learning a task manager 😁 .

This task will stay alert to the files you configure, and when any of those change, it will launch the needed tasks to see your changes applied. And you won't even have to refresh the browser your own. Sweet, isn't it?

The dependency `grunt-contrib-watch` is the one which does all the magic.

Once installed, configure at Gruntfile.js:

```
module.exports = function(grunt){
    require("load-grunt-tasks")(grunt);

    grunt.initConfig({
        //... all the former configuration

        "watch": {
            css: {
                files: [ 'scss/*.scss'],
                tasks: ['sass','postcss']
            },
            js: {
                files: [ 'js/*.js'],
                tasks: ['babel']
            },
            html: {
                files: ["index.html"]
            },
            images: {
                files: ['images/*'],
                tasks: ['responsive_images']
            },
            options: { livereload: true },
        }


        // Former tasks registrations

        grunt.registerTask("default", ["build","watch"]);
    }
}
```

In every configuration key, I have organized all the files I want the process to be aware of and which tasks should be launched every time one of the watched files change.

Finally the line `options: { livereload: true }` is which enables the browser autorefresh. This is the easiest way to do it. Personally, I don't like the kind of solution that implies getting a plugin installed in your browser. However, with `grunt-contrib-watch` just add the following script to your page and you're done.

```
<script src="http://localhost:35729/livereload.js"></script>
```

Then, any watched file that is changed will trigger the refreshing of the browser. That's why I have the process listening for `index.html`. Even though it has no task associated, the fact of **watch** being listening for its changes will refresh the browser when something in the DOM is modified.

You can't imagine how much I've loved this process while programming my portfolio 😍.

By the way. I think is worth to mention that I register this task together with the `build` task naming it as `default`. With this, what I accomplish is to get this process running by simply typing at my console `grunt`. Laziness is the way 😜 .


And that's been all. I leave you here my complete Gruntfile.js and package.json files in case you want to use them.

Happy coding =)

<script src="https://gist.github.com/edgarshurtado/6ed2fae56b300386b81f2c992a2da8ab.js"></script>
<script src="https://gist.github.com/edgarshurtado/38aa19212a692ce37885d5cd0c056ad8.js"></script>

P.S: I want this post to be helpful, so any suggestion for improving the explanations or examples will be appreciated.
