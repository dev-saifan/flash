# Flash ⚡

A blazing-fast, lightweight block-device flashing utility written in Python and compiled into a native single-file binary using Nuitka. Designed as a high-speed, custom alternative to traditional tools like `dd` with real-time progress and speed tracking.

## Features
- **Raw Block Device Support:** Flashes ISOs and disk images directly to USB drives and raw block devices (`/dev/sdX`).
- **High-Performance Chunking:** Streams data in optimized 16MB blocks to maximize disk throughput and minimize context switching.
- **Real-Time Progress Bar:** Accurately displays percentage, total written bytes, and real-time write speed (MB/s).
- **Zero-Dependency Binary:** Compiled natively with Nuitka into a standalone ELF executable—no Python runtime or external packages required.

---

## ⚠️ Precautions & Safety Warnings

> [!CAUTION]
> **Data Loss Risk:** `flash` writes directly to raw block devices, overwriting all existing partition tables and data on the target device without confirmation prompts.

1. **Verify Your Target Drive:** Always check device nodes before running the command using `lsblk` or `fdisk -l`.
2. **Never Target System Disks:** Double-check that your destination is not your primary root drive (`/dev/sda`, `/dev/nvme0n1`, etc.).
3. **Unmount Partition Volumes:** Ensure target partitions are not mounted by your desktop environment prior to flashing.
4. **Requires Root Privileges:** Raw block writes require `sudo` to access raw storage nodes under `/dev/`.

---

## One-Click Installation

To instantly download the latest binary release and install it to `/usr/local/bin` in a single command, run:

```bash
sudo curl -L https://github.com/void556/flash/releases/latest/download/flash -o /usr/local/bin/flash && sudo chmod +x /usr/local/bin/flash

---

## 🛠️ Compiling Locally From Source

If you want to modify the source code or compile the standalone binary yourself, you can build it using Python and Nuitka.

### Prerequisites
Make sure you have Python and Nuitka installed on your system:
```bash
pip install nuitka
