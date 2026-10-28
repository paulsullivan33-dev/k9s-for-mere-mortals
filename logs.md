# Logs

Outcome: a misbehaving pod's own words
are two keystrokes away, you can read
the *previous* incarnation's words —
the ones a crash tries to take with
it — and you know the settings that
govern how much log you get.
Documented-only chapter: logs come
from live pods, and the lab had none;
every behaviour below is the
project's documented behaviour, and
the key names are on the verified
help references.

## The key: `l`

Put the cursor on a pod and press
`l`. The view becomes that pod's log:
lines stream in, newest at the
bottom, and the view **follows** —
as the program writes, the screen
scrolls. That is the whole gesture,
and it is the reason many people
install K9s: the
`kubectl logs -f -n ... ` command
they can never quite remember is a
single letter here, and the pod came
from a table they were already
looking at.

Inside the log view:

- **`p` — previous logs.** On a pod
  row this is the key that earns the
  chapter: the logs of the
  container's *previous* run. A pod
  in CrashLoopBackOff has usually
  already said why it dies, in the
  incarnation that just exited; the
  current one may only be a splash
  screen or silence. Read `p` first
  on any restarting pod.
- **`w`** toggles line wrap, **`t`**
  toggles timestamps on each line,
  **`f`** toggles full screen.
- **`/`** filters the log text, the
  same slash as chapter 6; `error`
  is the classic first filter.
- **Esc** returns to the pods; the
  stream stops when you leave.
- A multi-container pod asks which
  container, or shows them combined
  under the documented log view's
  container handling — check the
  right-edge strip, which lists what
  the current view accepts.

Logs also hang off other rows: the
project's reference keys `l` as
"resource specific", and containers
within a pod's describe page lead to
their own logs. The shape is the
same wherever you enter.

## How much log you get

K9s' logger is governed by settings
the lab *can* vouch for, because
they are in the default configuration
its first run generated (transcript
t07 — this part is verified):

```yaml
logger:
  tail: 100
  buffer: 5000
  sinceSeconds: -1
  textWrap: false
  disableAutoscroll: false
  columnLock: false
  showTime: false
```

Read them as: opening a log fetches
the last **100 lines** (`tail`); the
view holds at most **5000** lines in
its buffer — a chatty pod pushes old
lines out of the *view* (they remain
in the cluster until the node's own
rotation takes them, and kubectl can
still fetch beyond; chapter 13 counts
this among the limits); `sinceSeconds:
-1` means no time cutoff on the first
fetch. Chapter 11 shows how to change
them in `config.yaml`; a pod that
speaks in thousands of lines a minute
wants a bigger buffer or a smaller
tail, and either is a one-line edit.

## What logs cannot do

A pod that never started has no logs
(the events in chapter 7's describe
page carry that story). A pod deleted
takes its logs with it — Ctrl-S,
which saves views to files elsewhere
in K9s, is the habit that survives
deletion, and chapter 9's delete keys
are why the habit matters. And a log
is what the program chose to say:
silence in the log is a fact about
the program, not about K9s.

One courtesy rule, small but real:
logs can contain other people's data
on shared systems — request details,
addresses, the occasional secret a
developer printed in a panic. The
guide's rule covers logs too: read
them where you are authorised, save
them like secrets when you save them
at all.
