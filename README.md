# Local software installation

This Ansible project installs Geant4, ROOT, Zoom and ZapZap on the local
Ubuntu/Debian machine. Ansible must already be installed. Run the commands below
from this directory. The project uses only built-in Ansible modules.

Install only Zoom:

```bash
ansible-playbook playbook.yml --ask-become-pass --tags zoom
```

Install only ZapZap:

```bash
ansible-playbook playbook.yml --ask-become-pass --tags zapzap
```

Install all software, including Zoom and ZapZap:

```bash
ansible-playbook playbook.yml --ask-become-pass
```

Geant4 and ROOT each use a dedicated directory under `/opt`:

| Contents | ROOT | Geant4 |
| --- | --- | --- |
| Source code | `/opt/root/root-6.40.04-source` | `/opt/geant4/geant4-11.3.2-source` |
| Build files | `/opt/root/root-6.40.04-build` | `/opt/geant4/geant4-11.3.2-build` |
| Installation | `/opt/root/root-6.40.04-install` | `/opt/geant4/geant4-11.3.2-install` |

Downloaded source archives are cached directly in `/opt/root` and `/opt/geant4`.
Each subfolder includes the package name and its configured version. The names
follow `root_version` and `geant4_version`, keeping different versions in
separate source, build and installation directories.
The login-shell environment scripts load the installation selected by the
playbook.
Override `root_base_dir` or `geant4_base_dir` to use another base location.

To build and install both packages using this layout:

```bash
ansible-playbook playbook.yml --ask-become-pass --tags geant4,root
```

On machines using the earlier layout, this builds into the new directories.
Existing source, build and installation trees are not moved or deleted.

The `zoom` and `zapzap` roles install the `flatpak` and `ca-certificates`
packages, add Flathub system-wide, and install `us.zoom.Zoom` and
`com.rtosta.zapzap` with their required Flatpak runtimes for all users. Built-in
command tasks check configured remotes and installed application IDs before
making changes, so repeated runs keep an existing installation in place.
Each app ID, remote name and repository URL can be overridden using the
variables in its role's `defaults/main.yml`.

After installation, open Zoom from the desktop application menu or run this
command as your regular desktop user:

```bash
flatpak run us.zoom.Zoom
```

If Zoom does not appear in the application menu immediately after installing
Flatpak for the first time, log out and back in.

After installation, open ZapZap from the desktop application menu or run this
command as your regular desktop user:

```bash
flatpak run com.rtosta.zapzap
```

The role installs Zoom only when its application ID is absent from the system
installation. To update Zoom later:

```bash
sudo flatpak update --system us.zoom.Zoom
```

To update ZapZap later:

```bash
sudo flatpak update --system com.rtosta.zapzap
```
