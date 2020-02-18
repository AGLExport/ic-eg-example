# Container example images repo

This image is based on yocto thud release.  

----
## Building the Container exsample for M3 Starter Kit

Currently using R-CarGen3 BSP 3.21 for thud release.  

### Clone build tree.

Move to build directry  
imagetype := *host.xml*, *guest-glibc-systemd.xml*, *guest-musl-systemd.xml*  

	export WORK=`pwd`  
	repo init -u https://github.com/AGLExport/ic-eg-example.git -m imagetype  
	repo sync  

### Download proprietary driver modules to $WORK/proprietary folder.

Download from <https://www.renesas.com/us/en/solutions/automotive/rcar-download/rcar-demoboard-2.html>  
You should see the following files:  

	$ ls -1 $WORK/proprietary/*.zip  
	R-Car_Gen3_Series_Evaluation_Software_Package_for_Linux-weston5-20190802.zip  
	R-Car_Gen3_Series_Evaluation_Software_Package_of_Linux_Drivers-weston5-20190802.zip  


### Populate meta-renesas with proprietary software packages.

	export PKGS_DIR=$WORK/proprietary  
	cd $WORK/meta-renesas  
	sh meta-rcar-gen3/docs/sample/copyscript/copy_evaproprietary_softwares.sh -f $PKGS_DIR  
	unset PKGS_DIR  


### Setup build environment

	cd $WORK  
	source poky/oe-init-build-env  


### How to build

#### host environment

Copy bblayers.conf and local.conf to conf directry.  

	cp $WORK/meta-container-host-example/files/m3ulcb/*.conf ./conf/  

Build host environment.  

##### host-image-minimal : under development

Minimum image of container host based on core-image-minimal from Yocto.  It's using systemd. 

	bitbake host-image-minimal  

##### host-image-weston: available
Container host image based on core-image-weston supporting weston compositor.

This image support a guest image including graphics application. ex. guest-image-wayland.

It's using systemd.

	bitbake host-image-weston  


#### guest environment (using guest-glibc-systemd.xml)

Copy bblayers.conf and local.conf to conf directry.  

	cp $WORK/meta-container-guest-example/files/m3ulcb/*.conf ./conf/  

Build host environment.  

##### guest-image-minimal : under development

	bitbake guest-image-minimal  


##### guest-image-wayland : under development

	bitbake guest-image-wayland  

---
### Install to SD card

$MEDIA_DIR indicates the directory where the SD card is mounted.  

#### 1st step: host image

Please select one of the host images.  

##### host-image-minimal : under development

Extract host-image-weston archive and copy the DTB file.  

	cd $MEDIA_DIR
	sudo tar xvjf /path/to/host/build/tmp/deploy/images/m3ulcb/host-image-weston-m3ulcb.tar.bz2
	sudo cp /path/to/host/build/tmp/deploy/images/m3ulcb/r8a7796-m3ulcb.dtb ./boot/

##### host-image-weston: available

Extract host-image-weston archive and copy the DTB file.  

	cd $MEDIA_DIR
	sudo tar xvjf /path/to/host/build/tmp/deploy/images/m3ulcb/host-image-weston-m3ulcb.tar.bz2
	sudo cp /path/to/host/build/tmp/deploy/images/m3ulcb/r8a7796-m3ulcb.dtb ./boot/


#### 2nd step: guest images

Can install any number of guest images.  
**Attention : Some guests depend on a specific host image.  ex. guest-image-wayland depends on host-image-weston.**  

##### guest-image-minimal : under development

TBD  


##### guest-image-wayland : under development

Install guest image

	cd $MEDIA_DIR/lxc/guests
	sudo mkdir guest-wayland
	cd guest-wayland
	sudo tar xvjf /path/to/guest/build/tmp/deploy/images/m3ulcb-guest/guest-image-wayland-m3ulcb-guest.tar.bz2

Install agl-container-manager config

	sudo cp /path/to/guest/meta-container-guest-example/files/configs/guest-wayland.json $MEDIA_DIR/lxc/conf/


## Building the Container exsample for H3 Starter Kit

TBD  


