# Open source whole slide image preparation and viewing pipeline
An overiew of how to implement an open source virtual microscopy viewing solution.

# Obtaining the software
The software can be download as binaries for major platforms, or alternatively can be loaded using a package manager or compiled manually from source.

## Relevant Websites:
- VIPS (with OpenSlide): http://www.vips.ecs.soton.ac.uk/index.php?title=VIPS
- OpenSeaDragon: https://openseadragon.github.io/
- Jekyll: https://jekyllrb.com/

## Install Software

### _For Apple macOS_ 

__If utilizing the Homebrew package manager__

`download brew at (https://brew.sh/)`

__lipvips__

`brew install vips`

__jekyll (commandline tools, Ruby, and RubyGems are installed)__

`gem install jekyll`

__OpenSeaDragon__

`download .js files from GitHub (https://openseadragon.github.io/#download)`

## Obtain WSI

You can convert WSI scanned using most scanners with the OpenSlide bindings in libvips.  The following example is with a Leica (Aperio) format .SVS file.

__Using libvips to convert a .SVS file to the Deep Zoom format (https://en.wikipedia.org/wiki/Deep_Zoom)

`vips dzsave --tile-size XXX in_file_name.svs out_folder_name`

This command will generate a .dzi file, and a folder containing additional folders with the tiles images in .DZI format.

## Viewing the WSI

Once converted to the DeepZoom format, the WSI can be viewed in the browser using an HTML webpage and the OpenSeaDragon JavaScript library.

An example using a minimal HTML wrapper:

```
<html>
<div id="openseadragon1" style="width: 100%; height: 100%";></div>
<script src="/filelocation/openseadragon.min.js"></script>
<script type="text/javascript">
    var viewer = OpenSeadragon({
        id: "openseadragon1",
        prefixUrl: "/filelocation/openseadragonimages/",
        tileSources: "yourwsi.dzi",
        sequenceMode: false,
        autoHideControls: true,
        animationTime: 1.0,
        blendTime: 0.6,
        constrainDuringPan: true,
        maxZoomPixelRatio: 1,
        visibilityRatio: 1,
        zoomPerScrolli: 1,
        defaultZoomLevel: 1,
        showReferenceStrip: true,
        showNavigator:  true,
	    showFullPageControl: false 
    });
</script>
</html>
```

## Viewing the WSI from the Cloud

This example is provided using the Amazon S3 service, other major (Google or Azure) or minor cloud servies should also work.

* Establish an Amazon S3 bucket (https://aws.amazon.com/s3/)
* Upload your image tile containing folder, .dzi file, and .html wrapper.
* File permissions must be set so that all 3 items are read-access from the viewer location (or public)
* Slides can be viewed via a link (https://s3.amazonaws.com/oswsi/F0ba3OtXIUaZfZ6Y_Y8yMA.html) or embedded in a webpage (https://s3.amazonaws.com/oswsi/index.html)
