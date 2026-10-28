# The Moving Parts

K9s has fewer parts than it first
appears. This chapter names them, so
that every later surprise can be filed
under a part you already know: the
kubeconfig it borrows, the connection
it keeps open, the views, the two modes
that steer them, the context and
namespace you are standing in, and the
files where K9s keeps its own settings.

## The kubeconfig: borrowed keys

K9s starts by reading a **kubeconfig**
file: by default `~/.kube/config`, or
the file named by the `KUBECONFIG`
environment variable, or the one given
with `--kubeconfig`. Inside are
**clusters** (an API server address and
its certificate), **users** (your
credentials: a token, a certificate, or
a helper program), and **contexts**,
which bind one cluster to one user and
a default namespace. kubectl reads the
same file, which is the point: if
kubectl can talk to a cluster, K9s
almost always can, with the same reach.
Chapter 13 is frank about what that
reach means.

## The connection: a watcher, not a poller of printouts

Once connected, K9s does not run
kubectl behind the scenes; it speaks to
the API server itself, lists the
resources in view, and **watches** them
— the API server tells it about changes
as they happen. The screen's refresh
(the default is every 2 seconds, a
setting the lab verified in the
generated configuration: `refreshRate: 2`)
keeps derived figures and timers
current. This is why K9s feels alive
where kubectl feels like photography —
and why it needs a real, continuing
connection. No connection, no picture:
chapter 4's unreachable-cluster screen
is what the tool looks like with the
watcher starved.

Two figures in the header — cluster CPU
and memory — come from a helper called
the **metrics server**, which not every
cluster runs. The lab's log shows K9s
noting its absence and carrying on
(`Fail to locate metrics-server`);
without it, some columns and the header
percentages simply stay empty.

## Views: one resource type at a time

The screen almost always shows a
**view**: a table of one kind of
resource — pods, deployments, services,
nodes — one row each, with the columns
that matter for that kind (name,
namespace, status, restarts, age, and
for workloads, CPU and memory when the
metrics server provides them). Some
views are not API resources at all:
**contexts**, **namespaces**,
**aliases**, **pulses** (a rollup of
cluster health), **xray** (a tree of a
resource and its relations), and
**port-forwards**. Every view is
reached the same way, through command
mode, and left with Esc, which walks
back through the **breadcrumbs** — the
trail of views along the bottom of the
screen, also captured in the lab's
transcripts.

## Command mode and filter mode

Two keys steer everything:

- **`:` — command mode.** Type the name
  of what you want to see — `pods`,
  `deploy`, `svc`, a short name, or an
  alias — and press Enter. The command
  line at the top accepts more than a
  name: a namespace (`:pod ns-x`), a
  filter (`:pod /fred`), a label
  selector (`:pod app=fred`), or a
  context switch (`:pod @other`). This
  is the program's own documented
  grammar, and chapter 5 teaches it
  piece by piece.
- **`/` — filter mode.** Narrows the
  view already on screen by matching
  text against the rows. It understands
  regular expressions, an inverted form
  (`/! text`), a label form (`/-l
  app=fred`), and a fuzzy form (`/-f
  text`). Chapter 6 is devoted to it.

There is also a single-key vocabulary
for the row under the cursor — `d`
describe, `y` YAML, `l` logs, `e` edit —
which the screen lists down its right
edge, changing with the view. And `?`
anywhere shows the help screen: the
lab captured it cluster-free, and
`k9s-lab/transcripts/t05-help-screen.txt`
is genuine.

## Contexts and namespaces: where you are standing

The header's left block always answers
four questions: **Context** (which
named connection), **Cluster**, **User**
(whose credentials), and the K9s and
Kubernetes revisions, with **CPU/MEM**
once the metrics server reports. Two
more letters matter: the header shows
`[RW]` for read-write or `[RO]` for
read-only — the lab's unreachable
screen shows `[RW]`, K9s' default
posture. Inside a context, a
**namespace** scopes most views; the
namespace view (`:ns`) moves you
between them, and chapter 5 covers
switching in full.
Nearly every accident story about K9s
and kubectl begins with standing in the
wrong context or namespace, which is
why the header never hides it.

## K9s' own files

K9s keeps its settings in a `k9s`
folder whose location follows the XDG
convention on Linux (`~/.config/k9s`),
and `k9s info` prints every path it
uses — the lab ran it, and
`k9s-lab/transcripts/t02-info.txt` is
the real output. The cast:

- `config.yaml` — the main settings:
  refresh rate, read-only, the logger,
  UI toggles, CPU/memory colour
  thresholds. **Created for you on
  first run**; the lab watched it happen
  (`Saving K9s config to disk` in the
  log) and kept the generated file as
  `k9s-lab/transcripts/t07-default-config.yaml`.
- `aliases.yaml` — your own short names
  for commands; created on first run
  with a starter set (`dp` for
  deployments, `sec` for secrets, `jo`
  for jobs — the lab read these back
  from its own generated file).
- `hotkeys.yaml`, `plugins.yaml`,
  `views.yaml`, `jumps.yaml` — custom
  keys, plugin definitions, custom
  columns, and jumps between related
  resources; present when you make
  them, covered in chapter 11.
- A per-context folder (under the XDG
  data directory, `clusters/<cluster>/<context>/`)
  holding settings and benchmarks for
  that one context — the lab's
  unreachable run created one for its
  invented context, which is how we
  know the machinery runs before any
  cluster answers.
- The **log**: `k9s.log` in the state
  directory. When the screen will not
  tell you what is wrong, the log
  will — chapter 14 lives there.

## Check

Run `k9s info` on your own machine
after chapter 3's install. Its list
should name a config folder you can
actually find on disk, and if
`config.yaml` does not exist yet, it
will after your first real start —
that is K9s writing its defaults, not
something going wrong.
