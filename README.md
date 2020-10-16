# xeokit-fullprecision-alpha

[![npm version](https://badge.fury.io/js/%40xeokit%2Fxeokit-fullprecision-alpha.svg)](https://badge.fury.io/js/%40xeokit%2Fxeokit-fullprecision-alpha)

This [xeokit-fullprecision-alpha](https://github.com/xeokit/xeokit-fullprecision-alpha) repository is for early evaluation 
of xeokit's upcoming XKT V6 file format, which supports full-precision geometry. 

This repository is copied from the [rtc-coords](https://github.com/xeokit/xeokit-sdk/tree/rtc-coords) branch of xeokit v1.4.3.

It's only for evaluating the loading and navigation of full-precision models. Some other xeokit features, such as 
Annotation occlusion and section planes, are still being adapted to full-precision, but are close.

# What this Solves

WebGL (and most GPUs) only support geometry precision to ~7 numeric places. 

If an IFC model is positioned far from the origin of the World coordinate system, or if the IFC model is geographically 
large with fine details, then WebGL will exhibit rounding errors (called "jittering") when rendering it. 

This is a problem for **every browser-based BIM viewer except CesiumJS**, and will soon not be a problem for xeokit. 

Shown below is an example of jitter in a model loaded from ````XKT V3````:

![Peek-2019-10-22-10-57](https://xeokit.github.io/xeokit-fullprecision-alpha/images/jitter.gif)

And the same model loaded from ````XKT V6```` into this repo:

![Peek-2019-10-22-10-57](https://xeokit.github.io/xeokit-fullprecision-alpha/images/no_jitter.gif)
  

# Quick demo

BIMData have generously provided us with an IFC model (see: What this Solves) that is positioned far from the origin, resulting in 
geometry that has very large coordinate values, which therefore rely on full-precision in order to render without rounding 
jitter. 

I converted that model to ````XKT V6```` and am loading it with this example: [examples/loading_XKT_preciseCoordinates.html](https://xeokit.github.io/xeokit-fullprecision-alpha/examples/loading_XKT_preciseCoordinates.html).

While BIMData could have used certain  ````IfcConvert```` options (eg ````--center-model````) to center the model's coordinates and make them require lower-precision 
(see [the tutorial](https://github.com/xeokit/xeokit-sdk/wiki/Creating-Files-for-Offline-BIM#4-dealing-with-precision-loss)), 
 they (and many users) need such models in their original coordinates so that they can be loaded and aligned alongside other 
models from the same project or building site.   

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

Temporarily substitute this xeokit version in your project. Easiest way is probably just to copy the ````src```` 
directory into your project's ````node_modules/@xeokit/xeokit-sdk```` directory and then rebuild your binaries.

### 2. Use the latest version of xeokit-gltf-to-xkt to generate XKT V6 files
   
Download V0.0.4+ of [xeokit-gltf-to-xkt](https://github.com/xeokit/xeokit-gltf-to-xkt) and integrate that into your 
data pipeline.  
 
Run your conversion pipeline on a model that relies on full-precision geometry. Such models are those that are either 
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
 
