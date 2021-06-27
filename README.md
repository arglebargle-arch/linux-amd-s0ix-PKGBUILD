Arch linux distro kernel with Renoir/Cezanne s0ix suspend enablement patches


I'll try to keep this package up to date until 5.14 is generally available or as new bugfixes become available.


More information about the s0ix enablement effort can be found here: https://gitlab.freedesktop.org/drm/amd/-/issues/1230#note_963056


This kernel presently includes:
  * 10 commits accepted for 5.14 for s0ix enablement
  * 1 PCIe XHCI quirk commit
  * 6 commits providing s0ix power state statistics
  * 2 reverts removing problematic drm/amdgpu patches


Note: kernel version 5.12.13.arch1 shipped by Arch is a bit broken; the 1st MB memory reservation changes they've pulled
in from mainline cause suspend/resume to crash on AMD GPUs. We also revert those as needed.
