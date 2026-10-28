# Limits

What K9s cannot do, and what it must
not be asked to do — the chapter the
rest of the guide has been promising.

## No cluster, no picture

K9s is a window, and a window needs
something on the other side of the
glass. Without a reachable API
server it can show you contexts,
aliases, help, and its own settings —
the lab verified exactly that much,
and it is genuinely all. It cannot
cache yesterday's cluster for
browsing, work offline against a
recording (kubectl has no such mode
either; flow files are not a K9s
concept), or tell you anything about
a cluster it cannot currently reach.
And its evidence about a cluster it
*can* reach is only ever the present:
it keeps no history, so "when did
this start failing" is answered by
events and log timestamps, not by
K9s' memory — it has none.

This guide's own limit sits inside
that one and is stated in
`criteria.md`: with no cluster able
to run in its lab, everything past
the connection chapters is taught
from K9s' documentation. A guide
that ran a cluster would have
screenshots of one; this one has
transcripts of the honest screens —
including the one that says `n/a`.

## Borrowed power cuts both ways

Everything K9s does, it does as your
kubeconfig's user. The bright side:
no extra accounts, no agent in the
cluster, nothing to install
server-side. The dark side is the
same sentence read again:

- **Your mistakes travel at your
  full permission.** If your account
  can delete a namespace, K9s'
  Ctrl-D can, after one confirmation.
  If it can read Secrets cluster-wide,
  K9s can display them. The tool adds
  speed, not restraint; restraint is
  read-only mode (chapter 9) and
  scoped credentials — a view-only
  service account for the clusters
  you only watch is a kubeconfig
  decision, and a good one.
- **Its vision is bounded by RBAC.**
  What your role cannot list, K9s
  cannot show; an empty view may
  mean "nothing there" or "not for
  your eyes", and the `forbidden`
  errors in the log (chapter 14) are
  how you tell.
- **Deleting a thing is not deleting
  its consequences.** The API server
  honours finalisers, owners and
  grace periods in its own time;
  K9s shows the object
  `Terminating` while the machinery
  finishes. Ctrl-K removes the grace,
  not the machinery.

## A screen that keeps no record

kubectl in a shell leaves history,
scripts leave files, pipelines leave
logs. A K9s session leaves almost
nothing: no record of which resources
you viewed, which YAML you read,
what you deleted at 14:03 and why.
The screen dumps you save (Ctrl-S)
are yours to keep, but they are a
choice, not a trail. For shared and
regulated systems that is a real
limit — the audit log that matters
is the API server's own, if the
cluster keeps one, and it records
the *user*, not which tool the user
was driving. Do not let "it was just
K9s" enter an incident report as if
it meant "informal": the API calls
were identical to kubectl's.

## The edges of the view

- **Scale.** On very large clusters
  the watch itself costs: every
  change redraws, wide views carry
  many columns, and the log view's
  buffer is finite (chapter 8's
  5000 lines by default). K9s is
  built for an operator's attention,
  not for ten thousand pods at once.
- **The metrics gap.** Without the
  optional metrics server in the
  cluster, the CPU and memory
  columns and header figures are
  empty (the lab's log noted its
  absence politely and carried on).
  K9s is showing the cluster's own
  instrumentation, not measuring
  anything itself.
- **Shells need shells.** `s` into a
  container only works if the image
  carries a shell; distroless and
  scratch images famously do not.
  That is the image's design, not a
  K9s fault; debug containers are
  the cluster's answer, and a
  different guide's chapter.
- **Node operations are blunt.**
  Drain respects the cluster's
  eviction rules and stops where
  they say stop; daemon-set pods
  and unmanaged pods need deliberate
  extra force that K9s will ask
  about and you should read twice.
- **One context at a time.** The
  screen shows one cluster's truth.
  Comparing two clusters is two
  windows, and keeping straight
  which is which is — again — the
  header's job and yours.

## The legal and ethical edge

Chapter 1's rule — *your clusters,
or explicit permission to administer
what you can reach* — is not merely
this guide being prim. Administering
systems you have no right to touch
is computer misuse in most
jurisdictions, with or without
deletion; a borrowed kubeconfig is
someone else's key, and "I only
looked" sits badly next to a tool
whose look and delete are two keys
apart. Employers' clusters come with
employers' rules, which outrank this
guide. Everything inside the line —
your clusters, your lab, the work
you are authorised for — needs no
apology and no concealment, and it
is years of interesting country.
