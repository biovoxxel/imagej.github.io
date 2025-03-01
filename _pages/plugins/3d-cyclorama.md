---
mediawiki:
- 3D_cyclorama
- 3Dcyclorama
title: 3D cyclorama
categories: [Digital Volume Unrolling, Digital Volume Flattening]
description: Fiji plugin to digitally unroll deformed tubes.
author: Charalambos Rossides
---

Fiji plugin to digitally unroll deformed tubes. If you use this plugin for your research or industrial applications we kindly ask you to cite our work [1]. 

[1] Rossides, C., Pender, S.L.F. & Schneider, P. "3D cyclorama for digital unrolling and visualisation of deformed tubes." Sci Rep 11, 14672 (2021).

[https://doi.org/10.1038/s41598-021-93184-x](https://doi.org/10.1038/s41598-021-93184-x)

## Download

Step 1 - enable the *3Dcyclorama* site:

- Start Fiji
- Go to Help > Update > Manage update sites
- Click *Add update site*. Name: 3D cyclorama, URL: https://sites.imagej.net/3Dcyclorama
- Check the newly added *3Dcyclorama* site
- Click *close*
- Click *Apply changes*

Step 2 - download dependencies:
- Create a directory named *FastDTW* in the Fiji plugins directory (Fiji.app/plugins/FastDTW)
- Download the FastDTW algorithm from github [https://github.com/rmaestre/FastDTW](https://github.com/rmaestre/FastDTW) (Click code > download zip)
- Extract the contents of FastDTW-master (FastDTW.iml, README.md, src, target, etc.) in Fiji.app/plugins/FastDTW

Step 3 - Run:
- Restart Fiji
- The 3D cyclorama plugin should appear in the Plugins menu towards the bottom in a menu titled **3D cyclorama**

## Tutorial

&lt; Help and tutorial coming soon &gt;

## ChangeLog
v1.0.0:

- Added functionality to flatten sheets and scrolls.
  
v0.6.0:
  
- Implemented sanity checks in GUI.
  
v0.5.0:

- Restricted 3Dcyclorama GUI access to unnecessary backend functionality.
  
v0.4.0:
  
- First mature GUI.
  
