# FUUTP™

**Fuck Yo(u) UTP.**

A deliberately small escape route for moving verified artifacts from ChatGPT into GitHub when the normal conversation-file → repository seam is missing or inconvenient.

FUUTP is not application architecture. It is transport.

## The proven ancestor: UTP

World Lab previously published a large self-contained HTML artifact through GitHub's Git-object API.

The important correction is that this did **not** demonstrate a dedicated conversation-file → Git-blob adapter. The working path was effectively:

```text
conversation file
→ Files returned essentially the complete artifact as text
→ that complete string was supplied to GitHub create_blob(content)
→ create_tree
→ create_commit
→ update_ref
```

The Git-object portion was sound. The model/tool context itself served as the transport bridge between Files and `create_blob`.

That distinction matters: a successful Git write does not prove that arbitrary conversation-file bytes can be handed directly to the GitHub connector.

## FUUTP contract

Given a verified standalone artifact and a target GitHub repository:

1. Preserve the artifact exactly. Do not reconstruct, parse/reserialize, minify, refactor, split, or redesign it merely to facilitate transport.
2. Acquire the complete artifact payload through the smallest available transport seam.
3. Write that payload to GitHub.
4. Prefer Git objects when useful:
   `create_blob → create_tree → create_commit → update_ref`.
5. Verify the published artifact against the source whenever the available tools permit meaningful verification.
6. Record the transport path, limits, and failures so the next model does not have to rediscover them.

## What FUUTP is for

FUUTP exists because artifact design should not be distorted by incidental ChatGPT/GitHub connector limitations.

It should remain:

- project-agnostic
- format-agnostic where possible
- byte-preserving in intent
- explicit about what has actually been proven
- willing to use an inelegant transport path rather than redesign the artifact around the pipe

## Current experiment

**T0 specimen: Foundry**

The extracted standalone Foundry laboratory is approximately 236 KB, dramatically smaller than the ~7 MB monolithic World Lab artifact.

This makes it a useful first FUUTP specimen:

```text
verified Foundry standalone
→ recover complete text
→ Git blob
→ tree
→ commit
→ main
→ verify
```

If this works, document the observed envelope. Do not generalize from one successful payload size to an assumed universal limit.

## Archaeology rule

Document what worked, what failed, and what was actually transported.

Do not rewrite history into a cleaner architecture than the one that succeeded.

---

Born from World Lab publication archaeology and a healthy refusal to let the transport layer dictate the experiment.
