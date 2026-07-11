# Contributing

This project uses [**mise**](https://mise.jdx.dev) to pin tools, expose tasks, and wire git hooks.

## Install mise (first time on this machine)

<details>
<summary><b>macOS / Linux</b></summary>

```sh
brew install mise
# or: apt install mise · dnf install mise · pacman -S mise · …
```

No package manager? See the [installation docs](https://mise.jdx.dev/installing-mise.html).

</details>

<details>
<summary><b>Windows</b></summary>

```sh
winget install jdx.mise
```

</details>

Activate in your shell, then confirm:

```sh
echo 'eval "$(mise activate zsh)"' >> ~/.zshrc   # zsh
echo 'eval "$(mise activate bash)"' >> ~/.bashrc # bash
# restart shell
mise doctor
```

## Set up the project

```sh
mise trust && mise run setup
```

## Everyday commands

| Command                                  | What it does                                                          |
| ---------------------------------------- | --------------------------------------------------------------------- |
| `mise run check` (alias `mise run lint`) | Run all linters, formatters, and validators. Add `--fix` to auto-fix. |
| `mise run test`                          | Run the test suite (placeholder — none yet).                          |
| `mise tasks`                             | List every available task.                                            |

Run `mise run <task> --help` to see a task's options.

## Git hooks

`mise run setup` runs `mise install`, which self-installs git hooks (via [hk](https://hk.jdx.dev)). On commit, staged files are formatted and linted — the same `check` that runs in CI.

Skip for a work-in-progress commit: `git commit --no-verify`.
