# LinuxAgent — Agente shell per Devuan + runit

Agente e cheat-sheet per amministrare Devuan senza systemd (PID 1 = runit).
Riusabile su altri PC: clona e usa `AGENTS.md` come istruzioni OpenCode.

## Contenuto

- `AGENTS.md` — regole agente: file/cartelle, servizi runit (`sv`), apt, diagnostica, tarball manuali in `/opt`, template `.desktop` stile Postman.

## Requisiti

- Devuan 6+ (excalibur) con runit: `/sbin/init -> runit-init`, `ps -p 1 -o comm=` = `runit`
- `apt`, `sv`, `runsvdir`
- Layout atteso:
  - `/etc/sv` disponibili, `/etc/service -> /etc/runit/runsvdir/current` abilitati
  - `/opt` tarball manuali, `/usr/local/bin` symlink, `/usr/share/applications` .desktop

## Uso

```bash
git clone https://github.com/miketester10/DevuanLinuxAgent LinuxAgent
cd LinuxAgent
cat AGENTS.md
# con OpenCode: apri questa cartella come workspace, AGENTS.md viene caricato in automatico
```

Su nuovo PC verifica:
```bash
cat /etc/os-release
ps -p 1 -o comm=
ls -1 /etc/service; ls -1 /etc/sv
df -h; apt list --upgradable
```

## Regole rapide

- Mai `systemctl/journalctl` → usa `sudo sv status/start/stop/restart NOME`
- Abilita: `sudo ln -s /etc/sv/NOME /etc/service/` — disabilita: `sudo sv down NOME; sudo rm /etc/service/NOME`
- Tarball: scompatta in `/opt/NOME-VERSIONE`, `sudo ln -sf /opt/.../bin/nome /usr/local/bin/nome`, `.desktop` in `/usr/share/applications` con `Exec=... %U`
- Apt: `sudo apt update && sudo apt upgrade`, `sudo apt install NOME`

## Esempio .desktop

```ini
[Desktop Entry]
Version=1.0
Type=Application
Name=Postman
Icon=/opt/Postman/app/resources/app/assets/icon.png
Exec=/opt/Postman/Postman %u
Comment=Postman API Platform
Categories=Development;IDE;
Terminal=false
StartupWMClass=Postman
```

Default generico: usa `%U` (lista file/URL in singola istanza). `%u` solo per singola istanza.
