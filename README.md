# darkshell (`dsh`)

`dsh` is a personal Bash shell configuration focused on fast day-to-day
operations, short Unix-style aliases, Kubernetes workflows, Git shortcuts,
network diagnostics, and a few convenience functions for development and
infrastructure work.

The main file is intended to be sourced into an interactive shell as `~/.dsh`.

```sh
source ~/.dsh
```

## Features

- Compact aliases for common Unix commands
- Git aliases and formatted `git log` views
- Kubernetes aliases for `kubectl`, `kubectx`, and `kubens`
- Helper functions for pods, images, volumes, labels, events, resources, and node capacity
- TLS certificate inspection helpers using `openssl`
- Network helpers for `ping`, `curl`, `mtr`, DNS, and routing
- Simple dice RPG aliases using `shuf`
- Light/dark terminal-aware theme choices for `bat` and Git log colors
- Bash completion wiring for custom Kubernetes aliases

## Installation

Clone the repository or download the `.dsh` file:

```sh
git clone https://github.com/akosela/dsh.git
cp dsh/.dsh ~/.dsh
```

```sh
curl -fsSL https://raw.githubusercontent.com/akosela/dsh/master/.dsh -o ~/.dsh
```

Then source it from your Bash startup file.

For Bash login shells, add this to `~/.bash_profile`:

```sh
if [ -f ~/.bashrc ]; then
  . ~/.bashrc
fi
```

For interactive Bash shells, add this to `~/.bashrc`:

```sh
if [ -f ~/.dsh ]; then
  . ~/.dsh
fi
```

Reload your shell:

```sh
source ~/.bashrc
```

or:

```sh
source ~/.bash_profile
```

## Configuration

### Terminal background

Set the terminal background mode near the top of `.dsh`:

```sh
terminal_background=light  # light|dark
```

This controls theme choices for aliases such as `b`, `glg`, and `lg`.

For example, the `bat` alias uses a light theme when `terminal_background=light` and a dark theme otherwise.

### Kubernetes config

If you use multiple Kubernetes config files, export `KUBECONFIG` before or inside `.dsh`:

```sh
export KUBECONFIG="$HOME/.kube/config-minikube:$HOME/.kube/config-rke:$HOME/.kube/config"
```

Then verify contexts:

```sh
ctl
```

## Core aliases

`dsh` defines short aliases for common shell tools:

```sh
alias l='ls'
alias ll='ls -Fl'
alias llh='ls -Flh'
alias c='cat '
alias g='grep'
alias gc='grep --color'
alias h='head'
alias t='tail'
alias m='man'
alias v='vi'
alias x='exit'
```

It also provides dice RPG helpers using `shuf`:

```sh
alias d6='shuf -i 1-6 -n 1'
alias 2d6='shuf -i 1-12 -n 1'
alias d20='shuf -i 1-20 -n 1'
alias d100='shuf -i 1-100 -n 1'
```

## Git aliases

Common Git operations are shortened:

```sh
alias st='git status'
alias add='git add'
alias co='git checkout'
alias cm='git commit -am'
alias dif='git diff'
alias br='git branch -vv'
alias psh='git push'
alias pull='git pull'
alias rebase='git rebase'
alias stash='git stash'
```

Formatted log aliases are also included:

```sh
glg
lg
```

`glg` shows a graph log, while `lg` limits the output to the latest 10 commits.

## Kubernetes aliases

`dsh` is heavily optimized for Kubernetes work.

Examples:

```sh
alias get='kubectl get'
alias des='kubectl describe'
alias app='kubectl apply'
alias del='kubectl delete'
alias log='kubectl logs'
alias exe='kubectl exec'
alias deb='kubectl debug'
alias ns='kubens'
alias ctl='kubectx'
```

Short resource helpers include:

```sh
,       # get pod
,e      # get pod (with errors only)
gp      # get pod
gpw     # get pod -o wide
gn      # get node
gs      # get svc
gi      # get ingress
gse     # get secret
gcm     # get configmap
gpv     # get pv
gpvc    # get pvc
```

## Kubernetes helper functions

### `get`

A wrapper around `kubectl get` that formats common resource types with `awk` and `column`.

```sh
get pod
get node
get svc
get deployment
get pvc
```

### `eve`

Shows Kubernetes events globally, by namespace, or for a pod.

```sh
eve
eve my-pod
eve my-pod -n my-namespace
eve -n my-namespace
```

### `img`

Lists container images used by pods.

```sh
img          # current namespace
img -a       # all namespaces
img my-pod   # selected pod
```

### `vol`

Shows pod volumes and volume mounts in a readable format.

```sh
vol my-pod
```

### `res`

Shows current resource usage and configured requests/limits for a pod.

```sh
res my-pod
```

### `laststate`

Shows recent container last-state information from `kubectl describe pod`.

```sh
laststate my-pod
```

## TLS helpers

`dsh` includes several OpenSSL wrappers:

```sh
tls host        # print certificate details
tlsend host     # print certificate expiration
tlspem host     # print PEM certificate
tlsv host       # verbose s_client output
tlscheck cert.pem cert.key    # check whether an X.509 certificate and RSA private key match.
```

Example:

```sh
tlsend example.com
```

## Network helpers

```sh
png host        # short ping output
cu host         # curl HTTPS headers and TLS summary
mtr host        # report-mode mtr
mtr -t host     # terminal mtr mode
```

DNS and network-related aliases include:

```sh
alias a='ansible'
alias ap='ansible-playbook'
alias n='netstat -pant'
alias i='ip -br a'
alias r='ip r'
```

## Encryption helpers

Symmetric GPG encryption and decryption helpers:

```sh
enc file
dec file.gpg
```

`enc` uses AES-256 and ASCII armor output.

## Dependencies

The configuration works best with the following tools installed:

- `bash`
- `coreutils`
- `git`
- `kubectl`
- `kubectx`
- `kubens`
- `bash-completion`
- `bat`
- `jq`
- `openssl`
- `gpg`
- `mtr`
- `curl`
- `watch`

On macOS with MacPorts, some GNU tools are expected under:

```sh
/opt/local/libexec/gnubin
/opt/local/bin
```

## Background: Civilization Y

`dsh`, or **darkshell**, is a small Unix-style shell environment imagined as
a fragment of a much older future: the command interface of **Civilization
Y** — *Yotta*.

In the twenty-first century, a nuclear world war destroyed most of
human civilization. Nation-states vanished, global networks collapsed,
and the accumulated machinery of the old world became either radioactive
debris or half-understood myth. The few surviving groups endured in sealed
shelters, underground facilities, mountain enclaves, and isolated technical
colonies. Over centuries, one of these settlements grew into **the City**
— known simply by that name because, for its inhabitants, there was no
other city left.

The City became the last organized human colony on Earth.

Outside its walls lay the cold zones, the ruins, the contaminated industrial
belts, and the territories of those who lived beyond civic order. Inside,
humanity survived through discipline, rationing, technical continuity, and
an almost religious respect for working systems. The old graphical interfaces
of the pre-war world were gradually abandoned. They required too much power,
too much visual complexity, too many opaque layers. The City’s engineers
returned to the most durable interface humanity had ever built: text.

By the year **2990**, the universal operating system of Civilization Y
is **Dunix** — a distant descendant of Unix, Linux, BSD, and countless
forgotten post-war operating systems. It is not a museum copy of the old
systems, but their cultural and technical heir. Its philosophy is old and
severe: small tools, clear text, pipes, files, permissions, processes,
terminals. What survived was what could be repaired, transmitted, typed,
audited, and understood.

`dsh` is the shell of that world.

It is not meant to be a large replacement for Bash, Zsh, or any modern
shell. Instead, it is a fictionalized shell layer: a compact command
environment that tries to evoke what a late-third-millennium terminal might
feel like if it still carried the DNA of classic Unix. It favors short
commands, aliases, terse status views, direct inspection, and fast movement
through systems. It treats the command line not as a fallback for experts,
but as the primary human-machine interface of a civilization rebuilt around
terminals.

In Civilization Y, terminals are everywhere. They are embedded in maintenance
corridors, archive rooms, reactor control panels, medical stations, hydroponic
farms, transit gates, military bunkers, and personal workstations. A
citizen of the City does not “open an app”; they enter a command. A
technician does not click through dashboards; they query logs, inspect
processes, trace routes, check certificates, list pods, tail events, and
move through contexts. The terminal is both instrument and language.

The name **darkshell** reflects that setting. It is the shell used in a dark
age after the old world: a command language for dim rooms, sealed habitats,
emergency consoles, and surviving networks. It belongs to a future where
computing has become more austere, not more decorative. Text is durable. Text
can be copied by hand. Text can be transmitted over degraded links. Text
can survive when every richer interface fails.

The project also has a retro-computing spirit. It deliberately leans
into the vocabulary of classic Unix: `cat`, `grep`, `sed`, `awk`, `sort`,
`uniq`, `ps`, `df`, `du`, `strings`, `touch`, `tar`, `nc`, `dig`, `mtr`,
and dozens of small operational commands. The aliases are not just shortcuts; 
they are part of the imagined ergonomics of a future operator who lives in the
shell all day.

`dsh` therefore exists in two layers.

On the practical layer, it is a personal shell configuration: aliases,
functions, prompt behavior, Kubernetes helpers, Git shortcuts, TLS inspection
helpers, and small conveniences for daily Unix-like work.

On the fictional layer, it is a recovered interface artifact from Civilization
Y: a glimpse of the operating culture of the City in the year 2990, when
**Dunix** runs the remaining human infrastructure known as **YANS** (Yotta
Advanced Network System) and `dsh` is the shell through which operators speak
to machines.

The premise is inspired by old European post-apocalyptic science fiction,
especially stories of sealed cities, frozen zones, forgotten wars, and
humanity surviving inside the technical remains of a destroyed world. In
that tradition, `dsh` imagines a future where the most advanced interface is
not holographic or neural, but textual: a prompt, a command, and a response.

Civilization Y did not preserve everything.

It preserved enough.

And at the center of that survival is the terminal.


## Author

Andy Kosela <andy.kosela@gmail.com>

Project URL: <https://github.com/akosela/dsh>
