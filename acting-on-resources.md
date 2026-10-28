# Acting on Resources

Outcome: you know the keys that change
the cluster, exactly how much
confirmation each one asks for, and
the setting that takes them all away
when they are not yours to use. Read
this chapter before your first real
cluster, not after your first
accident. Documented-only: these keys
act on live resources; the lab pressed
none of them, and the confirmations
described are the project's documented
behaviour.

## The two deletes

K9s offers deletion at two speeds, and
the difference between them is the
chapter in miniature:

- **Ctrl-D — delete, with a
  confirmation.** K9s raises a dialog
  naming the resource; you Tab to the
  confirm and press Enter, or Esc and
  nothing happened. Deletion in
  Kubernetes is the ordinary kind: the
  API server removes the object, pods
  get their graceful termination
  period, and anything that owns the
  object (a deployment, say) may
  recreate it — deleting a pod that a
  deployment wants is a restart, which
  is often exactly why you pressed it.
- **Ctrl-K — kill, with none.** The
  project's own words for it, from its
  key reference: "no confirmation
  dialog, equivalent to
  `kubectl delete --now`". The grace
  period is forced to zero. It exists
  for the object that will not die
  politely, and it will not ask
  whether you meant it.

Marked rows (chapter 5's Space and
Ctrl-Space) multiply both keys: a
confirmed delete of twelve marked
pods is one dialog and twelve
consequences. Before confirming
anything plural, read the dialog's
list, not just its title.

## The other actors

- **`r` — restart**, on deployments,
  daemon sets and stateful sets: a
  rolling restart of the workload,
  the kubectl rollout restart
  equivalent. On nodes, `r` is drain
  (below); on cron jobs, `t`
  **triggers** a run now.
- **Scale**: on a deployment (and its
  kin), the scale action asks for a
  new replica count. Scaling to zero
  is the polite off-switch for a
  workload you may want back intact.
- **`u` — cordon/uncordon**, on
  nodes: cordon marks a node
  unschedulable (no new pods), the
  first move in maintenance.
- **Drain**, on nodes: cordon plus
  evicting the pods that can move,
  with the usual Kubernetes caveats
  about the ones that cannot (chapter
  13).
- **Ctrl-L — rollback**, on replica
  sets: return to the previous
  revision.
- **`s` — shell**, on pods: a shell
  inside the container, if it has one
  (chapter 13 on the ones that don't).
  **`a` attaches** to the container's
  own console instead — rarer, and
  easier to disturb the program with;
  know which you pressed.

Every one of these goes through the
API server as *you*: the server
checks your permissions, and a
`forbidden` answer means your role
does not include the action —
chapter 14 reads that error. There
is no K9s privilege to exceed yours;
that is chapter 1's rule, mechanised.

## The leash: read-only mode

K9s can be told to forget every verb
above:

- Start with `--readonly`, or
- set `readOnly: true` in
  `config.yaml` (the lab's generated
  default has it `false` — transcript
  t07, verified), globally or in a
  per-context config (chapter 11), or
- `--write` explicitly overrides a
  read-only config when you truly
  mean it.

The header shows the posture: `[RO]`
instead of `[RW]` — visible in every
screenshot and, in the lab's
unreachable capture, conspicuous by
its absence (transcript t03 shows
`[RW]` on a machine with no cluster
and no risk; on a borrowed cluster
that default deserves a second
thought). In read-only mode the edit
key is not offered, the deletes do
nothing, and K9s is what it appears
to be at a glance: a window. Setting
read-only per-context — strict on
production, free on the practice
cluster — is the single habit this
guide most wants to leave you with.

## A word on aim

Speed is K9s' virtue and its hazard:
the distance between "the stuck pod
on my practice cluster" and "a pod,
somewhere" is one forgotten context
switch. The checks that survive
haste are the header (context, user,
namespace, `[RO]`/`[RW]`) and the
confirmation dialog's words — both
are on screen *before* the action.
The tool shows you where you are
standing, every second. Let it.
