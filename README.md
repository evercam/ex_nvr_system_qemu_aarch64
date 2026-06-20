# ex_nvr_system_qemu_aarch64

The QEMU ARM64/aarch64 Nerves system for running **ex_nvr** firmware on a virtual machine instead of physical hardware — handy for experiments such as the watchdog.

It is a rename of the upstream [`nerves_system_qemu_aarch64`](https://github.com/nerves-project/nerves_system_qemu_aarch64) to the `ex_nvr_system_*` convention (cf. [`evercam/ex_nvr_system_rpi4`](https://github.com/evercam/ex_nvr_system_rpi4)) so the ex_nvr Nerves firmware can target it.

This is the Nerves base system for the [Qemu ARM system emulator](https://www.qemu.org/docs/master/system/target-arm.html). It is focused on ARM64 and intended to be used with [Little Loader](https://github.com/fhunleth/little_loader).

| Feature        | Description                                                 |
| -------------- | ----------------------------------------------------------- |
| CPU            | Cortex-A53 or host CPU (64-bit)                             |
| Storage        | virt                                                        |
| Linux kernel   | 6.12                                                        |
| Ethernet       | Yes                                                         |
| RTC            | Yes                                                         |
| HW Watchdog    | Software watchdog                                           |

## Prerequisites

- [fwup](https://github.com/fwup-home/fwup)

## Usage

When using this for your Nerves system the details of the qemu command-line args can vary a bit based on your environment. To help with this we provide a mix task to generate a command:

```
mix nerves.gen.qemu
```

If it can determine that you likely can support KVM acceleration or you are on Apple Silicon and can use HVF acceleration it will provide those. Otherwise it falls back to full emulation.

The guest Nerves device can be accessed via the console or by SSH via port 10022.
