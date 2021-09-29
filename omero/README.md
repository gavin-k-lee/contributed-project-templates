# RENKU desktop for Image Processing

This project showcases the latest VNC (virtual desktop) that RENKU comes with. Feel free to fork this project and have a look around!

We include a copy of Fiji and an OMERO plug-in. Napari is also installed with a sample notebook to show its use.

The installation of Fiji is done 'on the fly' - that is we pull the latest Fiji version available at their website. Since Fiji updates very often, you can also include a copy of `Fiji.app` by extracting the Linux version of Fiji you would like to use, for example in this repository: https://renkulab.io/gitlab/lee.gavin.k/imagej. If you do that, you will need to make sure to edit the .gitignore file to upload binaries and jar files: https://renkulab.io/gitlab/lee.gavin.k/imagej/-/blob/master/.gitignore.
