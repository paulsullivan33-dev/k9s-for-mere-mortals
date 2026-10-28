# Writing criteria for this guide

Every "for Mere Mortals" guide is written to the same nine-point
standard, so a reader who trusts one can trust the rest. Here it
is, with the verification notes for this particular guide below.

1. **Fix it first.** Reader outcomes lead, not action items: each
   chapter opens by naming the outcome it delivers, shows a check
   the reader can run to confirm they got it, and a reasonable
   reader finishes able to state what was done, what it means,
   and what to try next.
2. **Plain language.** Short sentences, everyday vocabulary,
   technical terms defined on first use. No slang, and no
   idioms that only make sense in one country — the reader may
   be translating as they go.
3. **No dead ends.** A guide never tells the reader to do
   something it cannot show them how to verify. Every claim a
   reader relies on — an install, a command, an output — is
   something this guide demonstrates or clearly labels
   documented-only below.
4. **Live-verified where it matters.** Procedures are run for
   real in the guide's lab wherever the build machine allows it,
   and the transcripts kept in the repository are the evidence.
   What could not be run is listed honestly below, never
   presented as if it had been.
5. **General audiences.** Nothing in this guide — examples,
   fixtures, names, addresses — is modelled on any particular
   reader or author's real life, network, or organisations.
   The lab's traffic is the lab talking to itself.
6. **Standalone.** A reader needs this guide, its lab, and
   nothing else. Where another guide would genuinely help, it
   is pointed to once, by name, and the chapter still completes
   without it.
7. **Honest about limits.** Chapter 13 names what the tool
   cannot do: anything at all without a reachable API server,
   seeing or changing more than the permissions in your
   kubeconfig allow, remembering what the cluster looked like
   yesterday, keeping any record of what you changed, and the
   legal edge around administering anything that is not yours.
8. **The ethics rule is in the text, early.** Chapter 1 and the
   README state the rule plainly — *your clusters, or explicit
   permission to administer what you can reach* — before any
   procedure depends on it, and later chapters point back to it
   where the tooling would make crossing the line easy (the
   delete keys of chapter 9, the benchmark of chapter 10, and
   other people's data in the logs of chapter 8).
9. **Builds clean.** The site builds with MkDocs in strict mode
   with no warnings, and a fresh reader following the chapters
   in order reaches the stated outcome of each.

## Verification notes for this guide

The build machine is a 2-CPU, 7.7 GB Ubuntu 24.04 sandbox.
K9s **v0.51.0** (commit 558caafe7ba067467de46b320cc22ef11fef9c34)
was downloaded as the official release tarball from the
project's GitHub releases page, unpacked into a scratch
folder, and run from there; nothing was installed
system-wide. All transcripts live in `k9s-lab/transcripts/`.

Run for real, with transcripts as evidence:

- `k9s version` (logo, version, commit, date) and the full
  `k9s --help` flag list (t00, t01).
- `k9s info` listing every configuration path it uses, run
  against fresh XDG folders — including its honest ERROR line
  about the not-yet-existing config file (t02).
- A real terminal session (tmux) against an invented
  kubeconfig pointing at an address where nothing listens:
  K9s starts, lands on the contexts view with an `n/a`
  header and `[RW]` posture, fails the connectivity check
  on Enter and stays put, flashing the error (t03).
- The `:aliases` view, opened cluster-free, listing all 23
  built-in command aliases (t04), and the `?` help screen
  with its real key mnemonics (t05).
- The first run's file behaviour: `config.yaml` and
  `aliases.yaml` created in the config folder (defaults
  quoted in chapters 2, 8, 9 and 11 are read from the
  generated file itself, t07), a per-context folder created
  under the data folder, and the log recording the whole
  connection attempt — the TLS `x509` error, then
  `connection refused`, the connectivity toast, and the
  `[CONFIG] Saving K9s config to disk` line (t06).

Documented only — described from the project's
documentation and the references the program itself
prints, not run, and the text does not claim otherwise:

- Everything that needs a living cluster. A disposable
  cluster was attempted for real: kind v0.30.0 with its
  experimental podman provider failed twice in this
  sandbox (no `/lib/modules` for the node image; then
  container networking not permitted and no fuse device
  for the overlay mount), and the docker daemon was not
  reachable at all (t08). The series' Kubernetes guide
  was likewise verified client-side only; this guide
  inherits that ceiling and says so in the README,
  chapter 1 and the chapters concerned.
- The resource views on a real cluster, navigation and
  sorting beyond the cluster-free views, the four filters
  on live rows, describe/YAML/edit, pod logs and previous
  logs, and every action in chapter 9 (delete, kill,
  scale, restart, cordon, drain, shell).
- Port forwarding, benchmarks, the contents of the
  pulses and xray views, the Popeye scan, and plugins in
  action. (The *names* of these views are verified —
  they are rows in the captured aliases view, t04.)
- Package-manager installs (Homebrew, winget and the
  rest); the lab used the release binary. The shells'
  completion subcommand is listed in the verified help.
- Skins, hotkeys, custom views and jumps as working
  edits; the files' locations are verified via
  `k9s info` (t02) and the default config/aliases files
  are real generated artifacts (t07), but no customised
  file was run against a cluster.

Fixtures are this guide's own and deliberately generic:
one invented kubeconfig with a context named `nowhere`,
a user named `nobody`, a token that is not a real token,
and an API address on this machine's loopback where the
lab deliberately ran nothing. No example is modelled on
any real person, cluster, service or organisation.
