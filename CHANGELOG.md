# Changelog

All notable changes to this project will be documented in this file.
Releases are named with the following scheme:

`<Yocto Project version name>.<TQ module family>.BSP.SW.<version number>`

**NOTE:** For details on changes in a release see the CHANGELOG.md file in
the sources/meta-tqmx86 git submodule. Changes described in the layer changelog
are not duplicated in this file.

[[_TOC_]]

## Next Release

## scarthgap.TQMx86.BSP.SW.0003

### Updated

- poky: updated to 72983ac391008ebceb45edc7a8f0f6d5f4fe715c
- meta-openembedded: updated to 2b26d30fc7f478f5735d514f0c1bc28f6a4148b6
- meta-intel: updated to 6fc37057c7f0293a2345e303588901889df8b937

### Changed

- The `x86` configuration template has been updated to use the `pretzel` distro
  defined by meta-tqmx86 instead of including additional packages via
  `local.conf`

### Removed

- The `x86-rt` configuration template has been dropped in favor of the new
  `pretzel-rt` distro

## scarthgap.TQMx86.BSP.SW.0002

Initial TQMx86-release based on Yocto Scarthgap.
