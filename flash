#!/usr/bin/env python3
import os
import sys
import time


def get_block_device_size(fd):
  """Query the block device ioctl to get the exact size in bytes."""
  import fcntl
  import struct

  # BLKGETSIZE64 ioctl code
  BLKGETSIZE64 = 0x80081272
  try:
    # 8-byte unsigned long long
    buf = fcntl.ioctl(fd, BLKGETSIZE64, b"\x00" * 8)
    return struct.unpack("Q", buf)[0]
  except Exception:
    return None


def flash_copy(src, dst, chunk_size=16 * 1024 * 1024):  # 16 MB chunks
  if not os.path.exists(src):
    print(f"Error: Source '{src}' not found.")
    sys.exit(1)

  source_size = os.path.getsize(src)
  copied = 0
  start_time = time.time()

  print(f"Flashing {src} -> {dst}...")

  try:
    with open(src, "rb") as fsrc, open(dst, "wb", buffering=0) as fdst:
      # Try to determine destination size if it's a block device
      dst_size = get_block_device_size(fdst.fileno())
      total_size = dst_size if dst_size else source_size

      while True:
        buf = fsrc.read(chunk_size)
        if not buf:
          break
        fdst.write(buf)
        copied += len(buf)

        # Force write to disk so progress is accurate
        os.fsync(fdst.fileno())

        # Calculate metrics
        elapsed = time.time() - start_time
        if elapsed > 0:
          speed = (copied / (1024 * 1024)) / elapsed  # MB/s
          percent = min((copied / total_size) * 100, 100.0)
          sys.stdout.write(
              f"\r[{percent:5.1f}%] {copied / (1024*1024):.1f} MB /"
              f" {total_size / (1024*1024):.1f} MB "
              f"({speed:.2f} MB/s)    "
          )
          sys.stdout.flush()

    print("\nFlash completed successfully and synced!")

  except PermissionError:
    print("\nError: Permission denied. Try running with 'sudo'.")
  except KeyboardInterrupt:
    print("\nFlash aborted by user.")


if __name__ == "__main__":
  if len(sys.argv) < 3:
    print("Usage: sudo flash <image-name.iso> /dev/sdX")
  else:
    flash_copy(sys.argv[1], sys.argv[2])
