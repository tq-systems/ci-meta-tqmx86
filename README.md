# TQ-Systems x86 modules Yocto Project build setup

This repo contains setup, configuration and dependencies use to build and test
the meta-tq hardware support layer. All Yocto Project / Open Embedded layers
the build depends on are git submodules

Clone this repo using:
```
git clone --branch=<branch-name> --recurse-submodules <url>
```

## License information

This repo contains shell scripts released under the GPLv2, see the file
[COPYING](COPYING).

## Supported hardware

See README.md of *meta-tqmx86* for a list of supported modules.

## Quick Start Guide

### Setting up an initial build space

To set up an initial build space, clone this repo using:
```
git clone --branch=<branch-name> --recurse-submodules <url>
```

Then change to checked out dir and run:
```
. ./setup-environment <builddir> <config>
```
Here, `<builddir>` is a directory to be created as a workspace for your build,
and `<config>` is the name of the configuration template to use.
*ci-meta-tqmx86* only supports a single template `x86`.

You can override defaults by setting certain environment variables before
sourcing the script:

* `export MACHINE=<machine>` (default is `intel-corei7-64-tqmx86`, which
  supports all TQ-Systems x86 modules)
* `export DISTRO=<distro>` (tested is `poky`; by default systemd and Wayland are
  selected via `DISTRO_FEATURES`)

`./setup-environment` uses the requested configuration in
`sources/template/conf/bblayers.conf.<config>` as initial template for your
bblayer.conf

Additionally some config variables are injected via `auto.conf.normal` from
`sources/template/conf/`.

In case you have a `~/.oe` or `~/.yocto` dir a site.conf file will be symlinked
to the conf dir of the buildir to allow machine specific overrrides. For
instance to use a shared download directory, you can provide `$DL_DIR` via
`~/.yocto/site.conf`.

Internally, the *oe-init-build-env* script from the OpenEmbedded-core / Poky
meta layer will be sourced to get the bitbake environment.

After this step everything is set up to build an image using bitbake.

### Return to an existing build space

To return to an existing buildspace go to the checked out dir and
```
. ./setup-environment <builddir>
```

### Reproducible build environment

Development and automated builds are supported by the scripts under CI and
configuration under ./sources/templates, notably

- sample bblayer.conf files
- sample auto.conf files and inclusion fragments (see Yocto Project doc for
  local.conf and auto.conf

### Build all supported machines

To build all supported machines in one of the configs, one can
use the CI helper script:
```
ci/build-all <builddir> <config>
```

Depending on the configuration, following images will be built:

* x86: core-image-base

### Clean build

To force a clean build of all supported machines and generate archives, do
```
ci/build-all <builddir> <config> ci
```

### Building package premirror

To help to create a package premirror (to support offline builds),
one can use the CI helper script:
```
ci/build-all <builddir> <config> mirror
```

The following configuration must added to `site.conf` or `local.conf`:
```
SOURCE_MIRROR_URL ?= "file://<full path>/"
INHERIT += "own-mirrors"

PREMIRRORS_prepend = "\
        git://.*/.* file://full path>/ \n \
        ftp://.*/.* file://full path>/ \n \
        http://.*/.* file://full path>/ \n \
        https://.*/.* file://full path>/ \n \
        "
```

To fill the mirror, the script

- allows to create also tarballs from SCM using `BB_GENERATE_MIRROR_TARBALLS=1`
- forces downloads for uninative packages by `INHERIT_remove = "uninative"`
- copies all archives from `DL_DIR` to the mirror

This way the mirror can be used to do offline builds without downloading
anything with `BB_FETCH_PREMIRRORONLY=1`

## Security

This project is focused on board bringup and demonstration for TQ-Systems
starter kits. Since embedded projects have different goals, the Yocto Project
brings lots of features to modify system configuration and setup. This project
is not a turn key distribution but a starting point for your own development.

For demonstration purpose and ease of development, the root filesystem created
with this setup has no root password set by default.
