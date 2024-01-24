# Custom image creator for raspios

Downloadable `raspios.img` file comes with default settings - including disabled `ssh`. 
A common way to customize the flashed OS is to use `rpi-imager` tool. This tool flashes OS directly to 
a removable device like USB or SDCard. Another way of enabling `ssh` is to simply put an empty file named 
`ssh` or `ssh.txt` onto `rootfs` partition of the `*.img` file containing raspios installation package.

TPi2 board requires either male-male USB cable (which I don't have) to access CM4 filesystem or to use `tpi` CLI for
remote filesystem mounting (which did not work on Windows). Therefore accessing the filesystem directly after flashing
it onto CM4 is out of the picture.

Last way to make CM4 accessible immediately after flashing was to modify the `*.img` file itself and put the 
aforementioned empty `ssh` file onto `rootfs` partition of the `*.img` file.

And this is exactly what the script does.

PS: I tested this on `2023-12-11-raspios-bookworm-arm64-lite` only!

### Creating the modified image

#### Prerequisites

- Virtualbox
- Vagrant

#### Running the script

`vagrant destroy -f && vagrant up`

After successful build you will find the image in the `./sync/` directory

### Whole content is based on

- https://brettops.io/blog/custom-raspberry-pi-image-no-hardware/
- https://raspberrypi.stackexchange.com/questions/13137/how-can-i-mount-a-raspberry-pi-linux-distro-imag
- https://www.raspberrypi.com/news/raspberry-pi-bullseye-update-april-2022/

