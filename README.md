# Asynchronous jQuery

Asynchronous programming is becoming more and more important. However, not everyone is aware of the fact that [jQuery has had asynchronous support since version 1.5](http://api.jquery.com/category/deferred-object/) 

This sample shows how to do asynchronous programming in jQuery using existing asynchronous methods and also how to create your own asynchronous methods.

## Implementation
The sample contains of a single page ([index.html](index.html)) that contains three buttons that shows three different examples of asynchronous methods in jQuery. 

The first example is the most basic one, where we use the Deferred object returned by `$.getJSON` and setup its `done` method to show the data when it has been retrieved:

```javascript
$.getJSON('Data/example1.json').done(function (content1) {
    alert('Data retrieved from example1.json: ' + content1);
});
```

The second example shows how you can use the `$.when` method to create a Deferred object that wraps one or more Deferred objects. This is very useful if you want to wait for several Deferred objects to be done before continuing. In our example, we wrap two calls to `$.getJSON` into a single Deferred object using `$.when`:

```javascript
$.when($.getJSON('Data/example2.json'), $.getJSON('Data/example3.json')).done(function (content2, content3) {
    alert('Data retrieved from example2.json: ' + content2[0]);
    alert('Data retrieved from example3.json: ' + content3[0]);
});
```

The third example shows how to create your own custom Deferred object. To do this, first you have create a Deferred object using `$.Deferred`. Then you need to setup your asynchronous action, which should call  `resolve` on the Deferred object when it finishes For our example, we use the `setTimeout` function as our asynchronous action. When the timeout has finished, we call the `resolve` method of our custom Deferred object. This causes the `done` function on the `$.when` call to be called:

```javascript
// Create the deferred object
var deferred = $.Deferred();

// Set the timeout for 1 seconds and when the timeout occurs, we call the
// resolve() method of the deferred object. The resolve() method changes the
// state of the deferred object to done, which means that it's done() method
// is then called.
setTimeout(deferred.resolve, 1000);

// We wait for the deferred object to get in the done state
$.when(deferred).done(function () {
    alert('timeout passed for example 3');
});
```

## License
[Apache License 2.0](LICENSE.md)
