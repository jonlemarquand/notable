---
tags: [javascript/three.js]
title: Three.js 3 - Working with Geometry
created: '2019-08-29T14:12:24.813Z'
modified: '2019-08-29T14:12:41.278Z'
---

# Three.js 3 - Working with Geometry

## Working with Geometry
Three.js can work with many geometric shapes. We also have to decide on what material to wrap it with. This is done via an object called ‘mesh’.

The BoxGeometry represents a 3D box.

`let geometry = new THREE.BoxGeometry(1,1,1,);`

The constructor gets the box’s width, height and depth as parameters.

```javascript

let cube;

let createCube = function() {
	let geometry = new THREE.BoxGeometry(1,1,1,);
	let material = new THREE.MeshBasicMaterial({color:0x00a1cb});
	cube = new THREE.Mesh(geometry, material);
	scene.add(cube);
};

```

* The ‘Basic material’ here is opaque with no reflections.
* From this info, the constructor gets a JS object with the material’s properties.

## Locating the Cube
Unless it’s defined, the object will start at 0.

## The Sphere
With the code for a sphere:

`let geometry = new THREE.SphereGeometry(5, 30, 40);`

The Numbers are:

* The Radius
* Number of Horizontal Segments
* Number of Vertical Segments

### Phi - Horizontal Sweep (Around the X-Axis)

When three.js paints a sphere, it starts with a vector that its length is the radius and it creates a horizontal sweep.

By default this phi is 360 degrees. We can add start and end of the phi to the sphere:

`let geometry = new THREE.SphereGeometry(5, 30, 30, 0, Math.PI);`

### Theta - Vertical Sweep (Around the Y-Axis)

`let geometry = new THREE.SphereGeometry(5, 30, 30, 0, Math.PI, 0, Math.PI/2);`


## Torus
A round rubber ring like tube. It is defined as:

`let geometry = new THREE.TorusGeometry(5, 1, 20, 20, 2*Math.PI)`

1. The first number is the radius from the centre of the rubber ring to the the outer line.
2. The second is the tube diameter (How thick the tube will be)
3. Radius segments number - Higher number, the more round the disc will be.
4. Tubular segments - How circular the torus will be. Low number = more like a polygon.
5. Default value for a circular torus.

## Custom Geometry
The object geometry represents any kind of geometric shape in Three.js.  A geometric shape is made of vertices, each vertex is a 3D point.

Three vertices can make a face.

```javascript

let geometry = new THREE.Geometry();
geometry.vertices.push(new THREE.Vector3(3,0,0)); 
//vertices[0]
geometry.vertices.push(new THREE.Vector3(0,5,0)); 
//vertices[1]
geometry.vertices.push(new THREE.Vector3(0,0,2)); 
//vertices[2]

geometry.faces.push(new THREE.Face3(0,1,2));

let material = new THREE.MeshBasicMaterial({color: 0xffffff, side: THREE.DoubleSide});

let shape = new THREE.Mesh(geometry, material);
scene.add(shape);

```


## Text Geometry
* Three.js enables us to render text objects on the screen and treat them as 3D objects as any other.
* The text object should have a font loaded before we can show the text.
* Three.js comes with some font files that can be used.
* The FontLoader object would load a font jon file and parse it:

```javascript

let loader = new THREE.FontLoader();
let font = loader.parse(fontJSON);
let text = new THREE.TextGeometry("Hello World", {font: font, size:5, height: 1});

let material = new THREE.MeshBasicMaterial({color:0x034b59});
text = new THREE.Mesh(geometry, material);
text.position.x = -15;
scene.add(text);

```

* font - the font object that was loaded by the font loader.
* size - the font size, determines how big or small the text would be.
* height - gives the text a depth. Determines the letters thickness in the z-axis.



