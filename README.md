# pro-uglifyjs-webpack-plugin

It's essentially a uglifyjs webpack plugin, but extended with extra option.

## Overview

You can replace `webpack.optimize.UglifyJsPlugin` with this plugin. Besides common usage of uglifyjs,
the new-added option `wrapCatch` enables you:

wrap `eval`:

    ...
    eval( 'var a = 5; console.log( a )' );
    ...

    =>

    ...
    try {
        eval( 'var a = 5; console.log( a )' );
    }
    catch ( e ) {
        throw Error( e );
    }
    ...

wrap `function body`:

    function a() {
        // function body goes here
    }

    => 

    function a() {
        try {
            // function body goes here
        }
        catch ( e ) {
            throw Error( e );
        }
    }

you should only:

1. set the `wrapCatch` option to true.
2. add directive `"use catch;"` where you need the plugin to wrap something for you.


## Installation

    npm install --save pro-uglifyjs-webpack-plugin


## Usage

> in `webpack.config.js`

    var proUglify = require( 'pro-uglifyjs-webpack-plugin' );
    config.plugins = config.plugins.concat( [
        new proUglify( {
            wrapCatch: true,
            compress: { ... },
            mangle: false,
            output: {
                comments: true
            }
        } )
    ] );

> in `file.js`

    "use catch";
    ...


