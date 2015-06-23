---
layout: post
title: Gulp Setup   
cover: cover.jpg
category: frontend
tags: [javascript, frontend, node]
---



`sudo npm install -g gulp   `

under the development dir
`npm install --save-dev gulp   `

`--save-dev` tells npm to add this module into package.json

install plugins

`npm install gulp-jshint gulp-sass gulp-concat gulp-uglify gulp-renam --save-dev`

example gulpfile.js

```javascript
//require gulp   
var gulp = require('gulp');  

//require plugins  
var jshint = require('gulp-jshint');
var sass = require('gulp-sass');
var concat = require('gulp-concat');
var uglify = require('gulp-uglify');
var rename = require('gulp-rename');

//lint task  
gulp.task('jshint',function(){
    gulp.src('./js/*.js')
    .pipe(jshint())
    .pipe(jshint.reporter('default'));
});

//conpile sass  
gulp.task('sass',function(){
    gulp.src('./scss/*.scss')
    .pipe(sass())
    .pipe(gulp.dest('./css'));
});  

//concat, minify   
gulp.task('scripts',function(){
    gulp.src('./js/*/js')
    .pipe(concat('all.js'))
    .pipe(gulp.dest('./dist'))
    .pipe(rename('all.min.js'))
    .pipe(uglify())
    .pipe(gulp.dest('./dist'));
});  

//default   
gulp.task('default',function(){
    gulp.run('lint','sass','scripts');
  
    gulp.watch('./js/*.js',function(){
        gulp.run('lint','scripts');0
    });
   
    gulp.watch('./scss/*.scss',function(){
        gulp.run('sass');
    });
});
```

Gulp's syntax is just like node, and it use streamming