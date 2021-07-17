Arch linux distro kernel with Renoir/Cezanne s0ix suspend enablement patches


I'll try to keep this package up to date until 5.14 is generally available or as new bugfixes become available.


More information about the s0ix enablement effort can be found here:
  - https://gitlab.freedesktop.org/drm/amd/-/issues/1230 (huge thread; expect *long* load times)
  - https://gitlab.freedesktop.org/drm/amd/-/issues/1629


This kernel presently includes all s0ix related patches through 2021-07-14, plus the AMD XHCI d3hot quirk; there
are some basic ASUS ROG hardware enablement patches added as well, feel free to comment those out of the PKGBUILD
if you don't need them.
