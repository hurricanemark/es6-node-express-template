# How to enable ES6 (and beyond) syntax with Node and Express

Reference: [https://www.freecodecamp.org/news/how-to-enable-es6-and-beyond-syntax-with-node-and-express-68d3e11fe1ab/](https://www.freecodecamp.org/news/how-to-enable-es6-and-beyond-syntax-with-node-and-express-68d3e11fe1ab/)

## 1. Project Setup

npx express-generator your-project-name --no-view

cd your-project-name

## 2. Installing Packages and Moving and Deleting Files

npm install

npm install -g win-node-env //for windows IDE

create a server/ folder

Put bin/ , app.js , and routes/ inside the server/ folder.

Rename www, found in bin to www.js

Leave public/ folder at your project root.

### Converting to ES6 code

Converting the generated code to ES6 is a little bit tedious, so just post the code here and feel free to copy and paste it.

Code for bin/www.js:

Now, because we modified the file structure, our start server script won&#39;t work. Here&#39;s what we&#39;ll do to fix it. On your package.json file, rename start script to server found in a JSON Object called &quot;scripts&quot;

```
// package.json

{

&quot;name&quot;: &quot;your-project-name&quot;,

// ....other details

&quot;scripts&quot;: {

&quot;server&quot;: &quot;node ./server/bin/www&quot;

}

}
```

Try it! Try running the server by typing npm run server on your terminal, and go to localhost:3000 on your browser.

## 3. Converting the top level code to use ES6 imports

Converting the generated code to ES6 is a little bit tedious, so just post the code here and feel free to copy and paste it.

Code for bin/www.js:

```
// bin/www.js

/\*\*

\* Module dependencies.

\*/

import app from &#39;../app&#39;;

import debugLib from &#39;debug&#39;;

import http from &#39;http&#39;;

const debug = debugLib(&#39;your-project-name:server&#39;);

// ..generated code below.
```

Almost all of our modifications are only at the top and bottom of the files. We are leaving other generated code as is.

Code for routes/index.js and routes/users.js:

// routes/index.js and users.js

```
import express from &#39;express&#39;;

var router = express.Router();

// ..stuff below

export default router;
```

Code for app.js:

```
// app.js

import express from &#39;express&#39;;

import path from &#39;path&#39;;

import cookieParser from &#39;cookie-parser&#39;;

import logger from &#39;morgan&#39;;

import indexRouter from &#39;./routes/index&#39;;

import usersRouter from &#39;./routes/users&#39;;

var app = express();

app.use(logger(&#39;dev&#39;));

app.use(express.json());

app.use(express.urlencoded({ extended: false }));

app.use(cookieParser());

app.use(express.static(path.join(\_\_dirname, &#39;../public&#39;)));

app.use(&#39;/&#39;, indexRouter);

app.use(&#39;/users&#39;, usersRouter);

export default app;
```

In app.js , because we left public/ at the project root , we need to change the Express static path one folder up. Notice that the path &#39;public&#39; became &#39;../public&#39; .

app.use(express.static(path.join(\_\_dirname, &#39;../public&#39;)));

## 4. Setting up Scripts

In setting up scripts, each script performs a different role. And we reuse each npm script. And for our development and production environments, they have a different configuration. (Almost identical, you�ll see later) That�s why we need to compose our scripts so we can use them without repeatedly typing the same stuff all over again.

Install `npm-run-all`

Since some terminal commands won�t work on windows cmd, we need to install a package called npm-run-all so this script will work for any environment. Run this command in your terminal project root.

`npm install --save npm-run-all`

## 5. Install babel, nodemon, and rimraf

Babel is modern JavaScript transpiler. A transpiler means your modern JavaScript code will be transformed to an older format that Node.js can understand. Run this command in your terminal project root. We will be using the latest version of babel (Babel 7+).

Note that Nodemon is our file watcher and Rimraf is our file remover packages.

`npm install --save @babel/core @babel/cli @babel/preset-env nodemon rimraf`

Adding transpile script

Before babel starts converting code, we need to tell it which parts of the code to translate. Note that there are a lots of configuration available, because babel can convert a lot of JS Syntaxes for every different kinds of purpose. Luckily we don&#39;t need to think about that because there&#39;s an available default for that. We are using default config called as preset-env (the one we installed earlier) in our package.json file to tell Babel in which format we are transpiling the code.

Inside your package.json file, create a &quot;babel&quot; object and put this setting.

```
// package.json

{

// .. contents above

&quot;babel&quot;: {

&quot;presets&quot;: [&quot;@babel/preset-env&quot;]

},

}
```

After this setup we are now ready to test if babel really converts code. Add a script named transpile in your package.json:

```
// package.json

&quot;scripts&quot;: {

&quot;start&quot;: &quot;node ./server/bin/www&quot;,

&quot;transpile&quot;: &quot;babel ./server --out-dir dist-server&quot;,

}
```

Now what happened here? First we need to run the cli command babel , specify the files to convert, in this case, the files in server/ and put the transpiled contents in a different folder called dist-server in our project root.

You can test it by running this command

`npm run transpile`

You&#39;ll see a new folder pop up.

### Extra: (github)

git remote add origin https://github.com/hurricanemark/es6-node-express-template.git

git branch -M main

git push -u origin main