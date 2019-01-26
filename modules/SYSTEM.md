Configuration de serveurs
==
Paquets de base
-
### [&#9756; Retour au menu](../README.md)
Aliases

    nano .bashrc
    
Modifier comme ceci

    if [ "$TERM" != "dumb" ]; then
        eval "`dircolors -b`"
        alias ls='ls --color=auto'
    fi
    
    # ls Aliases
    alias ll='ls -alL'
    alias la='ls -A'
    alias l='ls -CF'
    
    PS1="\u [\w] > "

Mise à jour du system
    
    source .bashrc && 
    apt-get update -y &&
    apt-get install build-essential -y
    
Installation des paquets de base

    apt-get install sudo git
   
&copy; 2019 [VsWeb](https://vsweb.be) 

















