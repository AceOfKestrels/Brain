# Zsh on NixOS
Install and enable Zsh:
```nix
programs.zsh.enable = true;
```

Set Zsh as default shell for all users:
```nix
users.defaultUserShell = pkgs.zsh;
```

## Plugins
Plugins must first be installed as packages:
```nix
environment.systemPackages = with pkgs; [
    zsh-powerlevel10k
    zsh-autosuggestions
    zsh-syntax-highlighting
];
```

After that they can be loaded and configured using Zsh promt init:
```nix
programs.zsh.promptInit = ''
    [[ ! -f ${pkgs.zsh-powerlevel10k}/share/zsh-powerlevel10k/powerlevel10k.zsh-theme ]] || 
        source ${pkgs.zsh-powerlevel10k}/share/zsh-powerlevel10k/powerlevel10k.zsh-theme

    [[ -f /etc/zsh/p10k.zsh ]] && source /etc/zsh/p10k.zsh

    [[ -f ${pkgs.zsh-autosuggestions}/share/zsh-autosuggestions/zsh-autosuggestions.zsh ]] && 
        source ${pkgs.zsh-autosuggestions}/share/zsh-autosuggestions/zsh-autosuggestions.zsh

    [[ -f ${pkgs.zsh-syntax-highlighting}/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh ]] && 
        source ${pkgs.zsh-syntax-highlighting}/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh

    ZSH_AUTOSUGGEST_HIGHLIGHT_STYLE='fg=8'
'';
```