# thinkpad-p50-fingerprint-rhel
How to enable fingerprint reader in Lenovo Thinkpad P50 under RHEL 7.7

This instruction is based on the custom Linux driver published here: https://github.com/3v1n0/libfprint. Their installation instruction for Fedora and other distributions is not updated after they switched build system from `GNU autotools` to `Meson`.

# Prerequisites
Verify you have a fingerprint sensor with ID `138a:0090` (Thinkpad P50 and possibly other models from around 2016 as well):
```
# lsusb
...
Bus 001 Device 004: ID 138a:0090 Validity Sensors, Inc. VFS7500 Touch Fingerprint Sensor
...
```
# Procedure
```
sudo yum install git python3 libfprint fprintd fprintd-pam gcc-c++.x86_64 cmake libusbx-devel nss nss-devel libXv-devel gtk3-devel gtk-doc ninja-build
git clone https://github.com/3v1n0/libfprint
cd libfprint/
meson builddir
cd builddir/
ninja-build
ninja-build install
sudo mv /usr/lib64/libfprint.so.0.0.0 /usr/lib64/libfprint.so.0.0.0.orig
sudo cp /usr/local/lib64/libfprint.so.0.0.0 /usr/lib64/libfprint.so.0.0.0
```
At this stage you should be able to enroll your fingerprint with `fprintd-enroll`. If successful, you can enable fingerprint login in Ubuntu `Settings`: Details -> Users -> Fingerprint Login.
