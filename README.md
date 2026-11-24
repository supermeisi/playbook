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

Geant4 and ROOT each use a dedicated directory under `/opt`:

| Contents | ROOT | Geant4 |
| --- | --- | --- |
| Source code | `/opt/root/source` | `/opt/geant4/source` |
| Build files | `/opt/root/build` | `/opt/geant4/build` |
| Installation | `/opt/root/install` | `/opt/geant4/install` |

Downloaded source archives are cached directly in `/opt/root` and `/opt/geant4`.
The login-shell environment scripts use the new `install` directories.
Override `root_base_dir` or `geant4_base_dir` to use another base location.

To build and install both packages using this layout:

```bash
ansible-playbook playbook.yml --ask-become-pass --tags geant4,root
```

On machines using the earlier layout, this builds into the new directories.
Existing source, build and installation trees are not moved or deleted.
Each new layout holds one version. Before changing the version variables,
use fresh source and build directories to avoid reusing the previous sources
and CMake cache.

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
