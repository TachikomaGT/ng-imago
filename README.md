## Ng-Imago
---

**Ng-Imago is a module for AngularJS that aims to be a comprehensive library to manage images into your app.**

Currently it's focused on:

1. **[Multi resolutions sources](https://github.com/panurge-ws/ng-imago#multi-resolutions-sources) (responsive images)**
2. **[Sequential loading](https://github.com/panurge-ws/ng-imago#sequential-loading) (automatic or manual)**
3. **[Auto-resize](https://github.com/panurge-ws/ng-imago#auto-resize) to parent (fit / cover)**

## Requirements
---
Insert AngularJS in your head tags. Eg.:
```html
<script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.2.19/angular.min.js"></script>
```
## Install
---
Download the [production version][min] or the [development version][max].

[min]: https://raw.github.com/aboschini/ng-imago/master/dist/ng-imago.min.js
[max]: https://raw.github.com/aboschini/ng-imago/master/dist/ng-imago.js
**HTML:** import the script in your HTML
```html
<script src="[path_to_library]/ng-imago.js"></script>
```
**Javascript code:** inject the module in Angular
```javascript
var app = angular.module('app', ['ngImago']);
```
You can also configure the defaults value. See below ([Configuration](https://github.com/panurge-ws/ng-imago#configuration)).



### HOW TO USE
***

## Multi resolutions sources*
---
**Use ng-imago to load the images according to a given resolution, exactly as you usually do with Media queries inside CSS.**

In your HTML, insert a **img** tag and add the **ng-imago** directive. 
Set attibutes for each resolution you want to support, passing inside the attribute the url of the image.
```html
<img ng-imago      
     default="default.jpg" 
     small="small.jpg"
     medium="medium.jpg"  
     large="large.jpg"
     xlarge="xlarge.jpg"
     medium-portrait="medium-portrait.jpg"  />
```
For the default values for each attribute, please see below ([Defaults](https://github.com/panurge-ws/ng-imago#defaults-values)).

**Attention: you can't set the "src" attribute using Ng-Imago.** Why? See below.

### **Pixel density support (Retina)**

If you want to support different Pixel densities, you can simply set two URLs (comma separated) in the attribute: if one of them has the "**@2x**" suffix in the name, Ng-Imago will load that in screens with pixel density > 1.
```html
<img ng-imago
     medium="default.jpg,default@2x.jpg"  />
```
### **Orientation support**
_(Really beta... anyway...)_
If you set an attibute with a "-portrait" suffix, Ng-Imago will load that source if: 
1. the resolution matches the attribute (medium => (min-width: 768px) and
2. if the device orientation is portrait 
```html
<img ng-imago  
     medium="medium.jpg"  
     medium-portrait="medium-portrait.jpg"  />
```
### **Angular templates**

You can naturally use the AngularJS templates style to set the attibute's value. Those values are bindable, at least until the image is loaded. (See the [default settings unbind_when_loaded](https://github.com/panurge-ws/ng-imago#default-settings) below for further details).
```html
<img ng-imago  
     medium="{{url_medium}}"  
     large="{{url_large}}"  />
```
### **Overriding single image**
If you want follow its own rules you can pass to the ng-imago attribute a series of overriding values, for sizes or for settings.
```html
<img ng-imago="{ small:'only screen and (min-width:320px)'}"
     small="{{url_small}}-override" 
     medium="{{url_medium}}" 
     large="{{url_large}}" 
     xlarge="{{url_xlarge}}" 
            />
```

### **Using with no img elements**
You can use the directive ng-imago also in elements that aren't "img" tag, as "div", "span", etc... In few words, all the elements that accept a background-image style. The module will load an Image object and then it will pass the url to the background-image CSS style.
```html
<div style="width:500px;height:200px;overflow:hidden;"
     ng-imago
     auto-load="false"
     medium="medium.jpg"
     class="div_to_load"
     imago-resize="{scale:'cover', center:true}">
 </div>
```
If you are wondering why? "Can't we use simply the CSS?" Yes, you can, but you can't control sequential loading...


*this library uses matchMedia detection. Yes, there is the usual bla bla about IE8. You can use a [polyfill](https://github.com/paulirish/matchMedia.js/) if you need to support "browsers" that doesn't support matchMedia feature.



## Sequential loading
---
### (Complete documentation coming soon)
### Automatic queueing (AKA The "queue-index" attribute )
The queue-index attribute helps you to load images sequentially. It creates a queue of images based on the attribute's value.
If you set an attribute (queue-index="1"), the images with that attributes will be loaded after all the other images with no queue attribute (or with "queue-index=0") have been loaded. And so on... **Attention**: to start the loading sequence, you must insert at least a queue-index="0" image (or omit the attribute).
```html
<img  ng-imago 
      queue-index="1"
      default="delayed-default.jpg" 
      medium="delayed-medium.jpg" />
```
### Manual queueing / loading
You can set an attribute (auto-load="false") which prevents the image to be immediately loaded. You can load it later, as you need, with some methods.
```html
<img  ng-imago 
      auto-load="false" 
      id="img_111" 
      load-group="group1"
      queue-index="1234"
      class="myImageClass"
      default="delayed-default.jpg" 
      medium="delayed-medium.jpg" />
```
You can use Javascript methods provided in the service called 'ngImagoService'. You can load the image above with the following methods. Inject the service in your controller or directive.
```javascript
// load images with attibutes and values
ngImagoService.loadByAttribute("load-group", "group1");

// load the images with a specified class
ngImagoService.loadByClass("myImageClass");

// load an image by id
ngImagoService.loadImageById('img_111');

// load imags by queue-index (if auto-load="false")
ngImagoService.loadQueueIndex(1234);

// changing the auto-load to true
var img = document.getElementById('img_111');
angular.element(img).attr('auto-load',"true");

```

### Events

Ng-Imago dispatches some events related to loading actions via the $rootScope.
```javascript

// all the images in queue are loaded
$rootScope.$on("$ngImagoLoadQueueComplete",function(event, data){

});
// when a single image is loaded
$rootScope.$on("$ngImagoImageLoaded",function(event, data, element){

});

// when a queue-index group is loaded
$rootScope.$on("$ngImagoQueueIndexComplete",function(event, index, data, element){

});

```

## Auto resize
---
**(Complete documentation coming soon)**
Set an attibute "imago-resize" with self-explanatory object values.
```html
<div style="width:500px;height:300px">
  <img ng-imago 
       medium="{{url_medium}}"
       imago-resize="{scale:'cover', center:true}" />
</div>
```

## Configuration
---
**(Complete documentation coming soon)**

```javascript
var app = angular.module('app', ['ngImago']).config(["ngImagoProvider",
    // configuring defaults option and sizez
    function(ngImagoProvider) {
    	// see the defaults settings and sizes
    	console.log(ngImagoProvider.defaultsSettings());
    	console.log(ngImagoProvider.defaultsSizes()); 
    	// you can use the function as a setter to pass an object with you customized defaults
    	// E.g. defaultsSettings({avoid_cache:false, loaded_class:'my-loaded-class'});
    	// add a custom size attribute with custom media query
    	ngImagoProvider.addDefaultSize('custom', 'only screen and (min-width:400px)');
    }
]);
```

## Defaults values
---
## Default sizes
**(Complete documentation coming soon)**

```javascript
'default' => 'only screen and (min-width: 1px)'
'small'   => 'only screen and (min-width: 480px)'
'medium'  => 'only screen and (min-width: 768px)'
'large'   => 'only screen and (min-width: 1280px)'
'xlarge'  => 'only screen and (min-width: 1440px)'
```

## Default settings
### (Complete documentation coming soon)

```javascript
// Append a random string to the URL to force reload
avoid_cache: true,
// Remove the bindings of the attributes when the images is loaded for performance reasons
unbind_when_loaded: true,
// The name of the class added to the image when loaded
loaded_class: "ng-imago-loaded",
// The name of the class added to the image while it's loading
loading_class: "ng-imago-loading",
// The name of the class added to the image when an error has occurred
error_class: "ng-imago-error",

// auto-size
// Scale mode
scale: 'fit', // [fit | cover]
// Center the image
center: true, // center inside the container
// The container type to scale and/or center the image
container: 'parent' // ["parent" | "window"]
```



## This project is currently in beta version. Use it at your own risk.
### Reporting issues is much appreciated.
### Wait some days (or ask) before pulling requests.

## Thanks.

## License
Copyright (c) 2014 Panurge Web Studio  
Licensed under the MIT license.
