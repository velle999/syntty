# syntty

A Wayland terminal that links no OpenGL. It rasterises glyphs with FreeType
and paints into a shared-memory buffer, so it starts on a machine with no GPU
driver worth the name, inside a VM, and on a first boot before anything is
configured.

It is the terminal SynapseOS opens, and it is a normal terminal anywhere
else: a pty, a VT parser with scrollback, mouse reporting, and the `-e`
convention other terminals take.

## Using it

```bash
syntty                       # a window, running your shell
syntty -e htop               # …running something else
syntty --hold -e ls          # keep the window open after the command exits
syntty config --example      # a commented config to start from
```

## The parts that are separately useful

The parser, the renderer and the input translation are each reachable from
the command line, which is how the terminal is tested and how a bug report
can be made precise without a screenshot:

```bash
syntty dump FILE             # feed a stream through the parser, print the screen
syntty run -- CMD...         # run CMD on a pty, print the screen it left
syntty render FILE --out s.ppm   # paint that screen to an image
syntty key F1 C-c            # what a keystroke becomes on the child's input
syntty mouse ...             # what a pointer event becomes
syntty bench FILE            # parse throughput, in MB/s
syntty damage-check FILE     # prove damage tracking draws the same pixels
```

`syntty about` says what it can do so far, and `syntty font` says which font
it resolved and what that cost.

## Requires

Wayland, libxkbcommon, FreeType and fontconfig. No GL, no GPU, no compositor
beyond one that speaks `wl_shm`.

## Install

```bash
git clone https://github.com/velle999/syntty
cd syntty && makepkg -si
```

makepkg fetches the source for this PKGBUILD's exact version from this
repository's releases, so a clone can only ever build the source it was
written against. `.SRCINFO` lists what it needs.

## Where this comes from

Developed in [the SynapseOS monorepo](https://github.com/velle999/SYNAPSE),
in `syntty/`. **This repository is generated from it** — the PKGBUILD, a
generated `.SRCINFO` and this README — so issues and patches belong there.

syntty 0.1.0-34 · GPL-2.0-or-later
