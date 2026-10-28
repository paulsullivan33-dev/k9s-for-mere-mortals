# Glossary

**Alias** — a short name for a
command: built-ins (listed by the
`:aliases` view), a starter set K9s
generates in `aliases.yaml`, and
your own additions (chapters 5, 11).

**API server** — the cluster's
control centre; every question and
change goes through it, and K9s is
in constant conversation with it
(chapter 2).

**Benchmark** — the load test K9s
can run against a service with `b`
(chapter 10).

**Breadcrumb** — the trail of views
along the bottom of the screen; Esc
walks back along it (chapter 5).

**Cluster** — in a kubeconfig, one
named entry holding an API server's
address and certificate (chapter 2).

**Command mode** — the `:` line at
the top of the screen where views
are named, with its small grammar of
namespaces, filters, labels and
`@context` (chapters 2, 5).

**Context** — a kubeconfig entry
binding a cluster, a user and a
default namespace; "where you are
standing" (chapters 2, 4).

**Cordon** — marking a node
unschedulable so no new pods land
on it; `u` in the nodes view
(chapter 9).

**Custom resource** — a resource
type added to a cluster by an
operator or administrator; K9s
discovers and lists them like the
built-in kinds (chapter 5).

**Describe** — the human-readable
summary of one resource, events
included; the `d` key (chapter 7).

**Drain** — cordon plus evicting
the pods that can move, the
maintenance gesture; on nodes
(chapter 9).

**Filter mode** — the `/` line that
narrows the current view: plain
regex, inverted (`/!`), by label
(`/-l`), or fuzzy (`/-f`)
(chapter 6).

**Hotkey** — a key you bind
yourself, in `hotkeys.yaml`, to a
command-mode string (chapter 11).

**Jump** — a defined leap between
related resources, in `jumps.yaml`;
Shift-J to the owner is the
built-in one (chapters 7, 11).

**Kill** — Ctrl-K: deletion with
no confirmation and no grace period
(chapter 9).

**Kubeconfig** — the file (`~/.kube/config`
by default) holding clusters, users
and contexts; kubectl and K9s read
the same one (chapter 2).

**Label selector** — a filter on
the cluster's own key=value labels,
asked of the API server itself
(chapters 5, 6).

**Metrics server** — the optional
cluster add-on that supplies CPU
and memory figures; without it,
K9s' numbers stay blank
(chapters 2, 13).

**Namespace** — a named partition
of a cluster most resources live
in; `:ns` moves between them
(chapter 5).

**Plugin** — a stanza in
`plugins.yaml` binding a key in a
view to a command of yours, run
against the selected resource
(chapters 10, 11).

**Popeye** — the project's sibling
sanitiser; `:popeye` runs its scan
inside K9s (chapter 10).

**Port forward** — a tunnel through
the API server making a cluster
port appear on your machine;
Shift-F sets one up (chapter 10).

**Pulse** — the `:pulse` view, a
one-screen health rollup of the
cluster (chapter 10).

**RBAC** — role-based access
control: the cluster's permission
system. K9s can only ever do and
see what your role allows
(chapters 9, 13, 14).

**Read-only mode** — the posture
that disables every changing
action: `--readonly`, or
`readOnly: true`, per context if
you like. The header shows `[RO]`
(chapter 9).

**Screen dump** — a table or view
saved to a file with Ctrl-S,
revisited with `:screendumps`
(chapter 5).

**Skin** — a YAML file of colours
for the whole interface, chosen by
the `skin` setting (chapter 11).

**View** — one screen of K9s: a
table of one resource kind, or a
special screen like contexts,
aliases or help (chapter 2).

**Watch** — the API server
mechanism by which K9s learns of
changes as they happen rather than
re-asking (chapter 2).

**Xray** — the `:xray` view: a
resource drawn as a tree of its
relations (chapter 10).
