# The kernel snap

The main focus of this example is to show how to cross-build a kernel snap,
including some of the more common modifications one would make when doing so.

This snap isn't necessarily meant to function out-of-the-box; for a more
maintained example, check the [IoT Field kernel snaps repository](https://github.com/canonical/iot-field-kernel-snap).

## The snap

This snap is composed of four primary pieces:

1) The official Debian Nezha kernel,
2) The Debian linux firmware package,
3) An initrd built from a minimal Noble base image, and
4) a WiFi driver

It is intended for use by both the Nezha and Sipeed LicheeRV boards, one very
accessible and another very, VERY accessible pieces of RISC-V hardware. While
not the most powerful boards available, they offer a simple development platform
to facilitate experimentation and native building.

## Building

Install a recent enough snapcraft and build:

```bash
  snap install --classic --channel=9.x/stable snapcraft
  snapcraft
```

## Continuing the exercise

A lot of kernel options are disabled in this example. This dramatically shrinks
kernel snap size (though it could be shrunk more), with the trade-off of making
the `snapcraft.yaml` a bit more unwieldy.

There are alternatives to this, of course! We could consolidate this into a
defconfig file we then specify should be used using the `kernel-kdefconfig`
kernel plugin option. Indeed, this may be a more appropriate method as the
kernel being built is no longer 1:1 equivalent to the Ubuntu kernel, so in some
sense the kernel ABI has changed - the kernel plugin does not generate this
Ubuntu-specific kernel information when supplying a defconfig.

We would do this by:

```bash
  git clone https://git.launchpad.net/~canonical-kernel/ubuntu/+source/linux-riscv/+git/noble
  cd noble
  export ARCH=riscv CROSS_COMPILE=riscv64-linux-gnu-
  fakeroot debian/rules clean genconfigs || true
  cat CONFIGS/riscv64-linux-gnu-config.flavour.generic > .config
  { echo CONFIG_FOO=x ; echo CONFIG_BAR=x ; ... ; } >> .config
  make oldconfig
  make savedefconfig
```

These steps ensure that the base config used for building the kernel at least
includes the Ubuntu-specific options, and adding our own `CONFIG_` values at the
end means that our choices will take precedence over those when regenerating the
config with `make oldconfig`.

`make savedefconfig` will create a file named `defconfig` in the top-level of
the kernel source. Copy the created defconfig file to the top-level of this
example snap and modify the `kernel` part:

```yaml
  kernel-kdefconfig: ["mine.config"]
  override-pull: |
    craftctl default
    cp -f "${CRAFT_PROJECT_DIR}/my_defconfig" \
      "${CRAFT_PART_SRC}/kernel/configs/mine.config"
```

And remove the `kernel-kconfigs` option.

When the plugin runs `make <specified defconfigs>`, the kernel will correctly
pick up `my_defconfig` in `kernel/configs` and generate a complete config from
it.
