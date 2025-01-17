---
title: "Viewing the geometry"
teaching: 10
exercises: 10
questions:
- "How can we view the geometry?"
objectives:
- "Understand how to export the geometry with `dd_web_display`."
- "Know of multiple way in which to open ROOT TGeo geometries."
keypoints:
- "The geometry, exported to the ROOT TGeo file, can be viewed with ROOT."
---

## Introduction

Before we move to discussion of the detector plugins in the next part, let's discuss visualization on your local system.

## ROOT visualization

The geometry included in `eic-shell` can be viewed with the ROOT geometry browser. However, we first need to export it from the built-in DD4hep format to the ROOT TGeo format, with a small utility program `dd_web_display`. To do this, we will need to ensure we are in a directory where we have write access, such as the directory `~/eic/`.
```console
$ cd ~/eic/
$ dd_web_display --export $DETECTOR_PATH/$DETECTOR_CONFIG.xml
```

> Note: If you are not inside `eic-shell`, you may need to be connected to the internet as your run this command since a magnetic fieldmap will need to be downloaded.
{: .callout}

The `dd_web_display` utility will create a ROOT file in the current directory that can be opened with the geometry viewer online at [https://eic.phy.anl.gov/geoviewer/](https://eic.phy.anl.gov/geoviewer/) or [https://root.cern/js/latest](https://root.cern/js/latest), or a local ROOT installation. VSCode also has a JSROOT extension which provides the same functionality as the browser. 

> Note: The version of JSROOT on https://eic.phy.anl.govgeoviewer/ is older than the https://root.cern/js/latest version so some differences in the visualisation may be present.
{: .callout}

The output file is by default named `detector_geometry.root` this can be changed if you want to look at several configurations using e.g.

```console
$ dd_web_display -o vertex_geometry.root --export $DETECTOR_PATH/epic_vertex_only.xml
```

For local ROOT installations, the following commands may be helpful:
```
TGeoManager::Import("detector_geometry.root");
gGeoManager->GetTopVolume()->Draw("ogl") 
```

The geometry viewer has to make decisions on what to draw in order to keep the number of facets small enough. This means that detectors with a large number of repeated components may not be drawn, or other detectors may not be drawn when those detectors are drawn. For this reason, we also have the subsystem-specific entry points xml files. For visualization of specific subsystems, these files are recommended.

Parameters describing how each component of the geometry should be vizualised are contained within the detector plugins and can be controlled through the xml description.

> Exercise:
> - Export a different detector configuration from the default `epic.xml`, and export this to ROOT TGeo format.
> - Open the exported ROOT file in the geometry viewer at [https://eic.phy.anl.gov/geoviewer/](https://eic.phy.anl.gov/geoviewer/) or [https://root.cern/js/latest](https://root.cern/js/latest).
{: .challenge}

## Geant4 visualization
**Warning might not work for various reasons** If you are used to the Geant4 geometry visualisation or want to visually inspect where a subset of your event sample this is still possible using npsim.

```console
  npsim --runType qt --compactFile $DETECTOR_PATH/epic_vertex_only.xml --inputFiles root://dtn-eic.jlab.org//work/eic2/EPIC/EVGEN/SIDIS/pythia6-eic/1.0.0/18x275/q2_0to1/pythia_ep_noradcor_18x275_q2_0.000000001_1.0_run9.ab.hepmc3.tree.root --macro macro/b0_vis.mac
```

This particular example uses pythia6 min-bias events stored on the xrootd server at jlab. The visualization of particle tracks and the detector is controlled by the `macro/b0_vis.mac` file

> Note: Geant4 does not try and limit its visualization of components so depending on your machine the full detector might struggle, please be patient. This performs much better for me outside of `eic_shell` but takes a long time to set up the environment.
{: .callout}


## Other visualisation you might want
ACTS Surfaces

Material Map

Event display

These are avaliable in various forms but not covered in this tutorial.
