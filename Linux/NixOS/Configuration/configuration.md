# Configration
The `configuration.nix` located in `/etc/nixos/` is the most important config file in NixOS. It contains all configuration for the entire system, including settings and installed packages. Other configuration files can be imported here.

Use [`nixos-rebuild switch`](../Commands/nixos-rebuild.md) to reload the configuration and rebuild the system.

## Packages
Packages can be installed by adding them to the `environment.systemPackages` array. Per-user packages can also be installed under the respective user entry.
```nix
environment.systemPackages = with pkgs; [
    git
    firefox
];
```

## Import configuration files
Other configuration files can be imported by adding them to the `imports` array. By default this includes the hardware configuration created by Nix. 
```nix
imports = [
    ./hardwareConfiguration.nix
    ./gnome.nix # GNOME Configuration
    ./zsh.nix # ZSH Configuration
];
```