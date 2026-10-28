# What K9s Is

A **Kubernetes cluster** is a group of
machines run as one: you describe what
should be running — so many copies of
this program, reachable at that name —
and the cluster's control centre, the
**API server**, works to make it true.
Everything you can ask or change goes
through that API server. The standard
command-line tool, kubectl, is a way of
asking it questions one at a time:
*show me the pods* — a table prints,
the command exits, and the answer is
already ageing.

**K9s is a terminal program that stays
connected and keeps the answer fresh.**
It opens one full-screen view of the
cluster — pods by default — and redraws
it every couple of seconds as things
change (the default refresh rate is 2
seconds; the lab's captured default
configuration says so, and chapter 11
shows where). A pod that crashes turns
red in front of you. A rollout moves
through its stages while you watch. And
because watching is only half the job,
K9s hangs the everyday actions on
single keys: `l` for a pod's logs, `d`
to describe it, `y` for its YAML, `e`
to edit it, and — the keys chapter 9
treats with respect — Ctrl-D to delete
and Ctrl-K to kill.

The name is a pun, honestly labelled:
Kubernetes shortened to "K8s" (K, eight
letters, s), so a livelier companion
becomes K9s, the dog. It is a
third-party open-source project, not
part of Kubernetes itself.

## Why anyone would do this

Three honest reasons account for most
of K9s' following:

- **Watching a change land.** You apply
  a new version and want to see the old
  pods terminate and the new ones come
  up — or catch the one that doesn't.
  With kubectl this is a loop of you
  re-running a command. With K9s it is
  one screen, already moving.
- **Chasing a failure.** A pod is in
  CrashLoopBackOff. The useful evidence
  — its logs, its events, its YAML — is
  scattered across four kubectl
  commands. In K9s each is a keystroke
  from the row that is already red.
- **Knowing the terrain.** New to a
  cluster, the fastest way to learn
  what is in it is to browse: namespaces
  across the top, resources a colon
  command away, everything one Esc back.
  K9s is a map you can also drive.

## The rule, stated before everything else

K9s has no accounts and no passwords of
its own. It reads your **kubeconfig**
file — by default `~/.kube/config`, the
same file kubectl uses — and presents
the credentials it finds there. Whatever
those credentials may do, K9s may do,
at keyboard speed, including deleting
resources cluster-wide if your account
allows it. So this guide has one rule,
and it is not fine print:

> **Point K9s only at clusters that are
> yours — or where you have explicit
> permission to administer what you can
> reach.**

Browsing your own practice cluster: yes.
The cluster at work, in the role your
job grants you: yes, and chapter 9's
read-only mode is still worth your
consideration there. A client's cluster
"just to look", a borrowed kubeconfig,
a production system you can view but
must not touch, with K9s in its default
write mode: no. Looking is not harmless
when the same screen deletes on two
keystrokes, and an accident is not
excused by the tool being fast. The lab
in this guide went further than most
readers will need to: its only
kubeconfig was invented for the
occasion, held a token that is not a
real token, and pointed at an address
where nothing listens.

If a future idea you have for this tool
does not fit inside the rule, the idea
is the problem. Chapter 13 returns to
this, and chapter 9 shows the leash —
read-only mode — that makes looking
safe when looking is all you should do.

## A familiar shape

K9s is one program with one face: the
terminal screen. Inside that screen it
behaves like a small world of its own
(chapter 2): **views** that list
resources, a **command mode** opened
with `:` where you name what you want
to see, a **filter mode** opened with
`/` that narrows the list in front of
you, and context-sensitive keys whose
meanings the screen itself displays
along its right edge. It is closer in
spirit to a text editor you live in
than to a command you run.

A first taste, after installing
(chapter 3):

```bash
k9s --kubeconfig ~/practice-kubeconfig
```

Run against the lab's invented
kubeconfig, that command does not fail
and walk away. K9s starts, finds it
cannot reach the cluster, and lands on
the list of **contexts** — the named
connections your kubeconfig holds —
with the header honestly showing `n/a`
for cluster, user and versions. Check:
the lab's transcript
`k9s-lab/transcripts/t03-contexts-unreachable.txt`
shows exactly that screen, context
`nowhere` and all. Chapter 4 walks
through it.

## Who it is for

The cluster operator whose kubectl
commands have become a choreography
they repeat fifty times a day. The
developer given namespace access and a
bug that only shows itself live. The
learner who understands diagrams better
once the rows move. The common thread
is one person, a terminal, and a
cluster they are allowed to touch.

## What it is not

K9s is not Kubernetes: it installs no
cluster, runs no workloads, and fixes
nothing by itself (the Kubernetes guide
in this series covers the cluster
itself). It is not a replacement for
kubectl — it is a seat in front of the
same API server, and chapter 12 explains
where kubectl still wins, scripting
above all. It is not a monitoring
system: it keeps no history, draws no
graphs of last week, and alerts nobody
at 3 a.m.; that is Prometheus'
country, and the header's CPU and
memory figures are a snapshot, not a
record. And it is not read-only by
default, whatever its calm screen
suggests — chapter 9 exists because of
exactly that.
