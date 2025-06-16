# sshm – SSH Config Manager

`sshm` is a tiny command-line helper that lets you manage the entries inside your `~/.ssh/config` file **without opening an editor**.  
It supports three simple operations:

| Command       | Purpose                                 |
| ------------- | --------------------------------------- |
| `sshm add`    | Append a new `Host` stanza              |
| `sshm remove` | Delete an existing stanza by alias      |
| `sshm list`   | Show all host aliases in a pretty table |

---

## Features

- Keeps the original formatting and comments of other stanzas intact
- Creates `~/.ssh/config` (and `~/.ssh/`) if they don't exist
- Makes an automatic backup (`config.bak`) whenever it rewrites the file
- Works with or **without** the optional `tabulate` package (better tables when installed)

---

## Installation / Making it Global

### 1. Quick move or symlink (recommended)

```bash
# clone or download this repo first
cd <path-to-your-dir>/sshmanager          # replace with your path
chmod +x sshm                               # ensure it is executable

# Move it into a directory that's already on $PATH
sudo mv sshm /usr/local/bin/                # Homebrew's default bin dir
# ...or leave it in place and create a symlink
sudo ln -s "$PWD/sshm" /usr/local/bin/sshm
```

### 2. Using `pipx` (isolated virtual-env + shim)

```bash
brew install pipx        # or: pip install --user pipx
pipx ensurepath          # adds pipx's bin dir to PATH

cd <path-to-your-dir>/sshmanager   # project root
pipx install .           # installs script + dependency
```

### 3. Plain `pip install`

The repository already ships with a `pyproject.toml`, so you can simply run:

```bash
pip install --user .
```

This installs `sshm` (along with the optional `tabulate` dependency) into your user-site packages and places a console script in `~/Library/Python/3.x/bin` (ensure that directory is on your `$PATH`).

---

## Getting Started

### Display help

```bash
sshm --help
```

### Add a host

```bash
sshm add mybox 192.168.1.10 -u ubuntu -p 2222
```

produces this stanza in `~/.ssh/config`:

```ssh
Host mybox
  HostName 192.168.1.10
  User ubuntu
  Port 2222
```

### List hosts

```bash
sshm list
```

Example output (with `tabulate` installed):

```
╭─────────────┬───────────────────┬────────────┬──────╮
│ Alias       │ HostName          │ User       │ Port │
├─────────────┼───────────────────┼────────────┼──────┤
│ mybox       │ 192.168.1.10      │ ubuntu     │ 2222 │
│ prod-server │ 88.198.150.15     │ root       │ 22   │
╰─────────────┴───────────────────┴────────────┴──────╯
```

### Remove a host

```bash
sshm remove mybox
```

---

## pyproject.toml template (optional packaging)

```
[project]
name = "sshm"
version = "0.1.0"
description = "SSH config manager"
dependencies = ["tabulate>=0.9"]

[project.scripts]
sshm = "sshm:main"
```

---

## Troubleshooting

- **`sshm: command not found`** – Make sure the directory containing `sshm` (or pipx's shims) is on your `PATH` and you opened a new shell.
- **Alias already exists** – `sshm add` refuses to overwrite. Remove first or choose a different alias.
- **tabulate missing warning** – Install it if you want pretty tables: `pip install tabulate`.

---

## License

MIT – do whatever you like, but without warranty.
