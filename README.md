# NightDiamond-cursors

## Overview

This repository contains a collection of NightDiamond family cursor themes for Linux desktops.  
These themes were collected and mirrored from various public sources for ease of installation and preservation, especially for use with NixOS and Home Manager.

## Attribution

**I am _not_ the original creator of these cursor themes.**  
If you are the original author and would like to be credited or have your work removed, please [open an issue](https://github.com/vimlinuz/NightDiamond-cursors/issues) or contact me.

If you have more information about the authorship or original license of a specific theme, please let me know so I can update this repository accordingly.

## Usage (Nix/NixOS/Home Manager)

You can use these themes declaratively in your NixOS or Home Manager configuration by adding a suitable Nix derivation and referencing the desired theme by name.

Example (with Home Manager):

```nix
# home.nix
{ pkgs, ... }:
let
  nightdiamondCursors = pkgs.nightdiamond-cursors;
in
{
  home.pointerCursor = {
    gtk.enable = true;
    x11.enable = true;
    package = nightdiamondCursors;
    name = "NightDiamond-Blue"; # Or NightDiamond-Red, NightDiamond-Fusion
    size = 20;
  };
}
```

Alternatively, you can use a local derivation or overlay if you want more control or customization. For example, if you want to use a different version, patch the theme, or keep your cursor package outside of nixpkgs, you can do the following:

**Using a local derivation:**

```nix
# home.nix
{
  pkgs,
  ...
}:
let
  nightdiamondCursors = pkgs.callPackage ./nightdiamond-cursors.nix {};
in
{
  home.pointerCursor = {
    gtk.enable = true;
    x11.enable = true;
    name = "NightDiamond-Red"; # Or NightDiamond-Blue, NightDiamond-Fusion
    package = nightdiamondCursors;
    size = 20;
  };
}
```

```nix
# nightdiamond-cursors.nix
{
  stdenvNoCC,
  fetchFromGitHub,
  lib,
}:
stdenvNoCC.mkDerivation {
  pname = "NightDiamond-cursors";
  version = "e13db9e75f74e42a68e32c53d96b53334c7b92b1";

  src = fetchFromGitHub {
    owner = "vimlinuz";
    repo = "NightDiamond-cursors";
    rev = "e13db9e75f74e42a68e32c53d96b53334c7b92b1";
    hash = "sha256-4cxQCN5MXGowRi/tzBPL/gTbPXpsMiSZSQr/vLFkcVQ=";
  };

  installPhase = ''
    runHook preInstall
    mkdir -p $out/share/icons
    cp -r NightDiamond-* $out/share/icons/
    runHook postInstall
  '';

  meta = with lib; {
    description = "NightDiamond custom cursor theme";
    homepage = "https://github.com/vimlinuz/NightDiamond-cursors";
    license = licenses.gpl3Plus;
    platforms = platforms.linux;
  };
}
```

## License

Unless otherwise specified in the theme files, these cursor themes are distributed under the [GNU General Public License v3.0 (GPL-3.0)](LICENSE).

---

**Disclaimer:**  
These themes were mirrored from public sources. If you are the original author and want credit or removal, please reach out!
