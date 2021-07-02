# Maintainer: Jan Alexander Steffens (heftig) <heftig@archlinux.org>

pkgbase=linux-amd-s0ix
pkgver=5.12.14.arch1
#pkgver=5.12.14.notarch1
#_tagver=5.12.14.arch1
pkgrel=1
pkgdesc='Linux'
_srctag=v${pkgver%.*}-${pkgver##*.}
#_srctag=v${_tagver%.*}-${_tagver##*.}
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
  #   url = https://git.archlinux.org/linux.git
  "$_srcname::git+https://github.com/archlinux/linux.git?signed#tag=$_srctag"
  config         # the main kernel config file

  # early 5.13 ACPI code for power saving
  #"5.13-acpi-refine-turning-off-unused-power-resources.patch"

  # s0ix patches for Renoir/Cezanne landing in 5.14; plus a PCI quirk needed for many machines
  "backport-from-5.14-s0ix-enablement-no-d3hot.diff"
  "PCI-quirks-Quirk-PCI-d3hot-delay-for-AMD-xhci.patch"
  # diagnostic patches to allow debugging time spent in s0ix states
  "v5-platform-x86-amd-pmc-s0ix+smu-counters.diff"
  # ROG enablement patches
  "0001-asus-wmi-Add-panel-overdrive-functionality.patch"
  "0002-asus-wmi-Add-dgpu-disable-method.patch"
  "0003-asus-wmi-Add-egpu-enable-method.patch"
  #"0004-HID-asus-Filter-keyboard-EC-for-old-ROG-keyboard.patch"
  #"0005-HID-asus-filter-G713-G733-key-event-to-prevent-shutd.patch"
  "0006-HID-asus-Remove-check-for-same-LED-brightness-on-set.patch"
  "0007-ALSA-hda-realtek-Fix-speakers-not-working-on-Asus-Fl.patch"
  "0008-ACPI-video-use-native-backlight-for-GA401-GA502-GA50.patch"
  "0009-Revert-platform-x86-asus-nb-wmi-Drop-duplicate-DMI-q.patch"
)
validpgpkeys=(
  'ABAF11C65A2970B130ABE3C479BE3E4300411886'  # Linus Torvalds
  '647F28654894E3BD457199BE38DBBDC86092693E'  # Greg Kroah-Hartman
  'A2FF3A36AAA56654109064AB19802F8B0D70FC30'  # Jan Alexander Steffens (heftig)
)
sha256sums=('SKIP'
            'ffe1c14930227a0a8e7b19ccf1243a8f119df75f51849edce470b6b96167e2d7'
            'e4cbedbcf939961af425135bb208266c726178c4017309719341f8c37f65c273'
            'dab4db308ede1aa35166f31671572eeccf0e7637b3218ce3ae519c2705934f79'
            'b108959c4a53d771eb2d860a7d52b4a6701e0af9405bef325905c0e273b4d4fe'
            '09cf9fa947e58aacf25ff5c36854b82d97ad8bda166a7e00d0f3f4df7f60a695'
            '7a685e2e2889af744618a95ef49593463cd7e12ae323f964476ee9564c208b77'
            '663b664f4a138ccca6c4edcefde6a045b79a629d3b721bfa7b9cc115f704456e'
            '034743a640c26deca0a8276fa98634e7eac1328d50798a3454c4662cff97ccc9'
            '32bbcde83406810f41c9ed61206a7596eb43707a912ec9d870fd94f160d247c1'
            'ee5fdeacb2d8059bc119d6990c0bd68c3c4f7f5b29c9b4a8b2140e9465dd43d5'
            'd46d3562b95c4f679386e7f3209babd335c7a2b599a1196bd8c50bccedd644be')

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
    [[ $src =~ .*(patch|diff)$ ]] || continue
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
