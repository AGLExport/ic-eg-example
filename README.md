# Container exsample images repo

This image is based on yocto thud release.  

## Building the BSP for M3 Starter Kit

Currently using R-CarGen3 BSP 3.21 for thud release.  

### Clone build tree.

Move to build directry  
imagetype := host.xml, guest-glibc-systemd.xml, guest-musl-systemd.xml  

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


### Install to SD card

TBD

## Building the BSP for H3 Starter Kit

TBD


