---
layout: post
category: howto
---
<head>
    <link rel="icon" href="assets/img/freebsd-logo-vector.svg" type="image/svg+xml">
</head>
<img src="assets/img/freebsd-logo-vector.svg" style="height: 200px; padding: 25px;" alt="Disgruntled">

## Install FreeBSD
1. Download a [FreeBSD](https://www.freebsd.org/where) image for Raspberry Pi
1. Uncompress the image file, `xz -d`
1. Copy the image to the microSD card, `dd`
1. Download the [RPI5_D0.zip](https://github.com/NumberOneGit/rpi5-uefi/releases) file
1. Uncompress the zip file, `unzip RPI5_D0.zip -d RPI5_D0`
{% highlight diff linenos %}
bcm2712d0-rpi-5-b.dtb
bcm2712-d-rpi-5-b.dtb
bcm2712-rpi-500.dtb
bcm2712-rpi-5-b.dtb
bcm2712-rpi-cm5-cm4io.dtb
bcm2712-rpi-cm5-cm5io.dtb
bcm2712-rpi-cm5l-cm4io.dtb
bcm2712-rpi-cm5l-cm5io.dtb
config.txt
overlays
RPI_EFI.fd
{% endhighlight %}
1. Copy the unzipped contents to the root of the microsd card.
1. Turn on the Raspberry Pi

## Configure
### Default login
{% highlight diff linenos %}
+user: root
+Password: root
{% endhighlight %}
### Network