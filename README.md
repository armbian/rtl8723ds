<h2 align="center">
  <a href=#><img src="https://raw.githubusercontent.com/armbian/.github/master/profile/logosmall.png" alt="Armbian logo"></a>
  <br><br>
</h2>

# rtl8723ds

## Purpose of This Repository

Out-of-tree Linux kernel driver for the Realtek **RTL8723DS** SDIO Wi-Fi + Bluetooth combo chip, packaged as a DKMS-friendly source tree for use with Armbian and modern Linux kernels.

This tree is derived from Realtek's vendor release `v5.1.1.5_20523_20161209_BTCOEX20161208-1212` and has been modified to build cleanly for kernels through v5.9.

## Features

Enabled by default in the top-level `Makefile`:

- Target IC: **RTL8723D** (`CONFIG_RTL8723D = y`)
- Host interface: **SDIO** (`CONFIG_SDIO_HCI = y`)
- Bluetooth coexistence (`CONFIG_BT_COEXIST = y`)
- Power saving (`CONFIG_POWER_SAVING = y`)
- Traffic protection (`CONFIG_TRAFFIC_PROTECT = y`)
- EFUSE config file support (`CONFIG_EFUSE_CONFIG_FILE = y`)
- Load PHY parameters from file (`CONFIG_LOAD_PHY_PARA_FROM_FILE = y`)
- Per-rate TX power (`CONFIG_TXPWR_BY_RATE_EN = y`)
- Bridge extension (`CONFIG_BR_EXT = y`)
- NAPI + GRO (`CONFIG_RTW_NAPI = y`, `CONFIG_RTW_GRO = y`)
- Keep SDIO power during suspend (`CONFIG_RTW_SDIO_PM_KEEP_POWER = y`)

## Requirements

- A Linux system with kernel headers matching the running kernel
- `make` and a working C toolchain (`gcc`)
- Root privileges to install and load the resulting kernel module

## Building and Installing

```bash
git clone https://github.com/armbian/rtl8723ds.git
cd rtl8723ds
make
sudo make install
sudo modprobe -v 8723ds
```

### Non-Concurrent Mode

If you do not want two virtual interfaces (station and access point) at once, disable concurrent mode before building:

1. Clone the repository and open the `Makefile`:

   ```bash
   git clone https://github.com/armbian/rtl8723ds.git
   cd rtl8723ds
   nano Makefile
   ```

2. Find the line containing `ccflags-y += -DCONFIG_CONCURRENT_MODE` and prepend a `#` to comment it out.

3. Build and install as usual:

   ```bash
   make
   sudo make install
   sudo modprobe -v 8723ds
   ```

## Repository Layout

```
core/         Core driver logic (MLME, cmd, xmit/recv, security, P2P, ...)
hal/          Hardware Abstraction Layer
  btc/          Bluetooth coexistence per-chip modules
  efuse/        eFuse mask tables
  hal_hci/      Host controller interface glue (SDIO)
  led/          LED handling (SDIO)
  phydm/        PHY dynamic management (DIG, antenna div., TX beamforming, ...)
  rtl8723d/     RTL8723D-specific HAL and SDIO code
include/      Public and internal driver headers
os_dep/       OS-dependent code
  linux/        Linux-specific glue (cfg80211, ioctl, proc, sdio_intf, ...)
platform/     Platform-specific SDIO/USB helpers
Makefile      Kbuild-compatible top-level build file
Kconfig       Kernel Kconfig entries
clean         Cleanup helper
ifcfg-wlan0   Example network interface config
runwpa        Example wpa_supplicant launch helper
wlan0dhcp     Example DHCP helper
```

## Built With

- **C** — the driver sources (`core/`, `hal/`, `os_dep/`, `platform/`, `include/`)
- **Kbuild Makefile** — top-level `Makefile` integrating with the Linux kernel build system
- **Kconfig** — kernel configuration integration
- **Shell** — helper scripts (`clean`, `runwpa`, `wlan0dhcp`)

## Continuous Integration

Build status and CI history for this repository are available on the Armbian CI dashboard:

<https://actions.armbian.com/?repo=rtl8723ds>

## License

Distributed under the terms in the [`COPYING`](COPYING) file (GPL, inherited from the upstream Realtek driver).

## Related Links

- Armbian project: <https://www.armbian.com>
- Armbian documentation: <https://docs.armbian.com>
