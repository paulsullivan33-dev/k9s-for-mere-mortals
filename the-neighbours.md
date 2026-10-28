# The Neighbours

K9s is not the only seat in front of
a cluster. Each neighbour below is
characterised, not benchmarked, and
each wins somewhere K9s does not —
choosing is knowing where.

## kubectl — the ground floor

The official command-line tool, and
the one K9s quietly depends on
conceptually: same kubeconfig, same
API server, same permissions model.
kubectl wins at:

- **Scripting.** It prints and
  exits; its output pipes, greps and
  feeds other programs. K9s is a
  room you sit in; kubectl is a
  vending machine a script can use.
  Anything automated — CI, cron,
  reports — is kubectl country
  (or the API directly).
- **Applying state.** Files to
  cluster is `kubectl apply`; K9s
  has no equivalent workflow, by
  design.
- **Existence proofs.** When K9s
  and a colleague disagree about
  the cluster, `kubectl get` is the
  arbiter — one question, one
  printed answer, nothing cached or
  coloured between you and it.

Most operators end up bilingual:
kubectl for the scripted and the
solemn, K9s for the live and the
exploratory. This guide's lab used
kubectl not at all — the cluster it
couldn't reach didn't care which
tool knocked.

## The Kubernetes Dashboard

The official **web UI**: a browser
page served from inside the
cluster, showing resources and
allowing the common edits. It wins
for occasional users (no terminal
fluency needed), for showing a
cluster to someone across a table,
and where installing local tools is
not allowed. It loses on speed,
keyboard flow, and history: it is
itself a cluster workload to
install, secure and keep updated,
and its access token story has
tripped up many a first install.
K9s lives on *your* machine and
asks only for your kubeconfig.

## Lens and OpenLens

**Lens** is a desktop application —
a full graphical IDE for clusters,
with multiple clusters in a sidebar,
built-in metrics views, and an
extension ecosystem. It wins for
people who think in windows and
want several clusters' shapes at a
glance. It is heavier by orders of
magnitude than a terminal program,
and parts of its feature set are
commercial. Its community sibling
**OpenLens** keeps the open core.
If K9s is a bicycle, Lens is a car:
more comfort, more machine.

## The other terminals

K9s is not alone in the terminal
either. **kubeshell**, **kubetui**
and similar tools each take a slice
— a shell with cluster awareness, a
single-purpose viewer — and the
honest summary is that K9s is the
most complete of them, with the
aliases, plugins and skins the
others mostly lack. **stern** (and
its fork **kail**) deserve a named
mention for one job: tailing logs
across *many pods at once*, matched
by label, coloured per pod — the
multi-pod log river that chapter 8
noted K9s does not attempt.

## Popeye — the sibling

Same author, different question:
**Popeye** scans a cluster for
misconfigurations and reports them
(chapter 10 met it inside K9s as
`:popeye`). Standalone, it runs as a
one-shot report — CI-friendly in a
way a terminal session is not.

## Monitoring — a different trade

Chapter 1 said it and it bears
repeating at the border:
**Prometheus and Grafana** (the
series has a guide) keep history,
draw trends, and alert while you
sleep. K9s shows now. A cluster
watched only through K9s is a
cluster whose last Tuesday is a
rumour; a cluster watched only
through dashboards is one whose
present tense you feel at one remove.
The tools are complements wearing
similar colours.
