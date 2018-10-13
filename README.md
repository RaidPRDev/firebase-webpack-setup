# webpack-setup
Bundle the firebase sdk with webpack.  For reference

## Prerequisites

#### Node.js

Before you can start, you need to have Node.js 8.0.0 or greater installed on your machine.
To download Node.js visit https://nodejs.org/en/download/.

#### webpack.js
Get familiar with webpack go to https://webpack.js.org
We will install webpack in the next section


## Project Setup
Create a directory, initialize npm, install webpack locally, and install the webpack-cli 

Open your Git Bash terminal or similar and 
> mkdir firebase-lib && cd firebase-lib

Setup package, creates package.json
> npm init -y

Install webpack and update the dev dependencies    
> npm install webpack@0.0.0 webpack-cli@0.0.0 --save-dev

Install firebase and update the project dependencies    
> npm install firebase@0.0.0 --save 

Create the following folder structure and files
```
firebase-lib
├── dist                                # build release folder
├── node_modules                        # firebase and webpack dependencies
└── src
    ├── index.js                        # entry point javascript 
├── package.json                        # npm configuration 
└── webpack.config.js                   # webpack configuration 
```

### webpack.config.js
Before we set this up, we will need a few nice webpack plugins 

Install CleanWebpackPlugin, it cleans up the output folder when building
> npm install clean-webpack-plugin --save-dev 

Install UglifyJsPlugin, gives us nice options to minimize the library
> npm install uglifyjs-webpack-plugin --save-dev 

Our config file should look something similar to this
```javascript
const path = require('path');
const CleanWebpackPlugin = require('clean-webpack-plugin');
const UglifyJsPlugin = require('uglifyjs-webpack-plugin');

module.exports = {
    mode: "none",
    entry: './src/index.js',
    optimization: {
        minimizer: []
    },
    plugins: [
        new CleanWebpackPlugin(['dist']),
        new UglifyJsPlugin({uglifyOptions:{
            compress: false,
            output: {
                comments: false,
                beautify: false,
            }
        }})
    ],
    output: {
        path: path.resolve(__dirname, 'dist'),
        filename: 'firebase.js',
        library: 'firebaseSDK'
    }
};
```

### package.json
Our package.json will look something simuilar to this
```
{
  "name": "LibraryName",
  "version": "0.0.1",
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
    "webpack": "^4.16.2",
    "webpack-cli": "^3.1.0",
    "clean-webpack-plugin": "^0.1.19",
    "uglifyjs-webpack-plugin": "^0.0.0"
  },
  "dependencies": {
    "firebase": "~5.5.3"
  }
}
```

### src/index..js
Our entry point javascript code should look similar to this
```javascript
const firebase = require('firebase/app');
require('firebase/auth');
require('firebase/database');

// additional services that you want to use
// require("firebase/firestore");
// require("firebase/storage");
// require("firebase/messaging");
// require("firebase/functions");

export { firebase };
```

##  Build library
Open your Git Bash terminal or similar and run the following command in your terminal     
> npm run build

That's it. The new javascript library should be in your output folder.




