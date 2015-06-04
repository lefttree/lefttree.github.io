---
layout: post
title: HTML5 Storage & Cache
category: web
tagline: "HTML5"
tag: [web, html5]
---

[HTML5 Rocks Slide](http://slides.html5rocks.com/)
[Dive into HTML5](diveintohtml5.info)

### Offline/Storage

```
// use localStorage for persistent storage
// use sessionStorage for per tab storage
saveButton.addEventListener('click', function () {
  window.localStorage.setItem('value', area.value);
  window.localStorage.setItem('timestamp', (new Date()).getTime());
}, false);
textarea.value = window.localStorage.getItem('value');
```

you can use Modernizr to detect support for HTML5 Storage.

```
if (Modernizr.localstorage) {
  // window.localStorage is available!
} else {
  // no native support for HTML5 storage :(
  // maybe try dojox.storage or a third-party solution
}

```
>The data is actually stored as a string.

`setItem()` and `getItem()`
`removeItem()` and `clear()`

Track when storage changes

```
if (window.addEventListener) {
  window.addEventListener("storage", handle_storage, false);
} else {
  window.attachEvent("onstorage", handle_storage);
};
```

“5 megabytes” is how much storage space each origin gets by default.


### Web SQL Database

```
var db = window.openDatabase("DBName", "1.0", "description", 5*1024*1024); //5MB
db.transaction(function(tx) {
  tx.executeSql("SELECT * FROM test", [], successCallback, errorCallback);
});
```

###IndexedDB

Act like a dictionary to me
>the example is not complete

### Application Cache

The home page of the offline web application points to this list, called a manifest file, which is just a text file located elsewhere on the web server. 
A web browser that implements HTML5 offline applications will read the list of URLs from the manifest file, download the resources, cache them locally, and automatically keep the local copies up to date as they change. When the time comes that you try to access the web application without a network connection, your web browser will automatically switch over to the local copies instead.

An offline web application revolves around a cache manifest file. 

`
<html manifest="cache.manifest">
`

```
window.applicationCache.addEventListener('updateready', function(e) {
  if (window.applicationCache.status == window.applicationCache.UPDATEREADY) {
    window.applicationCache.swapCache();
    if (confirm('A new version of this site is available. Load it?')) {
      window.location.reload();
    }
  }
}, false);
```

```
CACHE MANIFEST
# version 1.0.0

CACHE:
/html5/src/logic.js
/html5/src/style.css
/html5/src/background.png

NETWORK:
*
```


Your cache manifest file can be located anywhere on your web server, but it must be served with the content type **text/cache-manifest**.

>Every page of your web application needs a manifest attribute that points to the cache manifest for the entire application.

#### manifest file

Divided into three parts: the “explicit” section, the “fallback” section, and the “online whitelist” section. If no section headers, default in the "explicit" section

>If multipage, should list all HTML pages in the manifest file

##### Network Sections

```
CACHE MANIFEST
NETWORK:
/tracking.cgi
CACHE:
/clock.css
/clock.js
/clock-face.jpg
```

NETWORK: the beginning of the "online whitelist" section, resources in this section are never cached and are not avaiable offline
CACHE: the beginning of the "explicit" section

##### Fallback Sections

```
CACHE MANIFEST
FALLBACK:
/ /offline.html
NETWORK:
*
```

> This is important. It means that you can have an offline web application that “lazily” adds pages as you visit them. You don’t need to list every single one of your HTML pages in your cache manifest.

The single character (/) will match any page on your site, not just the home page. 

If your browser doesn’t find the page in the appcache, instead of displaying an error message, it will display the page /offline.html

>Is this example complete? No. Wikipedia is more than HTML files. It uses common CSS, JavaScript, and images on each page. Each of these resources would need to be listed explicitly in the CACHE: section of the manifest file, in order for pages to display and behave properly offline.

##### window.applicationCache

Events:
* downloading
* progress
* cached
* noupdate
* updateready
* error

##### Developing & Debugging

how your browser checks whether a cache manifest file has changed. 
**3-phase process**

1. Via normal HTTP semantics, your browser will check whether the cache manifest has expired. 
Just like any other file being served over HTTP, your **web server** will typically include **meta-information** about the file in the HTTP response headers. Some of these HTTP headers (Expires and Cache-Control) tell your browser how it is allowed to cache the file without ever asking the server whether it has changed. 
2. If the cache manifest has expired (according to its HTTP headers), then your browser will ask the server **whether there is a new version, and if so, the browser will download it**. To do this, your browser issues an HTTP request that includes that **last-modified date** of the cache manifest, which your web server included in the HTTP response headers the last time your browser downloaded the manifest file. If the web server determines that the manifest file hasn’t changed since that date, it will simply return a 304 (Not Modified) status. 
3. If the web server thinks the manifest file has changed since that date, it will return an HTTP 200 (OK) status code, followed by the contents of the new file, along with new Cache-Control headers and a new last-modified date, so that steps 1 and 2 will work properly the next time. (HTTP is cool; web servers are always planning for the future. If your web server absolutely must send you a file, it does everything it can to ensure that it doesn’t need to send it twice for no reason.) **Once it’s downloaded the new cache manifest file, your browser will check the contents against the copy it downloaded last time.** If the contents of the cache manifest file are the same as they were last time, your browser won’t re-download any of the resources listed in the manifest.

**Debug**
Step 1:
So here’s one thing you should absolutely do: reconfigure your web server so that your cache manifest file is not cacheable by HTTP semantics.
>you should either qualify this with a <Files> directive so it only affects your cache manifest file, or create a subdirectory that contains nothing but this .htaccess file and your cache manifest file.

Step 2:
The easiest way I’ve found to accomplish this is to include a comment line with a revision number.
```
CACHE MANIFEST
# rev 43
clock.js
clock.css
```

