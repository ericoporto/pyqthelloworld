# pyqthelloworld
[![Build Status](https://travis-ci.com/ericoporto/pyqthelloworld.svg?branch=master)](https://travis-ci.com/ericoporto/pyqthelloworld)

pyqt hello world!

## Why this?

I made so I could test building a snap from the [`snapcraft.yaml`](https://github.com/ericoporto/pyqthelloworld/blob/master/snap/snapcraft.yaml) below

```YAML
name: pyqthelloworld
version: 0.1.0
summary: pyqt hello world
description: |
 a pyqt5 python3 hello world test
grade: devel # must be 'stable' to release into candidate/stable channels
confinement: strict # use 'strict' once you have the right plugs and slots
base: core18

apps:
  pyqthelloworld:
    command: desktop-launch $SNAP/bin/pyqthelloworld
    plugs:
    - desktop
    - desktop-legacy
    - wayland
    - x11
    - opengl

parts:
  # This part installs the qt5 dependencies and a `desktop-launch` script to initialise
  # desktop-specific features such as fonts, themes and the XDG environment.
  # 
  # It is copied straight from the snapcraft desktop helpers project. Please periodically
  # check the source for updates and copy the changes.
  #    https://github.com/ubuntu/snapcraft-desktop-helpers/blob/master/snapcraft.yaml
  # 
  desktop-qt5:
    source: https://github.com/ubuntu/snapcraft-desktop-helpers.git
    source-subdir: qt
    plugin: make
    make-parameters: ["FLAVOR=qt5"]
    build-packages:
      - build-essential
      - qtbase5-dev
      - dpkg-dev
    stage-packages:
      - libxkbcommon0
      - ttf-ubuntu-font-family
      - dmz-cursor-theme
      - light-themes
      - adwaita-icon-theme
      - gnome-themes-standard
      - shared-mime-info
      - libqt5gui5
      - libgdk-pixbuf2.0-0
      - libqt5svg5 # for loading icon themes which are svg
      - try: [appmenu-qt5] # not available on core18
      - locales-all
      - xdg-user-dirs
      - fcitx-frontend-qt5

  pyqthelloworld:
    after: [desktop-qt5]
    plugin: python
    python-version: python3
    source: git://github.com/ericoporto/pyqthelloworld
    source-type: git
    build-packages:
      - python3
      - python3-pyqt5
      - execstack
    stage-packages:
      - python3
      - python3-pyqt5
      - libc-bin
      - locales
```
