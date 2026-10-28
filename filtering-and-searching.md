# Filtering and Searching

Outcome: faced with four hundred pods,
you can cut the screen down to the
three you care about — and know which
kind of filter you used, because K9s
has four and they are not
interchangeable. Documented-only
chapter: the filter grammar below is
the project's own reference (and the
shape of it is visible in chapter 5's
verified command-mode examples); the
lab had no cluster to filter.

## The slash

Press `/` in any view and type. The
list narrows as you type; Enter keeps
the filter, Esc clears back out of
filter mode. By default the text is a
**regular expression** matched against
the visible row — names mostly, but
the whole row's text is fair game:

- `/web` — rows mentioning web.
- `/fred|blee` — rows mentioning
  either; the `|` is the regular
  expression "or".
- `/^api-` — rows starting with api-;
  the anchors of ordinary regex work.

A filter stays attached to its view
while you watch it — new pods arriving
that match appear, ones that stop
matching leave. That makes a filter a
standing question, not a one-off
search: park `/error|failed` thinking
on a status column and the screen
becomes a small alarm panel (chapter
13 will remind you it alerts nobody
but you, and only while you watch).

## Invert it: `/!`

`/` followed by `!` keeps everything
that does *not* match:

- `/! kube-system` — everything except
  the cluster's own plumbing.
- `/! running` — the not-healthy set,
  in views whose rows carry status
  text.

Inverting is the fastest route to a
worklist: a healthy cluster is mostly
rows you do not need; the complement
is the shift report.

## Filter by label: `/-l`

Labels are the cluster's own filing
system — key=value pairs the
deployments themselves declare — and
filtering by them asks the *server*,
not the screen, for the subset:

- `/-l app=web` — pods labelled
  app=web.
- `/-l app=web,tier=frontend` — both
  conditions at once.

The difference matters: a text filter
can only narrow what is on screen,
while a label selector asks the API
server for exactly the labelled set —
across all namespaces if the view is
in all-namespaces mode, and including
rows a text filter would never see.
The same selectors work in command
mode, as chapter 5 showed
(`:pod app=fred,env=dev`).

## Fuzzy: `/-f`

`/-f` switches the match to **fuzzy**:
the characters of your text must
appear in order, not adjacently:

- `/-f apipod` finds `api-pod-7d9f`
  and `api__pod__canary` alike — the
  letters are all there, in order,
  with other characters in between.

Fuzzy is for when you remember the
shape of a name but not its spelling —
a common state at 2 a.m. It is the
loosest of the four filters and the
one most likely to include surprises;
check the row count before acting on
a fuzzy-filtered view in chapter 9's
territory.

## Which filter when

- **Text/regex** for names and
  statuses you can see.
- **Inverted** for "everything except
  the healthy/expected".
- **Label** for "the set the cluster
  itself says belongs together" — and
  whenever the answer might live
  outside the current screenful.
- **Fuzzy** for half-remembered names.

And a caution that belongs to this
chapter because filters hide things:
**a filter is a blindfold as well as
a lens.** The title of a filtered
view shows the filter; believe it.
Most "the cluster deleted my pod"
alarms are a forgotten `/` still
active on the view. Esc clears the
filter; the pod was never gone.
