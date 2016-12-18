# ABOUT

All the full resolution channel logos and their link to the actual channel (=serviceref) are kept up2date in this repository. The end result are picons for Enigma2 tuners and Kodi mediacenter in combination with a compatible PVR backend.

# BUILDING THE PICONS

[Ubuntu](http://www.ubuntu.com/download), [Bash on Ubuntu on Windows](https://msdn.microsoft.com/commandline/wsl), [Cygwin](https://cygwin.com/install.html) and [Git Bash](https://git-scm.com/download) are tested and supported platforms for building the picons. Git Bash however has no ipk file as output. See below for some [benchmarks](https://github.com/picons/picons-source#benchmarks).

When using [Bash on Ubuntu on Windows](https://msdn.microsoft.com/commandline/wsl), clone to `/mnt/c`, which is your `C:\` drive on Windows. That way you can manipulate your files from within Windows, going to `%localappdata%\lxss\rootfs` inside Windows, will create problems.
[Git Bash](https://git-scm.com/download) should be used as a last resort, the output is limited and it is seriously slow.

Download the repository by using one of the following commands:
```
# Ubuntu, Cygwin and Git Bash
git clone https://github.com/picons/picons-source.git /tmp/picons-source

# Bash on Ubuntu on Windows
git clone https://github.com/picons/picons-source.git /mnt/c/picons-source
```

Next, copy the required files to the folder [build-input](https://github.com/picons/picons-source#build-input).

We will start the creation of the servicelist and the picons with the following commands:
```
# Ubuntu, Cygwin and Git Bash
cd /tmp/picons-source

# Bash on Ubuntu on Windows
cd /mnt/c/picons-source

# Ubuntu, Cygwin, Git Bash and Bash on Ubuntu on Windows
./1-build-servicelist.sh
./2-build-picons.sh
```

Take a look at the folder [build-output](https://github.com/picons/picons-source#build-output) for the results.

TIP: To automate the building process, you can also use some of the following commands:

```
./1-build-servicelist.sh snp
./1-build-servicelist.sh srp
./2-build-picons.sh snp
./2-build-picons.sh srp
./2-build-picons.sh snp-full
./2-build-picons.sh srp-full
```

# CONTRIBUTING

So you would like to contribute? Have a look [here](https://github.com/picons/picons-source/blob/master/CONTRIBUTING.md), just so you'll know the rules.

# SNP - SERVICE NAME PICONS

The idea behind SNP is that a simplified name derived from the channel name is used to lookup a channel logo. The idea and code was first implemented by OpenVIX for the Enigma2 tuners. Any developer currently using the serviceref method as a way to lookup a logo and would like to implement this alternative, can find the code used to generate the simplified name at the OpenVIX github [repository](https://github.com/OpenViX/enigma2/blob/master/lib/python/Components/Renderer/Picon.py#L88-L89).

# FOLDER OVERVIEW

## build-input

###### Enigma2 servicelist creation
Copy your `enigma2` folder, probably located in `/etc` on your box into this folder.

###### TvHeadend servicelist creation
TvHeadend users, have two options:

1. Use the server generated configuration files by copying your `tvheadend` configuration folder, probably located in `/home/hts/.hts` on your server into this folder.

2. Use the servers API and directly ask the server about all channels by creating a file called `tvheadend.serverconf`.

The first option has the advantage to work even without a running server. The advantage of the second option is that you don't have to copy files around, automatically you'll have the most accurate channel list and it's about 20% faster.

The file `tvheadend.serverconf` can contain the following values:

```sh
# hostname or ip address of tvheadend server (default: "localhost")
TVH_HOST="localhost"
# port of tvheadend API (default: 9981)
TVH_PORT="9981"
# tvheadend user name
TVH_USER=""
# tvheadend password of above user
TVH_PASS=""
```

Only the values which are different from the default values are required. For most people this will be a file with a single host name or host IP address.

```sh
TVH_HOST="my.tvheadend.server"
```

If you're running TvHeadend on the same machine, even an empty file (defaulting to `localhost`) should be sufficient.

###### VDR servicelist creation
If you're using VDR together with the Kodi addon xvdr, copy your `channels.conf` file to this folder.

###### Configuring which backgrounds to build
A file `backgrounds.conf` should be placed in this folder. If no file is found, the default file will be used.

Syntax:
```
<resolution>;<resolution-padding>;<logotype>;<background>
```

Example:
```
# My own awesome settings
100x60;86x46;dark;reflection
100x60;100x60;default;transparent
100x60;100x60;light;transparent

# My commented settings
#800x450;800x450;light;transparent
```

## build-output

This folder will contain the output from the build. Similar to the files [servicelist-enigma2-snp](https://raw.githubusercontent.com/picons/picons-source/master/resources/samples/servicelist-enigma2-snp) and [servicelist-enigma2-srp](https://raw.githubusercontent.com/picons/picons-source/master/resources/samples/servicelist-enigma2-srp). The picon binaries are also saved in this folder.

Possible output files and folders:
```
binaries-snp/
binaries-srp/
servicelist-enigma2-snp
servicelist-enigma2-srp
servicelist-tvheadend-filemode-snp
servicelist-tvheadend-filemode-srp
servicelist-tvheadend-servermode-snp
servicelist-tvheadend-servermode-srp
servicelist-vdr-snp
servicelist-vdr-srp
```

## build-source

This is where all the channel logos go and how they are linked to the serviceref or a simplified version of the name. Backgrounds and the default `backgrounds.conf` file can also be found in this directory.

## resources/tools

Some additional scripts used by the main scripts.

# BENCHMARKS

```
Bash on Ubuntu on Windows (Ubuntu 14.04.5 LTS) - Intel Core i5-6600 16GB RAM
----------------------------------------------------------------------------

Build: snp-full.100x60-86x46.light.on.black
Image Compression: pngquant
Archive Compression: xz
ImageMagick: ImageMagick 6.7.7-10 Q16
SVG Converter: rsvg 2.40.2
Output: *.ipk, *.hardlink.tar.xz, *.symlink.tar.xz

real    6m35.650s
user    6m17.609s
sys     3m26.828s


Cygwin - Intel Core i5-6600 16GB RAM
------------------------------------

Build: snp-full.100x60-86x46.light.on.black
Image Compression: pngquant
Archive Compression: xz
ImageMagick: ImageMagick 6.9.1-3 Q16 x86_64
SVG Converter: rsvg 2.40.11
Output: *.ipk, *.hardlink.tar.xz, *.symlink.tar.xz

real    15m29.627s
user    5m31.907s
sys     4m10.524s


Cygwin - Intel Core i5-6600 16GB RAM
------------------------------------

Build: snp-full.100x60-86x46.light.on.black
Image Compression: pngquant
Archive Compression: xz
ImageMagick: ImageMagick 6.9.1-3 Q16 x86_64
SVG Converter: Inkscape 0.91 r13725
Output: *.ipk, *.hardlink.tar.xz, *.symlink.tar.xz

real    26m35.348s
user    14m2.122s
sys     7m47.723s


Git Bash - Intel Core i5-6600 16GB RAM
--------------------------------------

Build: snp-full.100x60-86x46.light.on.black
Image Compression: none
Archive Compression: bzip2
ImageMagick: ImageMagick 6.9.3-7 Q16 x64
SVG Converter: Inkscape 0.91 r13725
Output: *.nolink.tar.bzip2, *.script.tar.bzip2

real    25m10.520s
user    1m47.206s
sys     4m8.019s


VirtualBox (Ubuntu 15.10, 6GB RAM) - Intel Core i5-6600 16GB RAM
----------------------------------------------------------------

Build: snp-full.100x60-86x46.light.on.black
Image Compression: pngquant
Archive Compression: xz
ImageMagick: ImageMagick 6.8.9-9 Q16 x86_64
SVG Converter: rsvg 2.40.10
Output: *.ipk, *.hardlink.tar.xz, *.symlink.tar.xz

real    4m42.611s
user    3m8.620s
sys     0m8.380s


VirtualBox (Ubuntu 15.10, 6GB RAM) - Intel Core i5-6600 16GB RAM
----------------------------------------------------------------

Build: snp-full.100x60-86x46.light.on.black
Image Compression: pngquant
Archive Compression: xz
ImageMagick: ImageMagick 6.8.9-9 Q16 x86_64
SVG Converter: Inkscape 0.91 r13725
Output: *.ipk, *.hardlink.tar.xz, *.symlink.tar.xz

real    9m11.571s
user    6m45.696s
sys     0m29.388s
```
