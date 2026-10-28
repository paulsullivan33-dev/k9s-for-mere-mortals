# K9s for Mere Mortals

A Kubernetes cluster is always moving:
pods start, fail, restart; deployments
roll out; services appear and vanish.
The standard tool, **kubectl**, answers
one question at a time and then stops —
you ask, it prints, it exits. **K9s**
takes the other approach. It stays
running in your terminal and keeps
asking on your behalf, redrawing the
answer every couple of seconds, so you
watch the cluster the way you would
watch a departures board. On top of the
watching it puts the everyday actions —
read a pod's logs, describe a resource,
look at its YAML, restart a deployment,
delete the thing that is stuck — each a
keystroke or two away.

This guide explains how K9s is put
together, how to install it and start
it, what its screen is telling you, how
to navigate, filter and search a view,
how to look inside a resource and read
its logs, how to act on resources (and
how to keep that power on a short
leash), and how to make the tool your
own with skins, hotkeys, aliases and
plugins. The limits are stated plainly,
including the big one: K9s acts with
the full power of the credentials in
your kubeconfig file, and a delete is
two keystrokes away.

The rule that governs everything here
is stated in chapter 1 and repeated in
chapter 13: **your clusters, or
clusters you have explicit permission
to administer.** The lab obeyed it
strictly: the only credentials it ever
loaded were invented for the occasion
and pointed at an address where nothing
listens.

The lab behind this guide ran the real
program: **K9s v0.51.0**, the official
release binary from the project's
GitHub page, on a small Linux machine.
Against it the lab checked the version
and help output, asked `k9s info` where
its configuration lives, watched a
first run create its default
configuration files, and started it
against a kubeconfig pointing at an
unreachable cluster — capturing the
screen it lands on, the aliases view,
and the help screen, all of which work
without a cluster, and the exact errors
its log records when the connection
fails. What the lab could not do, it
did not fake. Its machine cannot run
containers with networking, so no
disposable practice cluster could be
built on it (the attempts are recorded,
with their errors, in the lab
transcripts), and the series'
Kubernetes guide was likewise verified
client-side only. So everything that
needs a living cluster — the resource
views, logs, editing, deleting, port
forwarding — is taught from the
project's own documentation and from
what the program itself prints, and is
labelled documented-only wherever it
appears. `criteria.md` keeps the full
ledger of what was run and what was
only read.

## What this guide covers

- [**What K9s is**](what-k9s-is.md) —
  the mental model: a live window onto
  the API server, the credentials it
  borrows, and the rule that comes with
  borrowed power
- [**The moving parts**](the-moving-parts.md) —
  views, the command line inside the
  tool, filters, contexts and
  namespaces, and the files K9s keeps
- [**Installing K9s**](installing-k9s.md) —
  the release binary and the package
  managers, and proving the install with
  `k9s version` and `k9s info`
- [**First run**](first-run.md) —
  starting K9s, what happens when the
  cluster cannot be reached, and reading
  the header it shows you
- [**Navigating views**](navigating-views.md) —
  the colon commands, aliases, sorting,
  and moving between resources, contexts
  and namespaces
- [**Filtering and searching**](filtering-and-searching.md) —
  the slash filter: plain text, regular
  expressions, inverted and fuzzy
  matches, and filtering by label
- [**Looking inside**](looking-inside.md) —
  describe, YAML, edit: the three ways
  K9s shows you one resource up close
- [**Logs**](logs.md) — reading a pod's
  logs without leaving the terminal,
  previous logs, and following along
- [**Acting on resources**](acting-on-resources.md) —
  delete, kill, scale, restart, drain:
  the powerful keys, their confirmations,
  and the read-only leash
- [**Port forwards and extras**](port-forwards-and-extras.md) —
  port forwarding, benchmarks, the
  pulses and xray views, and plugins
- [**Making it yours**](making-it-yours.md) —
  the configuration files, skins,
  hotkeys, aliases and custom views
- [**The neighbours**](the-neighbours.md) —
  kubectl itself, the Kubernetes
  Dashboard, Lens, and the other
  terminals: what each does that K9s
  doesn't, and vice versa
- [**Limits**](limits.md) — no cluster,
  no picture; borrowed credentials cut
  both ways; a screen that keeps no
  record of what you did
- [**Troubleshooting**](troubleshooting.md) —
  the unreachable cluster, the forbidden
  error, the missing kubeconfig, and the
  log file that explains it all
- [**Glossary**](glossary.md) — the
  terms, collected in one place

## What it doesn't cover

Kubernetes itself — what a pod is, why
deployments exist, how services route
traffic. That is the companion
Kubernetes guide's job; this one starts
where it ends, with a cluster you can
already reach. Running a cluster,
choosing one, securing one, and
multi-cluster fleets run by a whole
team are likewise pointed at, not
taught. And pointing K9s at a cluster
you are not authorised to administer —
or using its delete key there — is not
a harder chapter of this book; chapter
13 explains the line.

## How to use this guide

Read chapter 1 for the model and the
rule, and chapter 2 before installing
anything — knowing which part holds
what turns later confusion into a named
problem. Chapters 3 and 4 get you from
nothing to a running K9s, including the
screen everyone meets first on a bad
day: the cluster that will not answer.
Chapters 5 to 8 are the daily driving:
navigate, filter, look inside, read
logs. Read chapter 9 slowly and in
full — it holds the keys that change
things, and the read-only setting that
tames them. Chapter 11 is where a
borrowed tool becomes your tool. The
lab's transcripts live in
`k9s-lab/transcripts/`; its notes are
`examples/lab-notes.txt` and its
distilled commands `examples/commands.txt`.

---

## Part of the series

This guide is one of the [For Mere Mortals](https://paulsullivan33-dev.github.io/mere-mortals/) guides: plain-language guides to the tools programmers and makers use.

Related guides:

- [Kubernetes for Mere Mortals](https://paulsullivan33-dev.github.io/kubernetes-for-mere-mortals/)
- [Docker for Mere Mortals](https://paulsullivan33-dev.github.io/docker-for-mere-mortals/)
- [Prometheus and Grafana for Mere Mortals](https://paulsullivan33-dev.github.io/prometheus-and-grafana-for-mere-mortals/)

## License

The writing in this guide is licensed under [CC BY 4.0](https://github.com/paulsullivan33-dev/k9s-for-mere-mortals/blob/main/LICENSE). Code examples and lab scripts are licensed under the [MIT License](https://github.com/paulsullivan33-dev/k9s-for-mere-mortals/blob/main/LICENSE-CODE).
