---
tags: [javascript/three.js]
title: Three.js 4 - Working with Materials
created: '2019-08-29T14:13:08.738Z'
modified: '2019-08-29T14:13:18.183Z'
---

# Three.js 4 - Working with Materials

## Normal Materials
* The basic material gives a uniform colour to each point on the geometry’s surface.
	* You can add transparency in defining the mesh material.

### Normals

* Every plane has a normal - a vector that is perpendicular to the plane (it has a 90 degree angle with the plane).
* The normal material maps a normal vector to RGB colour.
* For each face the material gives a colour according to its current normal.
* There is a normals helper which has the code of:

```javascript

normals = new THREE.FaceNormalsHelper(cube, 5);
scene.add(normals);
normals.update();
```

For a sphere, each one of the triangles it’s composed of has it’s own vector, therefore it’s own normal.

To add normal material:

`let material = new THREE.MeshNormalMaterial();`


## Depth Materials
* The depth material gives a geometry grey-scale colours based on the distance from the camera.
* Closer points are darker, further away points are whiter.

`let material = new THREE.MeshNormalMaterial();`


## Line and Points Material
* The Line Material shows only the outline of the shape, some kind of skeleton.
* To use this we have to use a Line mesh instead of the usual Mesh object.

```javascript
let material = new THREE.LineBasicMaterial({color: 0xffffff, line width: 1});
cylinder = new THREE.Line(geometry, material);
```


### Dash Material

By adding in `dashSize: 1, gapSize: 1`,  as well as `geometry.computeLineDistances();`  you can use dashed lines.


### Points Material

```javascript
let material = new THREE.PointsMaterial({color: 0xffffff});
cylinder = new THREE.Points(geometry, material);
```


## Light Sensitive Material

### MeshLambertMaterial

* A material for non-shiny surfaces like stone. It gives the surface a dull look and with light it becomes brighter.
* Based on the Lambertian Model for modelling lights reflection.
* Emissive is like a glow from the material that is unaffected by light. 
	* The emissive property is the emissive colour of the material.
	* The default is null meaning that by default the material doesn’t emit colour.
	* The emissiveIntensity property defines how intense the emissive colour will be.
		* This is a number between 0 (no emissive) and 1 (full emissive). 
		* The default is 1.

```javascript
let material = new THREE.MeshLambertMaterial({
	color: 0xffffff,
	emissive: 0x0455ef,
	emissiveIntensity: 0.2
});
cylinder = new THREE.Points(geometry, material);
```


### Meshpong Material

* A material for shiny surfaces with specular effect (like mirrors).
* Shininess is how much the material shines. 
	* Lower values mean less shining.
	* Default value is 30.
* Specular defines the colour of the shine.
	* The default value is 0x111111 (dark grey).

```javascript
let material = new THREE.MeshPhongMaterial({
	color: 0xffffff,
	emissive: 0x0455ef,
	emissiveIntensity: 0.2,
	shininess: 30,
	specular: 0x111111,
});
cylinder = new THREE.Points(geometry, material);
```

### MeshStandard Material

* Based on a physical material model.
* This material uses a metallic-roughness model to simulate real materials.
* Combines both the previous materials.
* Defines how much the material will be like metal. Non-metallic materials like wood use 0 for this value, and metallic surfaces use 1.
	* The default is 0.5.
* Another property is *roughness* which defines how rough the material appears.
	* 0 is for a smooth mirror reflection, 1 for fully diffuse.

```javascript
let material = new THREE.MeshPhongMaterial({
	color: 0xffffff,
	emissive: 0x0455ef,
	emissiveIntensity: 0.2,
	metalness: 0.5
});
cylinder = new THREE.Points(geometry, material);
```
