# regolith-st

This is the terminal package for Regolith, which is an open source project called [Simple Terminal from the suckless.org project](https://st.suckless.org/).  This repo does not contain the actual code for the terminal application, only the Debian package metadata necessary to bundle it as a package and any customizations Regolith has added, expressed as patches against the upstream source.

# Known issues

## `neovim` font colors producing invisible characters in `st`.

See [this issue](https://github.com/regolith-linux/regolith-st/issues/1) for description and resolution.

# How to build this package

This guide assumes you wish to make changes to a Regolith package and host those changes in a new version.  It also assumes you'll be building from an Ubuntu-based system like Regolith and you already have setup the Debian package tools.  See [this concise guide](https://wiki.debian.org/BuildingTutorial) to install the tools and get a familiar with the workflow.

## Check out the package

This step will pull the Regolith package metadata down from GitHub:

```
mkdir workspace

git clone -b debian https://github.com/regolith-linux/regolith-st

```

## Download and prepare the upstream source

This step will download the original package source code and change the name of the tarball to make Debian's build scripts happy.

```
wget https://dl.suckless.org/st/st-0.8.2.tar.gz

mv st-0.8.2.tar.gz regolith-st_0.8.2.orig.tar.gz
```

## Build the package

Now we will build the package locally and generate the Debian package metadata required for hosting in a Private Package Archive (PPA).

```
cd regolith-st

debuild -S -sa
```

There should now be a number of files generated in the parent directory, such as `regolith-st_0.8.2-1ubuntu18_source.changes`.

## Upload the package to your PPA

<sub>You may wish to change the package version in the `debian\changelog` file to avoid conflicts, but that is optional and up to you.</sub>

```
dput ppa://<username>/<ppa name> ../regolith-st_0.8.2-1ubuntu18_source.changes
```

## Verification

After uploading the package to your PPA, you should get an email from Launchpad.net regarding if the package was accepted or rejected.  Then, some time later the package should have been build and available for installation.  Assuming you have already added your PPA via `add-apt-repository`, `sudo apt update` and `sudo apt upgrade` should cause the new package to be installed on your local system.