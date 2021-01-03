---
tags: [javascript/three.js]
title: Three.js 5 - Lights
created: '2019-08-29T14:13:39.806Z'
modified: '2019-08-29T14:13:43.781Z'
---

# Three.js 5 - Lights
#PD - Three.js# #javascript/three.js#

## Ambient Light
* An ambient light is a light that illuminates everywhere in the same level. It doesn’t come from a specific direction.
* We usually use it to light the whole scene with a constant colour so there will be no positions that have no light.

```javascript

let light = new THREE.AmbientLight(color, intensity);
scene.add(light);
```

* the default for intensity is 1. 
	* To increase the value makes the scene brighter.
* with ambient light, shapes display in a dull manner as it doesn’t display from any particular location.


## Hemisphere Light
* The hemisphere light is similar to the ambient light in that it  doesn’t have a position or location.
* The hemisphere light source fades as it goes from the top (sky) to the bottom (ground).

```javascript

let light = new THREE.HemisphereLight(skyColor, groundColor, intensity);
scene.add(light);
```


## Directional Light
* The directional light is a light that pours from a light source and aims to a direction.
* It’s like the sun light - since the sun is far away, we can practically assume that all light rays are parallel.

```javascript

let light = new THREE.DirectionalLight(0xffffff);
light.position.y = 5;
light.position.z = 0;
light.position.x = 10;

lightHelper = new THREE.DirectionalLightHelper(light, 5, 0x000000);
//DirectionalLightHelper(object, size of guide, color)
scene.add(lightHelper);

scene.add(light);

```

* We can use `light.target` to target a particular object in the scene. 

## Point Light
* Point light is a light that radiates from a single point to all directions.
* It’s like a light bulb - the light fades as a function of the distance from the light source.
* Distance - distance from the light source in which the light has completely faded away.
* Decay - amount the light fades along the distance. Default is 1 but suggested to move it to 2.

```javascript

let light = new THREE.PointLight(
	color: 0xffffff,
	intensity: 1,
	distance: 0,
	decay: ,
);
light.position.y = 5;
light.position.z = 0;
light.position.x = 10;


scene.add(light);

```
