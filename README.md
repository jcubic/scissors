# Scissors – fork of Paper.js

[![CI](https://github.com/jcubic/scissors/actions/workflows/ci.yml/badge.svg?branch=develop)](https://github.com/jcubic/scissors/actions/workflows/ci.yml) [![NPM](https://img.shields.io/npm/v/@jcubic/scissors.svg)](https://www.npmjs.com/package/@jcubic/scissors)

## Installing Paper.js

The recommended way to install and maintain Paper.js as a dependency in your
project is through the [Node.js Package Manager (NPM)](https://www.npmjs.com/)
for browsers, Node.js or Electron.

If NPM is already installed, simply type one of these commands in your project
folder:

```sh
npm install paper
```

Upon execution, you will find a `paper` folder inside the project's
`node_modules` folder.

For more information on how to install Node.js and NPM, read the chapter
[Installing Node.js and NPM](#installing-nodejs-and-npm).

### Using Paper.js with a Bundler

With a bundler such as webpack, Rollup, esbuild or Vite, import Paper.js and
set it up with a canvas element:

```js
import paper from 'paper'
// Or, with CommonJS: const paper = require('paper')

paper.setup(document.getElementById('canvas'))

const { Path } = paper
const circle = new Path.Circle({
  center: paper.view.center,
  radius: 30,
  fillColor: 'red'
})

paper.view.onFrame = event => {
  circle.rotate(1)
}
```

The module exports the global `paper` scope as its default export, with all
classes as its properties. Use `paper.view` and `paper.project` after calling
`paper.setup()`, rather than importing them, as they only exist from then on.
To use the core version without PaperScript, import `'paper/dist/paper-core'`
instead. With TypeScript, the default import requires `esModuleInterop`.

### Which Version to Use

The various distributions come with two different pre-build versions of
Paper.js, in minified and normal variants:

- `paper-full.js` – The full version for the browser, including PaperScript
  support and Acorn.js
- `paper-core.js` – The core version for the browser, without PaperScript
  support nor Acorn.js. You can use this to shave off some bytes and compilation
  time when working with JavaScript directly.

## Development

The main Paper.js source tree is hosted on
[GitHub](https://github.com/paperjs/paper.js/). `git` is required to create a
clone of the repository, and can be easily installed through your preferred
package manager on your platform.

### Get the Source

    git clone --recursive git://github.com/jcubic/scissors.git
    cd paper.js

To refresh your clone and fetch changes from origin, run:

    git fetch origin

To update the `jsdoc-toolkit` submodule, used to generate the documentation,
run:

    git submodule update  --init --recursive

### Setting Up For Building

Paper.js uses [Gulp.js](https://gulpjs.com/) for building, and has a couple of
dependencies as NPM modules. Read the chapter [Installing Node.js and
NPM](#installing-nodejs-and-npm) if you still need to install these.

Development uses [pnpm](https://pnpm.io/), which needs to be installed too. See
[Installing pnpm](https://pnpm.io/installation) for details.

In order to be able to build Paper.js, after checking out the repository, paper
has dependencies that need to be installed. Install them by issuing the
following commands from the Paper.js directory:

    pnpm install

### Building the Library

The Paper.js sources are distributed across many separate files, organised in
subfolders inside the `src` folder. To compile them all into distributable
files, you can run the `build` task:

    pnpm build

You will then find the built library files inside the `dist` folder, named
`paper-full.js` and `paper-core.js`, along with their minified versions. Read
more about this in [Which Version to Use?](#which-version-to-use).

### Running Directly from Separate Source Files

As a handy alternative to building the library after each change to try it out
in your scripts, there is the `load` task, that replaces the built libraries
with symbolic links to the `scrc/load.js` script. This script then load the
library directly from all the separate source files in the `src` folder, through
the [Prepro.js](https://github.com/lehni/prepro.js) JavaScript preprocessing
library.

This means you can switch between loading from sources and loading a built
library simply by running.

    pnpm load

And to go back to a built library

    pnpm build

Note that your PaperScripts examples do not need to change, they can keep
loading `dist/paper-full.js`, which will always do the right thing. Note also
that `src/load.js` handles both browsers and Node.js, as supported by Prepro.js.

### Other Build Tasks

Create a final zipped distribution file inside the `dist` folder:

    pnpm dist

### Branch structure

Since the release of version `0.9.22`, Paper.js has adopted aspects of the Git-
Flow workflow. All development is taking place in the
[`develop`](https://github.com/paperjs/paper.js/tree/develop) branch, which is
only merged into [`main`](https://github.com/paperjs/paper.js/tree/main)
when a new release occurs.

As of version `0.9.26`, the `dist` folder is excluded on all branches, and the
building is now part of the `pnpm publish` process by way of the `prepublish`
script.

We also offer prebuilt versions of the latest state of the `develop` branch on
[`prebuilt/module`](https://github.com/paperjs/paper.js/tree/prebuilt/module)
and [`prebuilt/dist`](https://github.com/paperjs/paper.js/tree/prebuilt/dist).

### Building the Documentation

Similarly to building the library, you can run the `docs` task to build the
documentation:

    pnpm docs

Your docs will then be located at `dist/docs`.

### Testing

Paper.js was developed and tested from day 1 using proper unit testing through
jQuery's [Qunit](https://qunitjs.com/). To run the tests after any
change to the library's source, simply open `index.html` inside the `test`
folder in your web browser. There should be a green bar at the top, meaning all
tests have passed. If the bar is red, some tests have not passed. These will be
highlighted and become visible when scrolling down.

If you are testing on Chrome, some of the tests will fail due to the browser's
CORS restrictions. In order to run the browser based tests on Chrome, you need
to run a local web-server through Gulp.js. The following command will handle it
for you, and will also open the browser at the right address straight away:

    pnpm test:browser

To run them in headless Chrome instead, as CI does, and report the result in the
terminal:

    pnpm test:chrome

To test the Node.js version of Paper.js, use this command:

    pnpm test

### Contributing [![Open Source Helpers](https://www.codetriage.com/paperjs/paper.js/badges/users.svg)](https://www.codetriage.com/paperjs/paper.js)

The main Paper.js source tree is hosted on GitHub, thus you should create a fork
of the repository in which you perform development. See
<https://help.github.com/articles/fork-a-repo/>.

We prefer that you send a
[pull request on GitHub](https://help.github.com/articles/about-pull-requests/)
which will then be merged into the official main line repository.

Also, in your first contribution, add yourself to the end of `AUTHORS.md` (which
of course is optional).

In addition to contributing code you can also triage issues which may include
reproducing bug reports or asking for vital information, such as version numbers
or reproduction instructions. If you would like to start triaging issues, one
easy way to get started is to
[subscribe to paper.js on CodeTriage](https://www.codetriage.com/paperjs/paper.js).

**Get the source (for contributing):**

If you want to contribute to the project you will have to [make a
fork](https://help.github.com/articles/fork-a-repo/). Then do this:

    git clone --recursive git@github.com:yourusername/paper.js.git
    cd paper.js
    git remote add upstream git@github.com:jcubic/scissors.js.git

To then fetch changes from upstream, run

    git fetch upstream

#### Creating and Submitting a Patch

As mentioned above, we prefer that you send a
[pull request](https://help.github.com/articles/about-pull-requests/) on GitHub:

1. Create a fork of the upstream repository by visiting
   <https://github.com/paperjs/paper.js/fork>. If you feel insecure, here's a
   great guide: <https://help.github.com/articles/fork-a-repo/>

2. Clone of your repository: `git clone
   https://yourusername@github.com/yourusername/paper.js.git`

3. This is important: Create a so-called *topic branch* based on the `develop`
   branch: `git checkout -tb name-of-my-patch develop` where `name-of-my-patch`
   is a short but descriptive name of the patch you're about to create. Don't
   worry about the perfect name though -- you can change this name at any time
   later on.

4. Hack! Make your changes, additions, etc., commit them then push them to your
   GitHub fork: `git push origin name-of-my-patch`

5. Send a pull request to the upstream repository's owner by visiting your
   repository's site at GitHub (i.e. https://github.com/yourusername/paper.js)
   and press the "Pull Request" button. Make sure you are creating the pull
   request to the `develop` branch, not the `main` branch. Here's a good guide
   on pull requests: <https://help.github.com/articles/about-pull-requests/>

##### Use one topic branch per feature:

Don't mix different kinds of patches in the same branch. Instead, merge them all
together into your `develop` branch (or develop everything in your `develop`
branch and then cherry-pick-and-merge into the different topic branches). Git
provides for an extremely flexible workflow, which in many ways causes more
confusion than it helps you when new to collaborative software development. The
guides provided by GitHub at <https://help.github.com/> are a really good
starting point and reference. If you are fixing an issue, a convenient way to
name the branch is to use the issue number as a prefix, like this: `git checkout
-tb issue-937-feature-add-text-styling`.

For a list of authors and contributors, please see
[AUTHORS](https://github.com/jcubic/scissors/blob/main/AUTHORS.md).

## License

Distributed under the MIT license. See
[LICENSE](https://github.com/jcubic/scissors/blob/main/LICENSE.txt)
for details.
