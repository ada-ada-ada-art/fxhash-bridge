BRIDGE collaborative project for fxhash
================

BRIDGE is an experimental project for fxhash. Any artist can contribute. 
The main rule: draw the tiles at the given coordinates. While they are quite visible, you can add everything you want: birds, stars, grass, reflexions, circles...
For each edition, there'll be a default style, but you could try all the other styles using the shortcut `n`.

# The story

I'm Camille Roux ([@camillerouxart](https://twitter.com/camillerouxart)). I planned to do a lot of collab this year with fxhash artists I like, but it takes too much time (or maybe I like too much artists 🤣). Some days ago, I woke up with this idea in mind: "Hey! What if I make a collaboration with all the artists I like at once!".
This is how the project was born.

Moreover, I love the mutual aid and solidarity between fxhash generative artists. The BRIDGE can be seen as a symbol of the link between us ♥️

# The rules

If you contribute to the project, you accept the following rules:

* I can mint this project on my [fxhash profile](https://www.fxhash.xyz/u/Camille%20Roux) with any subset of the styles. (If you've an idea about what I could do with the unselected styles, let me know!)
* I'll select the styles I'll mint
* I'll share manually and equitably the primary market revenue of the/each mint with the creators of the styles included (the 2sd market revenue is for me)

# How to contribute

- clone the project
- run `npm i``
- duplicate the `./styles/boilerplate.js`, change the name and info
- in `index.js` set `defaultStyleClass` to your class style to only use you style and import the class
- run `npm start` to launch the project
- create a pull request when you're ready

# How to be selected

In order to have your style selected, it must:

* be dimension-less (same output whatever the canvas dimensions)
* be determist (same output if you refresh)
* draw the tiles at the correct place (tiles must be not move if you change style)
* write your tez wallet adress in the comment in your code
* have fun, make something awesome... ♥️

# Technical info
- The 3D coordinates of point of the bridge is represented by a p5.Vector with the following dimension:
  - x: from -gridSizeX / 2 (left) to gridSizeX / 2 (right)
  - y: from 0 (front) to gridSizeY (back)
  - z: from 0 (down) to 1 (up)
- The 2D coordinates of the tiles are an array of p5.Vector. The coord go from (0,0) (top left) to (1,1) (bottom right). So you'll have to multiply them by `s` at drawing (this is done to be dimension-less)


About the FXHASH Generative Token webpack boilerplate
================

A boilerplate to automate and ease the creation of Generative Tokens on fxhash. This project uses [webpack](https://webpack.js.org/) and [webpack-dev-server](https://github.com/webpack/webpack-dev-server) to improve the development and deployment experience.

Before diving into the development of your token, we recommend reading the [Guide to mint a Generative Token](https://fxhash.xyz/articles/guide-mint-generative-token) to get some understanding of the process.

If you are looking for a simpler boilerplate, you can use the [fxhash simple boilerplate](https://github.com/fxhash/fxhash-simple-boilerplate) instead.


# Scope of this project

* provide a local environment in which you can iterate and use modern features from the javascript ecosystem
* automate the creation of a .zip file ready to be uploaded on fxhash


# How to use

You will need to have [nodejs](https://nodejs.org/) installed.

## Installation

> First, make sure that your node version is >= 14

Clone the repository on your machine and move to the directory
```sh
$ git clone https://github.com/camilleroux/fxhash-webpack-boilerplate-p5js.git your_folder && cd your_folder
```

Install the packages required for the local environment
```sh
$ npm i
```

## Start local environment

```sh
$ npm start
```

This last command will start a local http server with [live reloading](https://webpack.js.org/configuration/dev-server/#devserverlivereload) enabled so that you can iterate faster on your projects. Open [http://localhost:8080](http://localhost:8080) to see your project in the browser.

## Build

```sh
$ npm run build
```

Will bundle your js dependencies into a single minified `bundle.js` file, move your files from the `public/` to the `dist/` folder, and link the `bundle.js` with the `index.html`.

**Moreover, it will create a `dist-zipped/project.zip` file which can be directly imported on fxhash**.

# Develop your token

Once the environment is started, you can edit the `src/index.js` file to start building your artwork. The `index.html` file is located in the `public/` folder.

You can import libraries using `npm` or by adding the library file in the `public/` folder and link those using relative paths in the `index.html`.

Any file in the `public/` folder will be added to the final project. 

## fxhash snippet

fxhash requires you to use a javascript code snippet so that the platform can inject some code when tokens will be generated from your Generative Token. The code snippet is already in the `index.html` file of this boilerplate, so you don't have to add it yourself.

**During the development stages, the snippet will generate a random hash each time the page is refreshed**. This way, it helps you reproduce the conditions in which your token will be executed on fxhash.

It creates 3 variables:
- `fxhash`: a random 64 characters hexadecimal string. This particular variable will be hardcoded with a static hash when someone mints a token from your GT
- `fxrand()`: a PRNG function that generates deterministic PRN between 0 and 1. **Simply use it instead of Math.random()**.

*The index.js of this boilerplate quickly demonstrates how to use these*.

## How do Generative Tokens work

This is how Generative Tokens work on fxhash:
* you upload your project to the platform (see next section)
* you mint your project
* when a collector will mint its unique token from your Generative Token, a random hash will be hard-coded in the **fxhash code snippet**
* the token will now have its own `index.html` file, with a **static** hash, ensuring its immutability 

The [Guide to mint a Generative Token](https://fxhash.xyz/articles/guide-mint-generative-token) give in-depth details about this process.


# Publish your token

Once you are happy with the results, you can run the following command:

```sh
$ npm run build
```

This will create a `dist-zipped/project.zip` file.

Go to [https://fxhash.xyz/sandbox/](https://fxhash.xyz/sandbox/) and upload the `project.zip` file in there to see if it works properly.

If your token does not work properly, you can iterate easily by updating your files, running `$ npm run build` again, and upload the zip file again.

Finally, you can mint your token using the same `project.zip`file.


# Rules to follow

> Theses rules must be followed to ensure that your token will be future-proof, accepted by fxhash, and behave in the intended way

* the zip file must be under 15 Mb
* any path to a resource must be relative (./path/to/file.ext)
* no external resources allowed, you must put all your resources in the `public/` folder (sub-folders are OK)
* no network calls allowed (but calls to get resources from within your `public/` folder)
* you must handle any viewport size (by implementing a response to the `resize` event of the `window`)
* you **cannot use random number generation without a seed** (the same input hash must always yield the same output). The `fxrand` function does a very good job in that regard.
