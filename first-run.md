# First Run

Outcome: K9s started, and — the part
most first-run chapters skip — you can
read the screen it shows when the
cluster does *not* answer, because some
day that is the first screen you will
get. This guide's lab met K9s in
exactly that position, on purpose, and
kept the evidence.

## Starting it

```bash
k9s
```

With no flags, K9s reads the default
kubeconfig (`~/.kube/config`, or the
`KUBECONFIG` variable), takes its
current context, opens a connection,
and lands on the pods view. The flags
worth knowing on day one (all verified
in the lab's captured `k9s --help`,
transcript t01):

- `--kubeconfig <file>` — use this
  kubeconfig, not the default. The
  safe way to practise: a separate file
  for a practice cluster.
- `--context <name>` — start in this
  context, whatever the file calls
  current.
- `--namespace <name>` / `-n` — start
  in this namespace; `-A` starts in
  all namespaces at once.
- `--command <view>` / `-c` — start on
  this view instead of pods
  (`--command deploy`, say).
- `--readonly` — start with every
  changing action disabled. Remember
  this one; chapter 9 leans on it.
- `--refresh <seconds>` / `-r` — how
  often the screen redraws its figures
  (default 2).

## The day the cluster does not answer

The lab's first run pointed K9s at an
invented context, `nowhere`: an API
server address of `https://127.0.0.1:6443`
where nothing in the lab listens, and a
token that is not real. Here is what
actually happened, and it is more
useful than a crash:

1. **K9s started anyway.** No stack
   trace, no exit. It drew its full
   screen — logo, header, table frame.
2. **It landed on the contexts view**,
   listing the one context the
   kubeconfig held, `nowhere(*)`,
   with its cluster, user and (empty)
   namespace.
3. **The header told the truth**: 
   `Context: n/a`, `Cluster: n/a`,
   `User: n/a`, `K8s Rev: n/a` — and
   `CPU`/`MEM` blank. The one thing it
   did know, `K9s Rev: v0.51.0`, it
   showed. It also showed `[RW]`: even
   with no cluster at all, K9s starts
   in read-write posture.
4. **Choosing the context failed
   politely.** Pressing Enter on
   `nowhere` ran a connectivity check,
   and when it failed K9s stayed on the
   contexts view and flashed the error
   at the bottom of the screen — in the
   lab's words from its log,
   `Context switch failed ... "no
   connection to cached dial"`.
5. **The log said everything.** The
   start-up lines in `k9s.log` record
   the whole attempt — the failed
   version fetch with the exact network
   error, the "Kubernetes connectivity
   toast", the config file being saved
   for the first time. Transcript t06
   keeps the relevant lines.

Check: compare the lab's screen,
`k9s-lab/transcripts/t03-contexts-unreachable.txt`,
with your own first run against a
stopped practice cluster — same shape:
contexts view, `n/a` header, one
patient program waiting for you to fix
the address, start the cluster, or pick
a context that works.

Two lessons to bank. First: **a K9s
that "opens fine but shows nothing" is
usually this screen** — it is not
broken, it is unconnected, and the
header's `n/a` fields are the tell.
Second: some failures *do* look
different — a kubeconfig that points at
a real server with the wrong
certificate produces a certificate
error in the log (the lab's first
connection attempt hit one, `x509:
certificate signed by unknown
authority`, because something in its
sandbox answered TLS on that port with
the wrong certificate before refusing
outright). Chapter 14 separates the
species.

## When it works: the pods screen

Against a reachable cluster (described
here from the program's documented
behaviour — the lab had none, and says
so), the same start lands on the pods
view of the current namespace: a table
of pods with name, ready count, status,
restarts, age and, with a metrics
server, CPU and memory per pod. The
header fills in: context, cluster, user,
both revisions, CPU/MEM percentages.
The right edge of the screen lists the
keys that act on a pod — this
context-sensitive strip is K9s' built-in
memory aid, and this guide will keep
pointing at it.

The splash screen K9s shows while
connecting can be switched off
(`--splashless`, or the `splashless`
setting in chapter 11); on slow or
distant clusters the wait is real, and
chapters of this guide will not pretend
otherwise.

## Leaving

`:q` and Enter, or Ctrl-C, quits; Esc
walks back a view at a time (watch the
breadcrumbs at the bottom change). `-`
returns to the last-used command, `[`
and `]` walk the command history — all
three verified on the lab's captured
help screen (transcript t05). If
Ctrl-C ever seems not to quit, someone
has set `noExitOnCtrlC` (chapter 11);
`:q` still works.
