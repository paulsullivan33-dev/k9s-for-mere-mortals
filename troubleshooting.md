# Troubleshooting

Outcome: the four failures everyone
meets, each with the check that names
it — and the master key to all of
them: the log.

## The master check: read the log

K9s writes a running log of itself
(`k9s info` names the path; on Linux
it is `~/.local/state/k9s/k9s.log`).
Nearly every failure below is written
there in plain words, usually with
the API server's own error attached.
The lab's log is the evidence base
for half this chapter; its key lines
are kept as transcript t06. Start
K9s with `-l debug`
(`--logLevel debug`) when the info
level is too terse, and expect the
file to grow quickly.

## "It opens, but everything says n/a"

The header shows `n/a` for context,
cluster, user and K8s revision, and
you are looking at the contexts view.
**The cluster is not answering.**
Verified end to end by the lab
(transcript t03 is the screen; t06
the log). Work the chain:

1. Is the address in the kubeconfig
   right, and does anything listen
   there? The log's version-fetch
   error names the cause —
   `connection refused` (nothing
   there), a timeout (a firewall or a
   sleeping network), or a name that
   does not resolve.
2. Is the cluster itself up? Check
   with whatever out-of-band means
   you have — the practice cluster's
   own console, `kubectl get nodes`
   from a machine that can reach it.
3. Pressing Enter on the context
   retries the connectivity check;
   K9s flashes the error at the
   screen's foot when it fails. There
   is nothing to fix inside K9s until
   one of the ends changes.

A cousin of this failure: the log
shows `x509: certificate signed by
unknown authority` (the lab hit this
exact line). Something *is* answering,
but its certificate is not signed by
the authority your kubeconfig
expects — a rebuilt cluster, a proxy
in between, or the wrong cluster at
that address altogether. The fix is
at the kubeconfig/certificate level,
and `--insecure-skip-tls-verify`
(it exists; it is in the flag list)
is a diagnostic, not a fix — it
turns off the check that is trying
to tell you something.

## "Forbidden"

Views come up empty, or actions
refuse, and the log (or a flash on
screen) says `forbidden` /
"user not authorized". This is
**RBAC**: your credentials are
accepted, but your role does not
include the thing K9s asked for.
The lab's log shows the mildest
species — `Namespace validation
failed ... user not authorized to
list all namespaces` — a warning, not
a failure: K9s fell back to the
context's own namespace. Full-strength
cases (cannot list pods anywhere)
mean the account is scoped elsewhere
or the role binding is missing.
Nothing in K9s can or should fix
this; the cluster's administrator
grants permissions, and chapter 13
explains why the limit is a feature.

## "No kubeconfig found" / it opens the wrong cluster

- K9s looks at `--kubeconfig`, then
  `$KUBECONFIG`, then
  `~/.kube/config`. If all three are
  empty it has nothing to read —
  create the file (your cluster's
  tooling usually writes it; the
  Kubernetes guide covers getting
  one) and start again.
- Wrong cluster on screen: a
  `:pod @other` from an earlier
  session of your attention, or the
  kubeconfig's current context is
  not the one you think. The header
  says which; `:ctx` changes it
  deliberately.
- K9s also tidies: it prunes
  per-context settings for clusters
  that have vanished from the
  kubeconfig (the `keepMissingClusters`
  setting exists for people who
  switch kubeconfigs often and want
  the old settings kept).

## The screen looks wrong

- **Boxes and icons are mangled.**
  The terminal's font lacks the
  glyphs. Set `noIcons: true` in the
  UI settings (chapter 11), or
  choose a plainer skin; the
  information is unchanged.
- **Colours are alarming on a
  healthy cluster.** The thresholds
  (70/90 by default, transcript t07)
  may not match your cluster's
  normal. Chapter 11: tune them.
- **A view seems frozen.** Check
  `liveViewAutoRefresh` — off by
  default in the generated config —
  and press Ctrl-R; check, too, that
  a `/` filter is not hiding the new
  rows (chapter 6's blindfold), and
  that the log is not reporting lost
  connections and retries
  (`maxConnRetry` governs how hard
  K9s tries before telling you).
- **`k9s info` ends with an ERROR
  about the config file.** On a
  fresh machine that means "not
  created yet" (chapter 3). If it
  persists after real runs, the
  config folder is not writable —
  the paths in `k9s info` are the
  ones to inspect.

## When in doubt, shrink the world

The sequence that resolves most
mysteries: quit K9s; reach the
cluster with kubectl (the
neighbour that prints once and
exits); fix *that* conversation
first; then start K9s with an
explicit `--kubeconfig` and
`--context`, in a terminal you
trust, and read the fresh lines at
the end of `k9s.log`. K9s adds a
screen to a conversation kubectl
has plainly; when the plain
conversation works, the screen
almost always follows.
