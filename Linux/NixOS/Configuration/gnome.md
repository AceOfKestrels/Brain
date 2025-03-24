# Gnome on NixOS
## Extensions
Extensions are installed as packages:
```nix
environment.systemPackages = with pkgs; [
    gnomeExtensions.dash-to-dock
];
```

They can be managed like normal using the Gnome Extensions app. 
After installing a new extensions you must reload the session for them to be recognized. On Wayland you can simply log out and back in, and on X11 you can press `Alt`+`F2` and type in `r` to reload the session directly.