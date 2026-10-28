# Port Forwards and Extras

Outcome: you can reach a service that
lives inside the cluster as if it were
on your own machine, try a service
under load, read the cluster's health
at a glance, and extend K9s with
plugins — the features past the daily
drive. All documented-only, from the
project's own documentation and its
in-program references (the aliases
view that names these views is a real
lab capture, transcript t04).

## Port forwarding: Shift-F

A service inside a cluster often has
no address on your network at all.
**Port forwarding** borrows one: the
API server tunnels a port on your
machine to a port on a pod. In K9s,
select a pod or service and press
**Shift-F**; K9s asks for the local
and container ports, sets the forward
up, and keeps it while K9s runs. Then
`localhost:8080` (or whichever local
port you chose) reaches the thing
itself — a database console, an admin
page, an API that was never meant to
be public.

- **`f`** on a pod, service or
  container shows its existing
  forwards; the `:portforwards` view
  (`pf` in the aliases list) shows
  them all in one place.
- Forwards die with the connection or
  with K9s; they are a visit, not a
  publication. If your team needs the
  service reachable for real, that is
  a service-type or ingress decision
  for the cluster's owners — and,
  given what may sit behind an admin
  page, a decision to make slowly.
- The default local address is
  `localhost` (the lab's generated
  config carries `portForwardAddress:
  localhost` — verified, transcript
  t07). Binding a forward to anything
  wider shares the service with your
  network; know that you did it.

## Benchmarks: `b`

On a service (or a port forward),
**`b`** starts a load benchmark
against it — K9s runs an HTTP
benchmarker and reports timings in
the benchmarks view. Two cautions,
both about aim: a benchmark is a
small denial-of-service you perform
on purpose, so it belongs on your
own services in clusters that are
yours (the rule again); and the
numbers measure the path through the
tunnel as much as the service, so
read them as a smoke test, not a
benchmark suite.

## Pulses and xray

- **`:pulse`** (aliases `pu`, `hz`)
  opens the pulses view: the cluster
  summarised as health — counts of
  resources by state, the sick
  highlighted. It is the morning
  glance: green page, coffee; red
  rows, chapter 7's habit.
- **`:xray <resource> [namespace]`**
  draws a **tree**: the resource and
  its relations — a service with its
  endpoints and the pods behind them,
  a deployment with its replica sets
  and pods. Where the table answers
  "what state is it in", xray answers
  "what is it connected to", which is
  the question behind half of all
  "the service is up but nothing
  works" reports.

## Popeye

**`:popeye`** runs the project's
sibling tool, Popeye, inside K9s: a
sanitiser that scans the cluster for
common misconfigurations — resources
with no limits, images tagged
`latest`, probes missing — and
reports them as a spinach-flavoured
list of warnings. It reads; it does
not fix. Treat its list as a review
agenda, not a verdict: some warnings
are real risks, some are taste, and
the difference is your context.

## Plugins

K9s can be extended with **plugins**:
small definitions (in `plugins.yaml`,
whose path `k9s info` prints — the
lab's transcript t02 lists it) that
bind a key in a given view to a
command of yours, run against the
selected resource, whose name,
namespace and kind are passed to the
command as arguments and environment
variables. The project ships example
plugins — opening a resource in other
tools, diving into a node, removing
finalisers — and the pattern generalises
to "the thing I always end up doing
to a pod". Chapter 11 covers writing
them. Two cautions: a plugin runs
with your shell's full power (vet
what you install, as with any code),
and a plugin's key only exists in
the view it was defined for — a
missing key is usually a plugin
scoped elsewhere, not a broken
install.
