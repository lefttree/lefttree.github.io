### Detect HTML5 Features

There are four basic techniques for detecting whether a browser supports a particular feature. From simplest to most complex:

1. Check if a certain property exists on a global object (such as window or navigator).

```
function supports_geolocation() {
  return 'geolocation' in navigator;
}
```

Instead of writing this function yourself, you can use [Modernizr](http://diveintohtml5.info/detect.html#modernizr)

```
if (Modernizr.geolocation) {
  // let's find out where you are!
} else {
  // no native geolocation support available :(
  // try geoPosition.js or another third-party solution
}
```

2. Create an element, then check if a certain property exists on that element.
3. Create an element, check if a certain method exists on that element, then call the method and check the value it returns.
4. Create an element, set a property to a certain value, then check if the property has retained its value.

#### Modernizr

Detects support for many HTML5 & CSS3 features.

check for canvas text support

```
if (Modernizr.canvastext) {
  // let's draw some text!
} else {
  // no native canvas text support available :(
}
```

check for HTML5 video formats

```
if (Modernizr.video) {
  // let's play some video! but what kind?
  if (Modernizr.video.webm) {
    // try WebM
  } else if (Modernizr.video.ogg) {
    // try Ogg Theora + Vorbis in an Ogg container
  } else if (Modernizr.video.h264){
    // try H.264 video + AAC audio in an MP4 container
  }
}
```

check for localStorage

```
if (Modernizr.localstorage) {
  // window.localStorage is available!
} else {
  // no native support for local storage :(
  // try a fallback or another third-party solution
}
```

......