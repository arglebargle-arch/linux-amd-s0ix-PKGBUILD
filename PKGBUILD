# Maintainer: Jan Alexander Steffens (heftig) <heftig@archlinux.org>

pkgbase=linux-amd-s0ix
pkgver=5.12.13.notarch1
_tagver=5.12.13.arch1
pkgrel=3
pkgdesc='Linux'
#_srctag=v${pkgver%.*}-${pkgver##*.}
_srctag=v${_tagver%.*}-${_tagver##*.}
url="https://github.com/archlinux/linux/commits/$_srctag"
arch=(x86_64)
license=(GPL2)
makedepends=(
  bc kmod libelf pahole cpio perl tar xz
  xmlto
  # python-sphinx python-sphinx_rtd_theme graphviz imagemagick
  git
)
options=('!strip')
_srcname=archlinux-linux
source=(
  # NOTE: Be sure to change to the new repo url in your arch-linux/config *before* running makepkg
  #
  #   url = https://git.archlinux.org/linux.git

  "$_srcname::git+https://github.com/archlinux/linux.git?signed#tag=$_srctag"
  config         # the main kernel config file

  # revert broken 5.12.13.arch1 commits back to 5.12.13 upstream-ish (include ZEN disallow unprivileged CLONE_NEWUSER)
  "revert-5.12.13.arch1-to-upstream-5.12.13ish.patch"

  # early 5.13 ACPI code for power saving
  "5.13-acpi-1of2-turn-off-unused.patch"::"https://git.kernel.org/pub/scm/linux/kernel/git/rafael/linux-pm.git/patch/?id=4b9ee772eaa82188b0eb8e05bdd1707c2a992004"
  # the second patch in this sequence (2of2) was rejected upstream as it causes problems for some Intel machines
  "5.13-acpi-refine-turning-off-unused-power-resources.patch"

  # these commits crash cezanne immediately on suspend; they're already reverted upstream
  "revert-4cbbe34807938e6e494e535a68d5ff64edac3f20.patch"
  "revert-1c0b0efd148d5b24c4932ddb3fa03c8edd6097b3.patch"

  # s0ix patches for Renoir/Cezanne landing in 5.14; plus a PCI quirk needed for many machines
  "5.14-ACPI-processor-idle-Fix-up-C-state-latency-if-not-ordered.patch"
  "PCI-quirks-Quirk-PCI-d3hot-delay-for-AMD-xhci.patch"
  "5.14-nvme-pci-look-for-StorageD3Enable-on-companion-ACPI-device.patch"
  "5.14-ACPI-Check-StorageD3Enable_DSD-property-in-AHCI-mode.patch"
  "5.14-ACPI-Add-quirks-for-AMD-Renoir+Lucienne-CPUs-to-force-the-D3-hint.patch"
  "5.14-ACPI-PM-s2idle-Add-missing-LPS0-functions-for-AMD.patch"
  "5.14-1of5-ACPI-PM-s2idle-Use-correct-revision-id.patch"
  "5.14-2of5-ACPI-PM-s2idle-Refactor-common-code.patch"
  "5.14-3of5-ACPI-PM-s2idle-Add-support-for-multiple-func-mask.patch"
  "5.14-4of5-ACPI-PM-s2idle-Add-support-for-new-Microsoft-UUID.patch"
  "5.14-5of5-ACPI-PM-s2idle-Adjust-behavior-for-field-problems-on-AMD-systems.patch"

  # diagnostic patches to allow debugging time spent in s0ix states
  "platform-x86-amd-pmc-Fix-command-completion-code.patch"
  "platform-x86-amd-pmc-Fix-SMU-firmware-reporting-mechanism.patch"
  "platform-x86-amd-pmc-Add-support-for-logging-SMU-metrics.patch"
  "platform-x86-amd-pmc-Add-support-for-s0ix-counters.patch"
  "platform-x86-amd-pmc-Add-support-for-ACPI-ID-AMDI0006.patch"
  "platform-x86-amd-pmc-Add-new-acpi-for-future-PMC.patch"
)
validpgpkeys=(
  'ABAF11C65A2970B130ABE3C479BE3E4300411886'  # Linus Torvalds
  '647F28654894E3BD457199BE38DBBDC86092693E'  # Greg Kroah-Hartman
  'A2FF3A36AAA56654109064AB19802F8B0D70FC30'  # Jan Alexander Steffens (heftig)
)
sha256sums=('SKIP'
            'ffe1c14930227a0a8e7b19ccf1243a8f119df75f51849edce470b6b96167e2d7'
            '05f47255831028b9e3a49e335323169d0156201d5e9b2bf8e09093440ab9e56e'
            '5af4796400245fec2e84d6e3f847b8896600558aa85f5e9c4706dd50994a9802'
            'f3b2dbdfd01d728ca1f4bc130eb227edd1985c2b2f7470c8a95aa75c6a85da10'
            'e03b26bbfd6d7a3fffa290346f96e6f4376e09ac3a76bc658eaab0cd8b486ddd'
            '3cff17ff6953eef7c17d066d56e510713f2692efac90c61b748d9d38b318f5c8'
            'b4a563ef30f86b9af0932c00bb3422b95eedbda1ff40a1a725c22a0ae9ab7084'
            'dab4db308ede1aa35166f31671572eeccf0e7637b3218ce3ae519c2705934f79'
            '9e83c46bed9059ba78df6c17a2f7c80a1cdb6efbdf64ec643f68573ede891b95'
            '6c5538dc21a139a4475af6c1acc5d2761923173992568f7c159db971ff3167cd'
            '84119c2d2beb6d7dc56389f2d1be8052b4fd23022e15edd86ee59130adcd9ab7'
            '478e908f89ae413c650116681710aed3e974384a2ed5e97be3755189688e5415'
            '1c58e4fd62cb7034e4fe834b55ffd8e183926571d4056b150bab5725f0ac5e29'
            '50f6e6a3371eaedd3b649a25c5044e6359853c2e3836a6af683a906abb973fba'
            '23ada5c29c415c0bb8d14cff213c697c649b438d7427a67a15b0b3f65c66aa6f'
            '9ea5d38eea3809e85c6f3165f4b410ee53f0fdb813cbbf229e18a87e79c13ad5'
            'd6113df716cb81f78abc58405648d90f8497e29d79f5fd403eda36af867b50f3'
            'bc783b22ab5ab75dc28ae10519a9d6da23d80ee291812115555945acd280edc5'
            'dce87ca35886d075554fe6d8831075237d80526e078431165d2ec0d1a9630c7b'
            'ad9f485bb262bb1156da57698ccab5a6b8d8ca34b6ae8a185dcd014a34c69557'
            '3e8c51aff84b6f12e6bc61057982befd82415626fe379e83271ddeb1a9628734'
            'bd975ab32d6490a4231d6ce4fab0343698b28407799bdaec133671e9fd778eb5'
            'ae66bbed96b5946b5a20d902bc0282c7dd172650812114b24429f40d5ba225bb')

export KBUILD_BUILD_HOST=archlinux
export KBUILD_BUILD_USER=$pkgbase
export KBUILD_BUILD_TIMESTAMP="$(date -Ru${SOURCE_DATE_EPOCH:+d @$SOURCE_DATE_EPOCH})"

prepare() {
  cd $_srcname

  echo "Setting version..."
  scripts/setlocalversion --save-scmversion
  echo "-$pkgrel" > localversion.99-pkgrel
  echo "${pkgbase#linux}" > localversion.20-pkgname

  local src
  for src in "${source[@]}"; do
    src="${src%%::*}"
    src="${src##*/}"
    [[ $src = *.patch ]] || continue
    echo "Applying patch $src..."
    patch -Np1 < "../$src"
  done

  echo "Setting config..."
  cp ../config .config
  make olddefconfig

  make -s kernelrelease > version
  echo "Prepared $pkgbase version $(<version)"
}

build() {
  cd $_srcname
  make all
  #make htmldocs
}

_package() {
  pkgdesc="The $pkgdesc kernel and modules"
  depends=(coreutils kmod initramfs)
  optdepends=('crda: to set the correct wireless channels of your country'
              'linux-firmware: firmware images needed for some devices')
  provides=(VIRTUALBOX-GUEST-MODULES WIREGUARD-MODULE)
  replaces=(virtualbox-guest-modules-arch wireguard-arch)

  cd $_srcname
  local kernver="$(<version)"
  local modulesdir="$pkgdir/usr/lib/modules/$kernver"

  echo "Installing boot image..."
  # systemd expects to find the kernel here to allow hibernation
  # https://github.com/systemd/systemd/commit/edda44605f06a41fb86b7ab8128dcf99161d2344
  install -Dm644 "$(make -s image_name)" "$modulesdir/vmlinuz"

  # Used by mkinitcpio to name the kernel
  echo "$pkgbase" | install -Dm644 /dev/stdin "$modulesdir/pkgbase"

  echo "Installing modules..."
  make INSTALL_MOD_PATH="$pkgdir/usr" INSTALL_MOD_STRIP=1 modules_install

  # remove build and source links
  rm "$modulesdir"/{source,build}
}

_package-headers() {
  pkgdesc="Headers and scripts for building modules for the $pkgdesc kernel"
  depends=(pahole)

  cd $_srcname
  local builddir="$pkgdir/usr/lib/modules/$(<version)/build"

  echo "Installing build files..."
  install -Dt "$builddir" -m644 .config Makefile Module.symvers System.map \
    localversion.* version vmlinux
  install -Dt "$builddir/kernel" -m644 kernel/Makefile
  install -Dt "$builddir/arch/x86" -m644 arch/x86/Makefile
  cp -t "$builddir" -a scripts

  # add objtool for external module building and enabled VALIDATION_STACK option
  install -Dt "$builddir/tools/objtool" tools/objtool/objtool

  # add xfs and shmem for aufs building
  mkdir -p "$builddir"/{fs/xfs,mm}

  echo "Installing headers..."
  cp -t "$builddir" -a include
  cp -t "$builddir/arch/x86" -a arch/x86/include
  install -Dt "$builddir/arch/x86/kernel" -m644 arch/x86/kernel/asm-offsets.s

  install -Dt "$builddir/drivers/md" -m644 drivers/md/*.h
  install -Dt "$builddir/net/mac80211" -m644 net/mac80211/*.h

  # http://bugs.archlinux.org/task/13146
  install -Dt "$builddir/drivers/media/i2c" -m644 drivers/media/i2c/msp3400-driver.h

  # http://bugs.archlinux.org/task/20402
  install -Dt "$builddir/drivers/media/usb/dvb-usb" -m644 drivers/media/usb/dvb-usb/*.h
  install -Dt "$builddir/drivers/media/dvb-frontends" -m644 drivers/media/dvb-frontends/*.h
  install -Dt "$builddir/drivers/media/tuners" -m644 drivers/media/tuners/*.h

  echo "Installing KConfig files..."
  find . -name 'Kconfig*' -exec install -Dm644 {} "$builddir/{}" \;

  echo "Removing unneeded architectures..."
  local arch
  for arch in "$builddir"/arch/*/; do
    [[ $arch = */x86/ ]] && continue
    echo "Removing $(basename "$arch")"
    rm -r "$arch"
  done

  echo "Removing documentation..."
  rm -r "$builddir/Documentation"

  echo "Removing broken symlinks..."
  find -L "$builddir" -type l -printf 'Removing %P\n' -delete

  echo "Removing loose objects..."
  find "$builddir" -type f -name '*.o' -printf 'Removing %P\n' -delete

  echo "Stripping build tools..."
  local file
  while read -rd '' file; do
    case "$(file -bi "$file")" in
      application/x-sharedlib\;*)      # Libraries (.so)
        strip -v $STRIP_SHARED "$file" ;;
      application/x-archive\;*)        # Libraries (.a)
        strip -v $STRIP_STATIC "$file" ;;
      application/x-executable\;*)     # Binaries
        strip -v $STRIP_BINARIES "$file" ;;
      application/x-pie-executable\;*) # Relocatable binaries
        strip -v $STRIP_SHARED "$file" ;;
    esac
  done < <(find "$builddir" -type f -perm -u+x ! -name vmlinux -print0)

  echo "Stripping vmlinux..."
  strip -v $STRIP_STATIC "$builddir/vmlinux"

  echo "Adding symlink..."
  mkdir -p "$pkgdir/usr/src"
  ln -sr "$builddir" "$pkgdir/usr/src/$pkgbase"
}

#_package-docs() {
#  pkgdesc="Documentation for the $pkgdesc kernel"
#
#  cd $_srcname
#  local builddir="$pkgdir/usr/lib/modules/$(<version)/build"
#
#  echo "Installing documentation..."
#  local src dst
#  while read -rd '' src; do
#    dst="${src#Documentation/}"
#    dst="$builddir/Documentation/${dst#output/}"
#    install -Dm644 "$src" "$dst"
#  done < <(find Documentation -name '.*' -prune -o ! -type d -print0)
#
#  echo "Adding symlink..."
#  mkdir -p "$pkgdir/usr/share/doc"
#  ln -sr "$builddir/Documentation" "$pkgdir/usr/share/doc/$pkgbase"
#}

#pkgname=("$pkgbase" "$pkgbase-headers" "$pkgbase-docs")
pkgname=("$pkgbase" "$pkgbase-headers")
for _p in "${pkgname[@]}"; do
  eval "package_$_p() {
    $(declare -f "_package${_p#$pkgbase}")
    _package${_p#$pkgbase}
  }"
done

# vim:set ts=8 sts=2 sw=2 et:
