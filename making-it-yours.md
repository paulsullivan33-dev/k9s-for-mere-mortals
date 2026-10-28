# Making It Yours

Outcome: K9s looks and behaves the
way you want, per cluster where that
matters, and every file you might
edit is one you can find — `k9s
info` names them all, and the lab ran
it (transcript t02). The defaults
below are quoted from the lab's
generated `config.yaml` (transcript
t07) — verified. The fancier files
(custom views, jumps, plugins) are
documented from the project, as the
lab created none of its own.

## config.yaml: the main dial

Created on first run — do not write
it from scratch; let K9s generate it,
then edit. The settings a new user
actually reaches for:

```yaml
k9s:
  refreshRate: 2          # seconds between redraws
  readOnly: false         # chapter 9's leash
  apiServerTimeout: 2m0s  # patience for a slow API server
  maxConnRetry: 5         # reconnect attempts before giving up
  ui:
    skin: dracula         # the default theme (see below)
    noIcons: false        # true removes the little icons
    headless: false       # true hides the header block
    enableMouse: false    # true lets the mouse click and scroll
  logger:
    tail: 100             # chapter 8's numbers live here
    buffer: 5000
  thresholds:
    cpu:    {critical: 90, warn: 70}
    memory: {critical: 90, warn: 70}
```

The thresholds colour the numbers:
past 70 the figure warns, past 90 it
criticises — in the default skin,
amber and red. Set them to your
cluster's honest normal, not to the
factory's: a threshold that always
cries wolf teaches you to ignore the
one real fire.

Environment variables override pieces
without editing files: `K9S_CONFIG_DIR`
relocates the whole config folder,
`K9S_SKIN` picks a skin for the
session, and chapter 3's
`KUBECONFIG` steers the cluster side.

## Per-context configs

Settings can also live per context,
in the folder `k9s info` calls
"Context Configs"
(`clusters/<cluster>/<context>/config.yaml`
under the data directory — the lab's
unreachable run really did create its
invented context's folder there).
Anything in the main file can be
overridden for one context: the
production context gets `readOnly:
true` and a sober skin, the practice
context gets neither. This is where
chapter 9's leash belongs — set once,
inherited every visit, no flag to
remember at 2 a.m.

## Skins

A **skin** is a YAML file of colours
in the `skins` folder (path in `k9s
info`), named by the `ui.skin`
setting or `K9S_SKIN`. K9s ships with
a shelf of them — `dracula` is the
default theme — and the
project collects community skins;
writing your own is a copy-and-adjust
job. If a skin file is missing or
malformed, K9s falls back rather
than failing; `--invert` flips a
dark skin light or a light one dark
for the session (it is in the lab's
captured flag list, transcript t01).

## Hotkeys and aliases

- **hotkeys.yaml** binds your own
  keys to commands: a key, a command
  (any command-mode string), and a
  description. Global in scope —
  pick keys that do not fight the
  built-ins on the help screen
  (transcript t05 is the real list).
- **aliases.yaml** is the same idea
  for command-mode words: the lab's
  generated file maps `dp` to
  deployments, `jo` to jobs, `sec` to
  `v1/secrets`, and the role/binding
  families to their short names.
  Add the resources *you* type all
  day; an alias can carry more than
  a name (arguments ride along), and
  `:aliases` shows the union of
  built-ins and yours.

## Custom views and jumps

- **views.yaml** redefines a view's
  columns: which fields appear, in
  what order — built from the
  resource's JSON paths. A pods view
  that shows the node and the image
  up front is the classic first
  custom view.
- **jumps.yaml** defines leaps
  between related resources: from an
  ArgoCD Application to its
  Deployments, from a NodePool to its
  Nodes — the project's own examples.
  A jump says "these two kinds are
  related by this field"; K9s adds
  the key. It builds on chapter 7's
  built-in owner jump (Shift-J).

## Plugins

Chapter 10 met plugins from the
user's side. Writing one is a stanza
in `plugins.yaml`: the view (scope)
it applies to, the key, a description,
and the command to run, which
receives the selected resource's
context as environment variables —
`$NAME`, `$NAMESPACE`, `$RESOURCE_NAME`,
`$KUBECONFIG` and friends, per the
project's documentation. Keep plugins
small, keep them yours, and read any
plugin you download the way you
would read a shell script a stranger
hands you — because that is what it
is, and it runs as you (and, through
your kubeconfig, potentially at your
cluster).
