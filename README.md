# FUUTP™

**Fuck Yo(u) UTP.**

FUUTP is our small, homebrewed escape route for moving verified artifacts from ChatGPT into GitHub when the useful artifact exists on one side of the seam and the repository tools exist on the other.

It is not an architecture, a framework, or a claim of novelty. We had a specific transport problem, looked for an existing solution, did not quickly find one that fit the seam available to us, and built the smallest thing that got our artifacts across it. We are keeping it because we expect to need the pipe again.

The name records the circumstances of its birth. We don't need to keep repeating the joke.

## Why this repo exists

World Lab began as a large self-contained artifact containing many laboratories. Publishing it exposed an awkward boundary: ChatGPT could possess and inspect the artifact, and the GitHub connector could construct Git objects, but there was no obvious direct conversation-file → Git-blob operation.

That transport limitation is not an application-design constraint.

As World Lab moves toward sovereign lab repositories, FUUTP gives us one place to preserve whatever transport machinery actually works instead of rediscovering it in every expedition.

## The proven ancestor: UTP

World Lab previously published a large self-contained HTML artifact through GitHub's Git-object API.

The important archaeological correction is that this did **not** prove a dedicated conversation-file → Git-blob adapter. The working path was effectively:

```text
conversation file
→ Files returned essentially the complete artifact as text
→ that complete string was supplied to GitHub create_blob(content)
→ create_tree
→ create_commit
→ update_ref
```

The Git-object portion was sound. The model/tool context itself served as the transport bridge between Files and `create_blob`.

That distinction matters. A successful Git write does not prove that arbitrary conversation-file bytes can be handed directly to the GitHub connector.

## Working principles

**Move the artifact; don't redesign it for the pipe.**  
A verified standalone artifact should not be reconstructed, parsed/reserialized, minified, refactored, split, or otherwise changed merely to facilitate transport.

**Executable evidence over imagined infrastructure.**  
Add machinery only when a real publication attempt earns it. A workaround that succeeds is more useful than an elegant transport architecture we have not exercised.

**Preserve archaeology.**  
Record what actually worked, what failed, what payload crossed the seam, and what remained unverified. Do not rewrite history into a cleaner mechanism than the one that succeeded.

**Verify when possible.**  
Hashes, Git blob SHAs, fetched content, byte counts, or other evidence should be used when the available tools make meaningful verification possible. Be precise about whether a path is byte-faithful or merely text-faithful.

**Stay portable.**  
FUUTP should know as little as possible about World Lab, Foundry, HTML, GitHub Pages, or any particular application's architecture. Those are specimens and destinations, not FUUTP's ontology.

## Transport contract

Given a verified standalone artifact and a target GitHub repository:

1. Acquire the complete artifact payload through the smallest available seam.
2. Preserve the accepted artifact rather than rebuilding it.
3. Write the payload to the requested repository/path.
4. Use the simplest GitHub path that works. Git objects are available when useful:
   `create_blob → create_tree → create_commit → update_ref`.
5. Fetch or otherwise verify the published result when possible.
6. Record any newly demonstrated capability, limit, or failure here.

FUUTP may accumulate multiple routes. They do not need to pretend to be one universal protocol.

## T0 experiment — Foundry

The first deliberate FUUTP specimen is the extracted standalone Foundry laboratory, approximately 236 KB—dramatically smaller than the roughly 7 MB monolithic World Lab artifact from which the publication problem became obvious.

The intended experiment is deliberately boring:

```text
verified Foundry standalone
→ recover complete payload
→ GitHub
→ main/index.html
→ verify
```

If that succeeds, we document the observed envelope. We do **not** infer a universal size limit from one successful artifact.

If it fails, the failure becomes evidence and FUUTP earns its next piece of machinery.

## Non-goals

FUUTP does not need to become a package manager, deployment platform, artifact format, World Lab runtime, universal Git abstraction, or respectable piece of enterprise middleware.

It just needs to get the artifact through the seam without making the artifact become something else.
