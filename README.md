This preprocessor uses [gulp-typescript transpiler](https://www.npmjs.com/package/gulp-typescript) a great plugin mainted by [ivogabe](https://github.com/ivogabe) and other 21 contributors. Among its best fetuares we highlight:

 - Very fast rebuilds by using incremental compilation
 - Very good error reporting handle



# How to install this plugin

First you need include reference to this plugin in your `package.json`, just write karma-typescript-preprocessor2 (note number **2** at the end):

```JavaScript
  {
    "devDependencies": {
      "karma": "^0.13.15",
      "karma-typescript-preprocessor2": "1.0.0"
    }
  }
```
You can also install by command line typing:

`$ npm install karma-typescript-preprocessor2 --save-dev`

## Configuration Options

Below we show a full featured example with all options that you can use to configure the preprocessor:

// karma.conf.js 
```javascript
module.exports = function(config) {
  config.set({
    preprocessors: {
      '**/*.ts': ['typescript']
    },
    typescriptPreprocessor: {
      // options passed to the typescript compiler 
      tsconfigPath: './tsconfig.json', //*obligatory
      
      tsconfigOverrides: {//*optional
        removeComments: false
      },
      // transforming the filenames 
      //you can pass more than one, they will be execute in order
      transformPath: [function(path) {//*optional
        return path.replace(/\.ts$/, '.js');
      }, [function(path) {
        return path.replace(/\.ts$/, '.js');
      }]
    }
  });
};
```
//tsconfig.json
```javascript
{
  "compilerOptions": {
    "noImplicitAny": false,
    "module": "amd",
    "noEmitOnError": false,
    "removeComments": true,
    "sourceMap": true,
    "listFiles": true,
    "experimentalDecorators": true,
    "target": "es5"
  },
  "exclude": [
    "node_modules",
    "wwwroot",
    "artifacts"
    ".git",
    ".vs"
  ]
}
```

By design ``karma-typescript-preprocessor2`` only allows primary configuration build by ``tsconfig.json`` as this way a lot of problems with typings and references are completed resolved, as compiler will use same ``basedir`` to resolve files, but you can always override (or include) new options by using ``tsconfigOverrides`` property.

## Unsuported typescript configuration options
As we use gulp-typescript to transpiler typescript code, we have same unsuported properties as they have, so:

 - Sourcemap options (sourceMap, inlineSources, sourceRoot)
 - rootDir - Use base option of gulp.src() instead.
 - watch - Use karma ``singleRun: false`` configuration instead.
 - project - See "Using tsconfig.json".
 - 
And obvious ones: help, version

## Plugin Options

Below there are list of plugin options

### transformPath: Function | Function[]

defualt value:
```
function(path){
 return path.replace(/\.ts$/, '.js');//replace .ts to .js from virtual path
}

```

It is used to change virtual path of served files. Some times it should be necessary to change virtual directory of a served file to permit tests, example:

Lets supose that you have the following folder hierarchy:

```
\basedir
 \src
   module
     file1.ts
     file2.ts
 \test
   module
     file1.spec.ts
     file2.spec.ts
```

If file1.spec.ts and file2.spec.ts reference file1.ts and file2.ts and you are using typescript ``module`` option, you will need change virtual directory ``test`` to ``src``, so modules referenced by *.specs.ts will be resolved sucefully, so to make that you just need write something like:

```
typescriptPreprocessor: {
  // options passed to the typescript compiler 
  tsconfigPath: './tsconfig.json', //*obligatory
  tsconfigOverrides: {//*optional
    removeComments: false
  },
  // transforming the filenames 
  // you can pass more than one, they will be execute in order
  transformPath: [function(path) {//
   return path.replace(/\.ts$/, '.js'); // first change .ts to js
 }, function(path) {
   return path.replace(/[\/\\]test[\/\\]/i, '/'); // change directory from test to /
  }]
}
```







