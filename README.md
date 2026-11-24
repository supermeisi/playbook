# Local software installation

This Ansible project installs Geant4, ROOT and Zoom on the local Ubuntu/Debian
machine. Ansible must already be installed. Run the commands below from this
directory. The project uses only built-in Ansible modules.

Install only Zoom:

```bash
ansible-playbook playbook.yml --ask-become-pass --tags zoom
```

Install all software, including Zoom:

```bash
ansible-playbook playbook.yml --ask-become-pass
```

The `zoom` role installs the `flatpak` and `ca-certificates` packages, adds
Flathub system-wide, and installs `us.zoom.Zoom` with its required Flatpak
runtimes for all users. Built-in command tasks check configured remotes and
installed application IDs before making changes, so repeated runs keep an
existing installation in place.
The app ID, remote name and repository URL can be overridden using the variables
in `roles/zoom/defaults/main.yml`.

After installation, open Zoom from the desktop application menu or run this
command as your regular desktop user:

```bash
flatpak run us.zoom.Zoom
```

If Zoom does not appear in the application menu immediately after installing
Flatpak for the first time, log out and back in.

The role installs Zoom only when its application ID is absent from the system
installation. To update Zoom later:

```bash
sudo flatpak update --system us.zoom.Zoom
```
