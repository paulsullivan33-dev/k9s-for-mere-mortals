# Navigating Views

Outcome: you can get to any resource
in the cluster, and back, from the
keyboard alone — and you have the one
screen that lists every destination:
the aliases view, which this guide's
lab captured for real.

Everything from here needs a reachable
cluster, so this chapter and chapters
6–10 are **documented-only**: taught
from K9s' own documentation and the
references the program itself prints
(the lab verified those: the aliases
view, the help screen, and the flag
list are real captures). The lab's
honesty ledger is in `criteria.md`.

## The colon is the steering wheel

Press `:` and type a resource name,
then Enter:

- `:pods`, `:pod`, `:po` — the same
  view; K9s accepts the plural, the
  singular and the **short name**, as
  kubectl does.
- `:deploy` (deployments), `:svc`
  (services), `:nodes`, `:events`,
  `:jobs`, `:cm` (config maps), `:sec`
  is taken by an alias for secrets —
  see below.
- Any resource the API server knows,
  including custom resources installed
  by operators, can be named this way;
  K9s discovers them from the cluster
  itself.

The command line accepts a small
grammar in one breath — the project's
own examples:

- `:pod ns-x` — pods, but in namespace
  `ns-x`.
- `:pod /fred` — pods, pre-filtered to
  rows matching `fred` (chapter 6).
- `:pod app=fred,env=dev` — pods
  carrying those labels.
- `:pod @ctx1` — pods on context
  `ctx1`, switching your context to do
  it. Powerful and easy to forget you
  did; the header is the witness.

## Aliases: the map of everywhere

An **alias** is a short name for a
command. K9s ships with some, reads
more from your `aliases.yaml`
(chapter 2 listed the starter set the
lab's first run generated: `dp` for
deployments, `jo` for jobs, `cr` and
`crb` for cluster roles and bindings,
`ro`/`rb` for roles and bindings, `np`
for network policies, `sec` for
secrets), and lets you add your own
(chapter 11).

The full map is a view of its own:
`:aliases` (or Ctrl-A). The lab opened
it with no cluster at all — proof it
is a local reference, not a cluster
query — and transcript t04 keeps the
screen. Twenty-three built-in commands
were listed, including some worth
knowing by name:

- `ctx` — contexts; `ns` does not
  appear because namespaces are the
  `:ns` command's own name.
- `pu` / `pulses` — the pulses view.
- `x` / `xray` — the xray view.
- `pf` / `portforwards` — your active
  port forwards (chapter 10).
- `sd` / `screendumps` — tables you
  saved to files (Ctrl-S saves one).
- `wk` / `workloads` — a combined
  workloads view.
- `hm` / `helm` — Helm releases.
- `Q`, `q`, `quit` — how to leave.

Check: open `:aliases` on your own
install and compare with transcript
t04; on a connected machine the same
view also lists every resource type
your cluster offers, which is the real
map — the lab's copy shows only the
built-ins, because its "cluster" was
a polite fiction.

## Moving within a view

The help screen (press `?`; transcript
t05 is the lab's real capture) lists
the movement keys, and they are the
usual suspects: `j`/`k` or arrows down
and up, `g` to the top, Shift-G to the
bottom, Ctrl-F/Ctrl-B a page at a
time, `h`/`l` left and right across
columns. Sorting hangs on Shift keys:
Shift-N by name, Shift-A by age,
Shift-S by status, Shift-P by
namespace (in all-namespaces views),
and Shift-O sorts by whichever column
is selected — Shift-Left and
Shift-Right move the selection between
columns. Ctrl-W toggles the wide set
of columns, Ctrl-Z toggles whether
faulty rows are shown, and Ctrl-R
refreshes the view on demand.

Space marks a row; Ctrl-Space marks a
range; Ctrl-\ clears the marks. Marked
rows are how you act on several
resources at once in chapter 9 — and
Ctrl-S saves the table to a file (find
them again with `:screendumps`).

## Moving between places

Three habits keep you oriented:

- **Esc is back.** Every view you open
  stacks onto the breadcrumbs along
  the bottom (the lab's transcripts
  show them growing: `<contexts>`,
  then `<contexts> <aliases>`, then
  `<contexts> <aliases> <help>`).
  Esc pops one off. Ctrl-G hides or
  shows the trail.
- **`-` is "last command".** Like
  `cd -` in a shell, it toggles you
  back to the view you last commanded;
  `[` and `]` walk further through the
  command history.
- **The header is home truth.** Context,
  cluster, user, namespace posture —
  if the rows look wrong, read the
  header before the rows. Most "K9s is
  showing me someone else's pods"
  moments are a context switched by
  `:pod @other` and forgotten.

Namespaces deserve one line of their
own: `:ns` lists them, Enter on one
makes it yours (`u` on the namespace
view does the same), and views then
show that namespace until you switch
again. `-A` at launch, or choosing
"all" in the namespace view, widens
back out. Namespaces you live in can
be pinned as favourites from that
view, after which the number keys hop
between them, with `0` returning you
to all namespaces.
