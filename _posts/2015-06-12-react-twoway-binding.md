---
layout: post
title: React Two-way Binding
cover: cover.jpg
category: Frontend
tags: [javascript, frontend]
---

#### input

valueLink

```javascript
var WithLink = React.createClass({
  mixins: [React.addons.LinkedStateMixin],
  getInitialState: function() {
    return {message: 'Hello!'};
  },
  render: function() {
    return <input type="text" valueLink={this.linkState('message')} />;
  }
});
```
> need to include react-with-addons.js

without valueLink

```javascript
var WithoutLink = React.createClass({
  mixins: [React.addons.LinkedStateMixin],
  getInitialState: function() {
    return {message: 'Hello!'};
  },
  render: function() {
    var valueLink = this.linkState('message');
    var handleChange = function(e) {
      valueLink.requestChange(e.target.value);
    };
    return <input type="text" value={valueLink.value} onChange={handleChange} />;
  }
});
```

#### select

no valueLink, use value and onchange event

```
<select text="Bandwidth" value={this.state.bw} changeE={this.bwChange}/>
```


