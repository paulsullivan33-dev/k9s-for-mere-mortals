# Looking Inside

Outcome: from one row in a table, you
can get the resource three ways —
the story (`d`), the exact words
(`y`), and the editable original
(`e`) — and you know what each is
for. Documented-only chapter: these
keys act on live cluster objects, and
the lab had none; the keys
themselves are verified — they are
printed on every view's right-edge
strip and in the project's reference.

## Describe: `d`

Select a row, press `d`, and K9s
shows the resource's **describe**
page: the same human-readable summary
`kubectl describe` produces, rendered
inside K9s and refreshed as you
watch. For a pod it is the page that
answers most first questions: which
node it landed on, which containers
and images it runs, its environment
and mounts in outline, its conditions
(ready? scheduled?), and — worth the
chapter alone — its **events**: the
cluster's own notes about the object,
newest last. "ImagePullBackOff" stops
being a status and becomes a sentence:
which image, which registry, what the
registry said back.

Describe is a summary, not the
object: fields the summariser does
not care about are not shown. When
the summary disagrees with your
expectations, go one level down.

## YAML: `y`

`y` shows the resource exactly as the
API server holds it, as YAML — every
field, including the ones the cluster
added itself: `metadata.managedFields`
(the verbose bookkeeping of who set
what) makes real clusters' YAML
longer than the file you applied.
Read it for ground truth: the actual
replicas, the actual image tag, the
labels and annotations as they are —
not as the file on your disk claims.

Inside the YAML view: `/` searches
the text, `f` toggles full screen,
Esc returns. Ctrl-S saves the view
to a file, as with tables (chapter 5's
screen dumps). The YAML is the object;
if you edit the object, this is the
text you will be editing — which is
the next key.

## Edit: `e`

`e` opens the resource's YAML in your
**editor** — the program named by the
`EDITOR` environment variable (set it
to one you are fluent in before you
need it at speed; K9s and kubectl both
honour it). Save and quit, and K9s
sends the changed object back to the
API server, which validates it:
well-formed but unacceptable changes
come back as an error naming the
field, and the resource stays as it
was. Malformed YAML is refused before
it leaves.

Editing live objects is the sharpest
of the three keys, and the guide's
rule applies at full strength:

- It bypasses every file, review and
  pipeline your team may have. The
  cluster now differs from its
  declared state, and nothing but
  memory says how. Prefer editing the
  source files and applying them; use
  `e` for the small, urgent, and
  understood.
- Some fields cannot be changed in
  place (a service's cluster IP, a
  pod's almost-everything — pods are
  edited by replacing them, usually
  via their deployment). The API
  server's error says which; that is
  the server teaching, not K9s
  failing.
- In read-only mode (chapter 9), `e`
  is not offered at all — one of the
  reasons read-only is the posture
  for clusters you may look at but
  not administer.

## The smaller keys around them

From the same right-edge strip:
`c` copies the resource's name and
`n` its namespace to the clipboard
(the lab's help references list copy
behaviour; chapter 11 covers the
clipboard settings); Shift-J **jumps
to the owner** of the selected
resource — from a pod to its
ReplicaSet or deployment — and
chapter 11's jumps.yaml lets you
define leaps of your own between
related kinds. In the namespace view,
`u` makes a namespace yours; on
config maps, secrets, service
accounts and persistent volume
claims, `u` instead shows what
*uses* the resource — the "UsedBy"
list, often the fastest answer to
"can I delete this?" (The cautious
answer is still "check twice",
chapter 9.)

## A worked habit

The order these keys get used in
practice, offered as a habit worth
stealing: **status first** (the row
itself), **describe second** (the
story and the events), **logs third**
(chapter 8), **YAML last** (the ground
truth when the story and the truth
seem to differ). Most mysteries
surrender at describe; arriving at
YAML too early is reading the
appendix before the chapter.
