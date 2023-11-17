# AppStream Generator
<img align="right" src="data/templates/default/static/img/asgen.png">

AppStream is an effort to provide additional metadata and unique IDs for all software available in a Linux system.
This repository contains the server-side of the AppStream infrastructure, a tool to generate metadata from distribution packages. You can find out more about AppStream collection metadata at [Freedesktop](https://www.freedesktop.org/software/appstream/docs/chap-CollectionData.html).

The AppStream generator is currently primarily used by Debian, but is written in a distribution agnostic way. Backends only need to implement [two interfaces](src/asgen/backends/interfaces.d) to to be ready.

If you are looking for the AppStream client-tools, the [AppStream repository](https://github.com/ximion/appstream) is where you want to go.


## Install from Flathub

You can install an up-to-date version of AppStream Generator from [Flathub](https://flathub.org) if you just
want to quickly test the software with your repository:
```ShellSession
# Add Flathub remote
sudo flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
# Install appstream-generator
flatpak install org.freedesktop.appstream.generator

# Run appstream-generator
flatpak run org.freedesktop.appstream.generator --help
```

You can use AppStream Generator as [documented](docs/usage.md), but you will need to replace all
`appstream-generator` commands with `flatpak run org.freedesktop.appstream.generator` and may need
to set the workspace as absolute path using `-w` instead of relying on autodetection.


## Usage

Take a look at the [docs/](docs/index.md) directory in the source tree for information on how to use the generator and write configuration files for it.


## Development
![Build Test](https://github.com/ximion/appstream-generator/workflows/Build%20Test/badge.svg)

### Build dependencies

 * LDC[1]
 * Meson (>= 0.46) [2]
 * GLibD [3]
 * AppStream [4]
 * libarchive (>= 3.2) [5]
 * LMDB [6]
 * GirToD [7]
 * LibSoup
 * Cairo
 * GdkPixbuf 2.0
 * RSvg 2.0
 * FreeType
 * Fontconfig
 * Pango
 * Yarn (optional) [8]

[1]: https://github.com/ldc-developers/ldc/releases
[2]: http://mesonbuild.com/
[3]: https://github.com/gtkd-developers/GlibD
[4]: https://github.com/ximion/appstream
[5]: https://libarchive.org/
[6]: https://symas.com/lmdb/
[7]: https://github.com/gtkd-developers/gir-to-d
[8]: https://yarnpkg.com/

On Debian and derivatives of it, all build requirements can be installed using the following command:
```ShellSession
sudo apt install meson ldc gir-to-d \
    libappstream-dev libappstream-compose-dev libsoup2.4-dev libarchive-dev \
    libgdk-pixbuf2.0-dev librsvg2-dev libcairo2-dev libfreetype-dev libfontconfig1-dev \
    libpango1.0-dev liblmdb-dev libglibd-2.0-dev \
    yarnpkg
```

### Build instructions

To build the tool with Meson, create a `build` subdirectory, change into it and run `meson .. && ninja` to build.
In summary:

```ShellSession
$ mkdir build && cd build
$ meson -Ddownload-js=true ..
$ ninja
$ sudo ninja install
```

We support several options to be set to influence the build. Change into the build directory and run `mesonconf` to see them all.

You might want to perform an optimized debug build by passing `--buildtype=debugoptimized` to `meson` or just do a release build straight
away with `--buildtype=release` in case you want to use the resulting binaries productively. By default, the build happens without optimizations
which slows down the generator.


## Hacking

Pull-requests and patches are very welcome! If you are new to D, it is highly recommended to take a few minutes to look at the D tour to get a feeling of what the language can do: https://tour.dlang.org/
