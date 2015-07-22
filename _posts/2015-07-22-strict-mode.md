---
layout: post
title: About strict mode
cover: cover.jpg
category: web
tags: [web, javascript]
---

###Why Strict mode(From MDN)

Strict mode makes several changes to normal JavaScript semantics.
    - strict mode eliminates some JavaScript silent errors by changing them to throw errors. 
    - strict mode fixes mistakes that make it difficult for JavaScript engines to perform optimizations: strict mode code can sometimes be made to run faster than identical code that's not strict mode. 
    - strict mode prohibits some syntax likely to be defined in future versions of ECMAScript.

###Avoid global strict mode

When you concatenate javascript, global strict mode may cause some problems, it may apply to all the javascript files.

> It is thus recommended that you enable strict mode on a function-by-function basis (at least during the transition period).
