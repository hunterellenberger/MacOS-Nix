# MacOS-Nix

The following will detail all required steps to get my Mac setup with all required settings and software needed for my daily workflow. This will mainly touch on getting Nix-Darwin working on the system and then from there the preconfigured flake.nix file will do the heavy lifting. 

## Setup

### Installing Nix

The first required component of the setup is Nix. This can be aquired by running `$ curl --proto '=https' --tlsv1.2 -L https://nixos.org/nix/install | sh` as of writing. If anything has been changed you can review the installation steps at [Nix Download](https://nixos.org/download/).

The installation can then be checked by running the following: `nix --version`.

### Retrieving Configuration

To get the flake configuration, pull down the files from this repo. You may as well grab dotfiles during this step. Mine are located [here](git@github.com:hunterellenberger/dotfiles.git). There may be a way to pull these files down with the nix config, but I will need to look into this later.

### Creating Directory for Configuration

Nix-Darwin recommends that you create a directory in /etc called nix-darwin to store your configuration files. I will be adhering to that here. To create the directory, give yourself access to the directory, and copy your given flake config into the directory run the following.

```zsh
sudo mkdir -p /etc/nix-darwin
sudo chown $(id -nu):$(id -ng) /etc/nix-darwin
cd /etc/nix-darwin

cp ~/MacOS-Nix/flake.nix .
cp ~/MacOS-Nix/flake.lock .
```
Make sure that the hostname following `darwinConfigurations` matches your own. It can be acquired with `scutil --get LocalHostName`.

### Apply Nix flake

This is the final step in the process is applying the changes made to the flake. This is done by executing the command `sudo nix run nix-darwin/nix-darwin-26.05#darwin-rebuild --extra-experimental-features "nix-command flakes" -- switch --flake .#Hunters-MacBook-Pro`. 

To apply changes made to the Nix flake run `sudo darwin-rebuild switch --flake /etc/nix-darwin#Hunters-MacBook-Pro`.

