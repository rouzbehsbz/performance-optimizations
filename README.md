## Hardware
- Best support for AMD CPU and GPU

## BIOS
- Update to latest version
- Consider performance boost options
- Enabling memory profile
- Manual CPU clock / voltage config

## Distributions
- Fedora: Best for developers, workstations and modern hardware. newer linux kernel, firmwares and drivers

## System Update
```bash
# Update installed packages with refreshed metadata
sudo dnf upgrade --refresh

# Faster downloading
sudo dnf config-manager setopt max_parallel_downloads=10
```

## Kernel
```bash
# Check supported version
/lib64/ld-linux-x86-64.so.2 --help | grep "(supported, searched)"

# Enable cachyos kernel repo and tools
sudo dnf copr enable bieszczaders/kernel-cachyos
sudo dnf copr enable bieszczaders/kernel-cachyos-addons

# Install main kernel
sudo dnf install kernel-cachyos kernel-cachyos-devel-matched

# Install v2 for older CPUs
sudo dnf install kernel-cachyos-lts kernel-cachyos-lts-devel-matched

# List kernels
sudo grubby --info=ALL | grep -E "^kernel|^index|^title"

# Current default
sudo grubby --default-kernel
sudo grubby --default-index

# Set CachyOS entry as default
sudo grubby --set-default-index=<INDEX_NUMBER>
```

## System Services
```bash
# Install and enable ananicy and cachyos process priority rules
sudo dnf install ananicy-cpp cachyos-ananicy-rules
sudo systemctl enable --now ananicy-cpp

# Install cachyos optimization packages
sudo dnf install cachyos-settings cachyos-ksm-settings scx-scheds scx-tools

# Configure scx_loader to use the LAVD scheduler in gaming mode
sudo mkdir -p /etc/scx_loader
sudo tee /etc/scx_loader/config.toml <<'EOF'
default_sched = "scx_lavd"
default_mode = "Gaming"

[scheds.scx_lavd]
gaming_mode = ["--performance"]
EOF

# Disable IRQ balancing to allow manual affinity tuning
sudo systemctl disable --now irqbalance

# Disable Bluetooth
# Disable mobile broadband management
# Disable printing services
# Disable mDNS and local network service discovery
# Disable waiting for network connectivity during boot
# Disable geolocation services
# Disable disk health monitoring
sudo systemctl disable --now bluetooth ModemManager cups avahi-daemon NetworkManager-wait-online geoclue smartd

# Enable performance mode
sudo tuned-adm profile throughput-performance

# Check the active profile
tuned-adm active
```

## GRUB
```bash
# Disable split-lock detection
# Enable AMD P-State CPU frequency scaling
# Optimize PCIe bus performance
# Favor PCIe performance over power savings
# Disable USB autosuspend
# Disable the kernel soft-lockup watchdog
# Disable the NMI watchdog
# Disable compressed swap (zswap)
sudo grubby --update-kernel=ALL --args="split_lock_detect=off amd_pstate=active pci=pcie_bus_perf pcie_aspm.policy=performance usbcore.autosuspend=-1 nowatchdog nmi_watchdog=0 zswap.enabled=0"
```

## Storage
```bash
# Check whether periodic SSD TRIM is enabled
systemctl status fstrim.timer

# Turn on weekly TRIM
sudo systemctl enable --now fstrim.timer
```

## Monitoring & Benchmark
```bash
# Performance overlay system monitor
sudo dnf install mangohud btop

# sysbench is system benchmark
# stress-ng is stress testing
sudo dnf install sysbench stress-ng sensors
```
