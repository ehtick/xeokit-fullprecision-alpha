# xeokit-fullprecision-alpha

This repository is for early evaluation of xeokit's upcoming XKT V6 file format, which supports full-precision geometry. 

This repo is copied from the [rtc-coords](https://github.com/xeokit/xeokit-sdk/tree/rtc-coords) branch of xeokit v1.4.3.

It's only for evaluating the loading and navigation of full-precision models. Some other xeokit features, such as 
Annotation occlusion and section planes, are still being adapted to full-precision, but are close.

### What Works

These features are currently working with full-precison geometry:-

* Loading XKT V6 with ````XKTLoaderPlugin````
* Rendering models loaded with ````XKTLoaderPlugin````
* Camera interaction
* Picking

### What doesn't work yet

These features are not yet working with full-precision geometry (but do work OK with single-precision):

* Annotation auto-occlusion
* SectionPlanes and SectionPlanesPlugin

I have working experimental versions of these functionalities, but they need more simplification, profiling and testing 
before I can actually do a proper xeokit release with full-precision.  

## Usage

To evaluate XKT V6 within your application, do the following steps:

### 1. Temporarily substitute your xeokit with this repository

Temporarily substitute this xeokit version in your project. easiest way is probably just to copy the ````src```` 
directory into your project's ````node_modules/@xeokit/xeokit-sdk```` directory and then rebuild your binaries.

### 2. Use the latest version of xeokit-gltf-to-xkt to generate XKT V6 files
   
Download V0.0.4+ of [xeokit-gltf-to-xkt](https://github.com/xeokit/xeokit-gltf-to-xkt) and integrate that into your 
data pipeline.  
 
Run your conversion pipeline on a ````gltf2xkt```` that relies on full-precision geometry. Such models are those that are either 
positioned far from the World origin, or are just geographically large, with fine details. 

Be sure to run ````gltf2xkt```` with ````XKT V6```` selected as output using the tool's new ````-f```` option, eg:

`````bash
gltf2xkt -f 6 -i myModel.gltf -o myModel.xkt
`````  

By default, ````gltf2xkt```` generates XKT V3 by default. 

You can actually leave this version of ````gltf2xkt```` 
in your pipeline, since it's fully QA'd and is backward-compatible with the previous version. The ````XKT V6```` format is 
now quite stable.

Note that you could also just replace an XKT model (and its metadata) in one of the examples in this repository.
 

## A pre-prepared example

This repo contains a ready-to-run example of ````XKTLoaderPlugin```` loading an ````XKT V6```` model that I 
converted earlier. For a super quick demo, just run this 
example locally: [examples/loading_XKT_preciseCoordinates.html](https://github.com/xeokit/xeokit-sdk/blob/rtc-coords/examples/loading_XKT_preciseCoordinates.html).