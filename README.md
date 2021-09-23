https://www.freecodecamp.org/news/how-to-enable-es6-and-beyond-syntax-with-node-and-express-68d3e11fe1ab/

How to enable ES6 (and beyond) syntax with Node and Express

1. Project Setup 
================
npx express-generator your-project-name --no-view
cd your-project-name

2. Installing Packages and Moving and Deleting Files
====================================================
npm install

npm install -g win-node-env   //for windows IDE
	create a server/ folder
	Put bin/ , app.js , and routes/ inside the server/ folder.
	Rename www, found in bin to www.js
	Leave public/ folder at your project root.

Converting to ES6 code
Converting the generated code to ES6 is a little bit tedious, so I�ll just post the code here and feel free to copy and paste it.

Code for bin/www.js:

Now, because we modified the file structure, our start server script won�t work. Here�s what we�ll do to fix it. On your package.json file, rename start script to serverfound in a JSON Object called "scripts"

// package.json
{
  "name": "your-project-name",
  // ....other details
  "scripts": {
    "server": "node ./server/bin/www"
  }
}

Try it! Try running the server by typing npm run server on your terminal, and go to localhost:3000 on your browser.

3.  Converting the top level code to use ES6 imports
====================================================
Converting the generated code to ES6 is a little bit tedious, so I�ll just post the code here and feel free to copy and paste it.

Code for bin/www.js:

// bin/www.js
/**
 * Module dependencies.
 */
import app from '../app';
import debugLib from 'debug';
import http from 'http';
const debug = debugLib('your-project-name:server');
// ..generated code below.


Almost all of our modifications are only at the top and bottom of the files. We are leaving other generated code as is.

Code for routes/index.js and routes/users.js:

// routes/index.js and users.js
import express from 'express';
var router = express.Router();
// ..stuff below
export default router;


Code for app.js:

// app.js
import express from 'express';
import path from 'path';
import cookieParser from 'cookie-parser';
import logger from 'morgan';
import indexRouter from './routes/index';
import usersRouter from './routes/users';
var app = express();
app.use(logger('dev'));
app.use(express.json());
app.use(express.urlencoded({ extended: false }));
app.use(cookieParser());
app.use(express.static(path.join(__dirname, '../public')));
app.use('/', indexRouter);
app.use('/users', usersRouter);
export default app;



In app.js , because we left public/ at the project root , we need to change the Express static path one folder up. Notice that the path 'public' became '../public' .

app.use(express.static(path.join(__dirname, '../public')));

4. Setting up Scripts
=====================
In setting up scripts, each script performs a different role. And we reuse each npm script. And for our development and production environments, they have a different configuration. (Almost identical, you�ll see later) That�s why we need to compose our scripts so we can use them without repeatedly typing the same stuff all over again.

Install `npm-run-all`
Since some terminal commands won�t work on windows cmd, we need to install a package called npm-run-all so this script will work for any environment. Run this command in your terminal project root.

npm install --save npm-run-all

5. Install babel, nodemon, and rimraf
=====================================
Babel is modern JavaScript transpiler. A transpiler means your modern JavaScript code will be transformed to an older format that Node.js can understand. Run this command in your terminal project root. We will be using the latest version of babel (Babel 7+).

Note that Nodemon is our file watcher and Rimraf is our file remover packages.

npm install --save @babel/core @babel/cli @babel/preset-env nodemon rimraf

Adding transpile script
Before babel starts converting code, we need to tell it which parts of the code to translate. Note that there are a lots of configuration available, because babel can convert a lot of JS Syntaxes for every different kinds of purpose. Luckily we don�t need to think about that because there�s an available default for that. We are using default config called as preset-env (the one we installed earlier) in our package.json file to tell Babel in which format we are transpiling the code.

Inside your package.json file, create a "babel" object and put this setting.

// package.json
{  
  // .. contents above
  "babel": {
    "presets": ["@babel/preset-env"]
  },
}
After this setup we are now ready to test if babel really converts code. Add a script named transpile in your package.json:

// package.json
"scripts": {
    "start": "node ./server/bin/www",
    "transpile": "babel ./server --out-dir dist-server",
}
Now what happened here? First we need to run the cli command babel , specify the files to convert, in this case, the files in server/ and put the transpiled contents in a different folder called dist-server in our project root.

You can test it by running this command

npm run transpile

You�ll see a new folder pop up.



==================================
github:
git remote add origin https://github.com/hurricanemark/es6-node-express-template.git
git branch -M main
git push -u origin main
