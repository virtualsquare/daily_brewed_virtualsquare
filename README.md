# Freshly Brewed Virtualsquare
Dynamically create a disk image to test the latest developments of Debian sid and virtualsquare

The script `brew_v2` downloads the latest version of SID/unstable from the 
[Debian Official Cloud Images](http://cdimage.debian.org/cdimage/cloud/)
and customizes it to be used as a Tutorial environment for the 
[Virtualsquare projects](http://wiki.virtualsquare.org/#!index.md).

## Requirements

`brew_v2` should need the following packages:

```bash
$ apt-get install qemu-kvm qemu-utils parted libguestfs-tools
```

## Usage

Simply run `brew_v2`:

```bash
$ ./brew_v2
```

At the end the script creates a disk image file like:
```
debian-sid-v2-amd64-daily-20200526-275.qcow2
```

(20200526 is the date and 275 is the version number of the latest version
so it will be updated consistently)

This image can be used to start a kvm virtual machine. e.g.:
```bash
$ kvm -smp 8 -drive file=debian-sid-v2-amd64-daily-20200526-275.qcow2 -m 1G \
    -monitor stdio -netdev type=user,id=net,hostfwd=tcp::2222-:22 \
    -device virtio-net-pci,netdev=net
```

When the VM has booted log in as root (password `virtualsquare`) and run the following
command:
```bash
# ./get_v2all
```

This command installs all the packets needed by the virtualsquare projects and then downloads, builds and
installs all the altest versions of the projects directly from the development repositories.

## Log in and try your experiments

Virtualsquare tools do not generally require root access. So an unprivileged user named `virtualsquare` (password `virtualsquare`) has been added for the tests.

It is possible to log-in on the console or by ssh (a tunnel is provided as localhost at port 2222):

```bash
$ ssh -p2222 -X virtualsquare@localhost
```

This example provides also a X-window forwarding service.

In case you update the disk image (e.g. using `brew_v2` again) ssh can complain that the host key has changed. In case delete the previous key:
```
$  ssh-keygen -R "[localhost]:2222"
```

