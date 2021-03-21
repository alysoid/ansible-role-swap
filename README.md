# [Catena](https://github.com/alysoid/catena) - Swap Ansible Role

[systemd-swap](https://man.archlinux.org/man/systemd-swap.8) is a script for creating hybrid swap space from zram swaps, swap files and swap partitions.

## Role variables

| Variable               | Default  | Info
| ---------------------- | -------- | ------------
| `systemd_swap_cleanup` | `no`     | Remove existing configuration files from `/etc/systemd/swap.conf.d/*.conf`.
| `systemd_swap_conf_d`  | `{}`     | Create custom configuration files in `/etc/systemd/swap.conf.d/*.conf`

---

### `systemd_swap_conf_d`

Manage [swap.conf.d](https://man.archlinux.org/man/swap.conf.5) - systemd-swap configuration files.

Configuration files will have the .conf extension and will be placed in the configuration snippets directory `/etc/systemd/swap.conf.d`. These configuration files can control zram, zswap and swapfc. The following example enables zram with lzo-rle compression:

```yaml
systemd_swap_conf_d:
  override:
    - zram_enabled: 1
    - zram_size: $(( RAM_SIZE / 4 )) # This is 1/4 of ram size by default.
    - zram_count: ${NCPU}            # Device count
    - zram_streams: ${NCPU}          # Compress streams
    - zram_alg: lzo-rle              # Compressor algorithm: lzo, lz4, zstd, lzo-rle
    - zram_prio: 32767               # 1 - 32767
```
