---
tags: [javascript/three.js]
title: Three.js 2 - Setting Up the Environment
created: '2019-08-29T14:12:04.396Z'
modified: '2019-08-29T14:12:11.281Z'
---

# Three.js 2 - Setting Up the Environment

## Download and Use the Library
Enable Three.js with local file:

```html

<head>
	<title></title>
	<link rel="stylesheet" href="style.css">
	<script src="three.min.js"></script>
</head>

```

Enable Three.js from CDN:

Add it from a script source online.


## The Application’s Blueprints
The HTML part:

```html
<html>
	<head>
		<title> 0 - Blueprints</title>
		<link rel="stylesheet" href="style.css">
		<script src="three.min.js"></script>
	</head>
	<body>
		//This will just contain the 3D canvas. No other element is needed here.
	</body>
</html>
```

The JS Script part:

```javascript
<script>

let scene, camera, renderer;

//set up the environment
// initialise scene, camera, objects and renderer
let init = function() {
	//1. create the scene
	//...

	//2. create and locate the camera
	//...

	//3. create and locate the objects on the scene
	//...

	//4. create the renderer
	//...
};

//main animation loop - calls every 50-60ms.
let mainLoop = function() {
	requestAnimationFrame(mainLoop);
};
//
init();
mainLoop();

</script>
```

### Notes on the Animation Loop:

* If we want to show animation, we have to redraw the screen periodically.
* The browser automatically refreshes its presentation a few times in a second, so we can “ask” the browser to let us know whenever it refreshes.
* To do so we use the browser’s requestAnimationFrame function. The function gets a call back function as a parameter, which will be activated in every refresh.


## Basic Elements of a 3D Application

* *The Scene* - The stage on which the show takes place.
* *The Camera* - A watching mechanism through which we see the scene.
	* The camera can move position and be rotated.
* *The Renderer* - This object interacts with the HTML and enables us to see the application we’ve made on the browser.
	* The thing is to take the scene that shows the application, and the camera that sets the point of view, and locate them both on the HTML.


## Showing a First Scene

```javascript
<script>

let scene, camera, renderer;

//set up the environment
// initialise scene, camera, objects and renderer
let init = function() {
	//1. create the scene
	scene = new THREE.scene();
	scene.background = new THREE.color(0xababab);

	//2. create and locate the camera
	camera = new THREE.PerspectiveCamera(30, window.innerWidth / window.innerHeight, 1, 1000);
	camera.position.z = 5;

	//3. create and locate the objects on the scene
	renderer = new THREE.WebGLRenderer();
	renderer.setSize(window.innerWidth, window.innerHeight);

	document.body.appendChild(renderer.domElement);

	//4. create the renderer
	//...
};

//main animation loop - calls every 50-60ms.
let mainLoop = function() {
	requestAnimationFrame(mainLoop);
};
//
init();
mainLoop();

</script>
```

