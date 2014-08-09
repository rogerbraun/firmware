# Freifunk Firmware Berlin

For the Berlin Freifunk firmware we use vanilla OpenWRT with additional patches
and packages. The *scripts* dir has some util scripts to automate firmware
creation and apply patches / integrate custom freifunk packages. All custom
patches are located in *patches/* and all additional packages can be found at
http://github.com/freifunk/packages-berlin.

## HowTo

```
git clone https://github.com/freifunk-berlin/firmware.git
cd firmware
make
```

Then the ImageBuilder files end up in the directory `bin`.

## Required packages
### Ubuntu/Debian
```
apt-get install git subversion build-essential libncurses5-dev zlib1g-dev gawk \
  unzip libxml-perl flex wget gawk libncurses5-dev gettext quilt
```
## Builds & continuous integration

The firmware is [built
automatically](http://ubildbot.berlin.freifunk.net/one_line_per_build) by our [buildbot farm](http://buildbot.berlin.freifunk.net/buildslaves). If you have a bit of CPU+RAM+storage capacity on one of your servers, you can provide a buildbot slave (see [berlin-buildbot](https://github.com/freifunk/berlin-buildbot)).

## Patches with quilt

**Important:** all patches should be pushed upstream!

If a patch is not yet included upstream, it can be placed in the `patches` directory with the `quilt` tool. Please configure `quilt` as described in the [openwrt wiki](http://wiki.openwrt.org/doc/devel/patches) (which also provides a documentation of `quilt`).

### Add, modify or delete a patch

In order to add, modify or delete a patch run:
```bash
make clean pre-patch
```
Then switch to the openwrt directory:
```bash
cd openwrt
```
Now you can use the `quilt` commands as described in the [openwrt wiki](http://wiki.openwrt.org/doc/devel/patches).

#### Example: add a patch
```bash
quilt push -a                 # apply all patches
quilt new 008-awesome.patch   # tell quilt to create a new patch
quilt edit somedir/somefile1  # edit files
quilt edit somedir/somefile2
quilt refresh                 # creates/updates the patch file
```

## Submitting patches

### openwrt

Create a commit in the openwrt directory that contains your change. Use `git
format-patch` to create a patch:

```
git format-patch origin
```

Send a patch to the openwrt mailing list with `git send-email`:

```
git send-email \
  --to=openwrt-devel@lists.openwrt.org \
  --smtp-server=mail.foo.bar \
  --smtp-user=foo \
  --smtp-encryption=tls \
  0001-a-fancy-change.patch
```

Additional information: https://dev.openwrt.org/wiki/SubmittingPatches
