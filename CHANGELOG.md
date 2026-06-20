# Changelog

## Unreleased — ex_nvr fork

Renamed from `nerves_system_qemu_aarch64` to `ex_nvr_system_qemu_aarch64` (the
`ex_nvr_system_*` convention used by `evercam/ex_nvr_system_rpi4`) so ex_nvr
firmware can target a QEMU VM for experiments. Forked from upstream v0.3.5 below.

Ported the ex_nvr OS-level dependencies from `evercam/ex_nvr_system_rpi4`
(diffed against base `nerves_system_rpi4`) so ex_nvr can run here:

* Packages: `e2fsprogs` (+host), `gptfdisk`/`sgdisk`, `lvm2`, `smartmontools`,
  `kmod`, `util-linux` (binaries + mount), `libsrtp`, `liburcu`, `inih`,
  `nginx` (+http cache), `socat`, plus the shared busybox config fragment.
* Kernel: `EXT4_FS` (+ACL/security), device-mapper (`MD`, `BLK_DEV_DM`) for
  LVM, and `LIBCRC32C`.
* Build: pinned `nerves_system_br` to `1.33.0` to match the evercam ex_nvr
  fleet (rpi4/rpi5) — all systems in one firmware project must share it (down
  from the upstream qemu system's 1.33.9; both are OTP 28, and `little_loader`
  is provided by this system, not by nerves_system_br).

Skipped the RPi-only bits (mesa/v3d, rpi-firmware, pigpio, USB-gadget RNDIS,
HDMI `libdisplay-info`) and the PREEMPT_RT→PREEMPT kernel tweak.

Resilience / fault-injection testing (from upstream `nerves_system_qemu_aarch64`):

* `mix nerves.gen.qemu` gained options for driving the VM from the outside:
  `--qmp`, `--serial`, `--data-disk`, `--ram`, `--smp` and `--accel`. A
  `virtio-balloon` device is now always included and the primary drive uses a
  stable id so QMP `block_set_io_throttle` can address it at runtime. Default
  output is unchanged.
* Added `examples/resilience_testing`, an example firmware with a QEMU-driven
  fault-injection test suite (disk latency/errors/corruption, sudden power
  loss, network latency). The guest is controlled over an Erlang `:peer` RPC
  channel that rides the serial console via `peer_bridge`. See
  `examples/resilience_testing/README.md`.

## v0.3.5

This is a security and bug fix release.

* Package updates
  * [nerves_system_br 1.33.9](https://github.com/nerves-project/nerves_system_br/releases/tag/v1.33.9)
    * [Erlang/OTP 28.5.0.1](https://erlang.org/download/OTP-28.5.0.1.README.md)

## v0.3.4

This is a security and bug fix release.

* Changes
  * Use ARMv8 floating point since available
  * Add Nerves backup site and other missing options

* Package updates
  * [nerves_system_br 1.33.7](https://github.com/nerves-project/nerves_system_br/releases/tag/v1.33.7)
    * [Erlang/OTP 28.5](https://erlang.org/download/OTP-28.5.README.md)
    * [fwup 1.16.0](https://github.com/fwup-home/fwup/releases/tag/v1.16.0)
## v0.3.3

This is a security update.

* Package updates
  * [nerves_system_br 1.33.5](https://github.com/nerves-project/nerves_system_br/releases/tag/v1.33.5)
    * [Erlang/OTP 28.4.2](https://erlang.org/download/OTP-28.4.2.README.md)
    * [Buildroot 2025.11.3](https://lore.kernel.org/buildroot/124c21a6-5810-495e-8b85-f3db41afa1a9@rnout.be/T/)
## v0.3.2

This is a security update.

* Package updates
  * [nerves_system_br 1.33.4](https://github.com/nerves-project/nerves_system_br/releases/tag/v1.33.4)
    * [Erlang/OTP 28.4.1](https://erlang.org/download/OTP-28.4.1.README.md)
    * [Buildroot 2025.11.2](https://lore.kernel.org/buildroot/de9c890a-760a-4e6d-86b8-f8e5000a07ff@rnout.be/T/)

## v0.3.1

This is a security/bug fix release.

* Changes
  * Enabled multipath TCP support in the Linux kernel

* Package updates
  * [nerves_system_br 1.33.2](https://github.com/nerves-project/nerves_system_br/releases/tag/v1.33.2)
    * [Erlang/OTP 28.3.1](https://erlang.org/download/OTP-28.3.1.README.md)
    * [Buildroot 2025.11.1](https://lore.kernel.org/buildroot/f6496994-b279-46f4-b554-7dbe2df92782@rnout.be/T/)

## v0.3.0

This is a major Buildroot update. It should be a seamless update for most users.

* Fixes
  * Fix firmware path computation in `mix nerves.gen.qemu`
  * Support ssh via port 10022 in shown commandline

* Updated dependencies
  * [nerves_system_br 1.33.0](https://github.com/nerves-project/nerves_system_br/releases/tag/v1.33.0)
    * [Buildroot 2025.11](https://lore.kernel.org/buildroot/87bjk439tj.fsf@dell.be.48ers.dk/T/)
    * [Erlang/OTP 28.3](https://erlang.org/download/OTP-28.3.README.md)
    * [fwup 1.15.0](https://github.com/fwup-home/fwup/releases/tag/v1.15.0)
    * [erlinit 1.15.1](https://github.com/nerves-project/erlinit/releases/tag/v1.15.1)
    * [nerves_heart 2.5.0](https://github.com/nerves-project/nerves_heart/releases/tag/v2.5.0)
    * [boardid 1.15.0](https://github.com/nerves-project/boardid/releases/tag/v1.15.0)

## v0.2.0

This is a major Erlang and Buildroot update. This updates from Erlang/OTP 27 to
Erlang/OTP 28.

* Changes
  * Do not require firmware path for nerves.gen.qemu
  * Switch fwup upgrade validation to use upgrade_available
  * Remove unneeded call to `rngd` and the `rng-tools` package. This was
    formerly needed to provide entropy to Linux during initialization.

* Package updates
  * [nerves_system_br v1.32.3 release notes](https://github.com/nerves-project/nerves_system_br/releases/tag/v1.32.3)

* Updated dependencies
  * [Erlang/OTP 28.1.1](https://erlang.org/download/OTP-28.1.1.README.md)
  * [Buildroot 2025.05.2](https://lore.kernel.org/buildroot/7bed9b2e-a9d3-476b-84d6-61134e2f726f@rnout.be/T/)

## v0.1.1

This is an important security/bug fix that addresses Erlang CVEs for the ssh
module (see Erlang release notes).

* Changes
  * Build `libnl` to avoid compile error with `vintage_net_wifi`. Note that it's
    not possible to use WiFi, but `mix nerves.new` includes it by default for everyone.

* Package updates
  * [nerves_system_br v1.31.7](https://github.com/nerves-project/nerves_system_br/releases/tag/v1.31.7). Also
    see [nerves_system_br v1.31.6](https://github.com/nerves-project/nerves_system_br/releases/tag/v1.31.6)

* Important derived package updates
  * [Erlang/OTP 27.3.4.3](https://erlang.org/download/OTP-27.3.4.3.README.md)
  * [Buildroot 2025.02.6](https://lore.kernel.org/buildroot/b051d400-debc-4269-975a-b2992eed8d61@rnout.be/T/)

## v0.1.0

Initial release
