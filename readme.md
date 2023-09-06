# first steps

Create a direcotry, initialize npm, install webpack locally and install the webpack-cli

run the following commands in a terminal window:

`mkdir webpack-demo
cd webpack-demo
npm init -y
npm install webpack webpack-cli --save-dev`

Create the following directory structure and files

`project

webpack-demo
|- package.json
|- package-lock.json
|- index.html

|- /src
|- - index.js`

Add another directory 'dist' which will be where our distribution code will go. Manually place index.html in 'dist', later on, we'll generate index.html through the build process, but for now, we'll just place it there

webpack-demo
|- package.json
|- package-lock.json
| - /dist
| - - index.html

|- /src
|- - index.js`

'index.html' relies on a library 'lodash', so install that locally

npm install --save lodash

then import 'lodash' into our script index.js:

import \_ from 'lodash';

Remove the lodash script from index.html as we now import it.

load the bundle instead of the raw ./src file

 <script src="main.js"></script>

run `npx webpacK` on the command line. This will generate 'main.js' in the dist folder

Next, create a config file for webpack, which will save you typing loads of commands in the terminal

`
webpack-demo
|- package.json
|- package-lock.json
|- webpack.config.js

| - /dist
| - - index.html

|- /src
|- - index.js`

webpack.config.js contents:
const path = require("path");

module.exports = {
entry: "./src/index.js",
output: {
filename: "main.js",
path: path.resolve(\_\_dirname, "dist"),
},
};
`

now run build again using the config file,
run the following in the console terminal:

npx webpack --config webpack.config.js

A configuration file allows far more flexibility than CLI usage. We can specify loader rules, plugins, resolve options and many other enhancements this way.

set up a short cut to run webpack:

package.json:
{
"name": "webpack-demo",
"version": "1.0.0",
"description": "",
"private": true,
"scripts": {

    "test": "echo \"Error: no test specified\" && exit 1",

    "build": "webpack"

},
"keywords": [],
"author": "",
"license": "ISC",
"devDependencies": {
"webpack": "^5.4.0",
"webpack-cli": "^4.2.0"
},
"dependencies": {
"lodash": "^4.17.20"
}
}

now we can run npm build in the terminal rather than running the npx command:

'npm run build'

Now we have a basic build compiled, the code in the 'dist' folder.

webpack-demo
|- package.json
|- package-lock.json
|- webpack.config.js
|- /dist
|- - main.js
|- - index.html
|- /src
|- - index.js
|- /node_modules

### Bundling some css

edit index.html to this:

 <body>

    <script src="bundle.js"></script>

   </body>
 </html>

webpack.config.js to this:

`
const path = require('path');

module.exports = {
entry: './src/index.js',
output: {

    filename: 'bundle.js',
     path: path.resolve(__dirname, 'dist'),

},
};`

### Loading CSS

In order to import a CSS file from within a JavaScript module, you need to install and add the style-loader and css-loader to your module configuration:

webpack.config.js:

`const path = require('path');

module.exports = {
entry: './src/index.js',
output: {
filename: 'bundle.js',
path: path.resolve(\_\_dirname, 'dist'),
},

module: {

    rules: [

      {

        test: /\.css$/i,

        use: ['style-loader', 'css-loader'],

      },

    ],

},
};`

Module loaders can be chained. Each loader in the chain applies transformations to the processed resource. A chain is executed in reverse order. The first loader passes its result (resource with applied transformations) to the next one, and so forth. Finally, webpack expects JavaScript to be returned by the last loader in the chain.

The above order of loaders should be maintained: 'style-loader' comes first and followed by 'css-loader'

webpack uses a regular expression to determine which files it should look for and serve to a specific loader. In this case, any file that ends with .css will be served to the style-loader and the css-loader.

This enables you to import './style.css' into the file that depends on that styling. Now, when that module is run, a <style> tag with the stringified css will be inserted into the <head> of your html file.

run this command in the terminal:
`npm install --save-dev style-loader css-loader`

add a new style.css file to our project and import it in our index.js:

webpack-demo
|- package.json
|- package-lock.json
|- webpack.config.js
|- /dist
|- - main.js
|- - index.html
|- /src
|- - index.js
|- - style.css
|- /node_modules

add the following to src/index.js:
`import './style.css';`

now run the build command:

`npm run build`

# Loading Images

Use the built in Asset Modules:

change our `webpack.config.js` file to the following:

`
const path = require('path');

module.exports = {
entry: './src/index.js',
output: {
filename: 'bundle.js',
path: path.resolve(\_\_dirname, 'dist'),
},
module: {
rules: [
{
test: /\.css$/i,
use: ['style-loader', 'css-loader'],
},

      {

        test: /\.(png|svg|jpg|jpeg|gif)$/i,

        type: 'asset/resource',

      },
     ],

},
};
`

add an image to our project:

`webpack-demo
|- package.json
|- package-lock.json
|- webpack.config.js
|- /dist
|- - bundle.js
|- - index.html
|- /src
|- - icon.png
|- - style.css
|- - index.js
|- /node_modules`

src/index.js:

`import \_ from 'lodash';
import './style.css';

import Icon from './icon.png';

function component() {
const element = document.createElement('div');

// Lodash, now imported by this script
element.innerHTML = \_.join(['Hello', 'webpack'], ' ');
element.classList.add('hello');

// Add the image to our existing div.

const myIcon = new Image();

myIcon.src = Icon;

element.appendChild(myIcon);

return element;
}

document.body.appendChild(component());`

src/styles.css:

`.hello {
color: red;

background: url('./icon.png');
}`

creata a new build:
`npm run build`
