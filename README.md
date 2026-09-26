# FUUTP™

**Fuck Yo(u) UTP.**

FUUTP is our small, homebrewed escape route for moving verified artifacts from ChatGPT into GitHub when the useful artifact exists on one side of the seam and the repository tools exist on the other.

It is not an architecture, a framework, or a claim of novelty. We had a specific transport problem, looked for an existing solution, did not quickly find one that fit the seam available to us, and built the smallest thing that got our artifacts across it. We are keeping it because we expect to need the pipe again.

The name records the circumstances of its birth. We don't need to keep repeating the joke.

## How a model should use FUUTP

**FUUTP is an external, bidirectional transport tool. Do not embed it in the artifact being transported.**

When the user says to use, invoke, or "hop over to" FUUTP, treat this repository as operational instructions. Do not ask the user to manually upload/download/shuttle a repository artifact until the documented FUUTP routes have actually been attempted.

### Pull: GitHub → working artifact

For a UTF-8 text artifact already in GitHub:

```text
repository + ref + path
→ resolve Git blob SHA
→ recover complete blob through GitHub Git-data API
→ verify payload
→ materialize/write recovered UTF-8 as the working file
→ execute/inspect/edit locally as required
```

Operational rules:

1. Try the ordinary repository file read when appropriate.
2. If a large file returns metadata/identity but empty, truncated, or rejected content, **do not stop and do not ask the human to shuttle it**.
3. Resolve the blob SHA and fetch the Git object through:
   `/repos/{owner}/{repo}/git/blobs/{sha}`.
4. Treat the returned blob content as the artifact payload. For UTF-8 HTML, the recovered string itself is the executable source; writing those characters as UTF-8 produces the working HTML artifact. A separate connector file reference is not conceptually required.
5. Verify fidelity. Strongest demonstrated check: recreate a Git blob from the recovered UTF-8 and require the resulting SHA to equal the source blob SHA.
6. Keep repository canonical state untouched unless the user separately authorizes publication/promotion.

Do not confuse **payload acquisition** with **execution locality**. If one tool call recovers the complete string and another execution surface must receive it, bridge the payload directly using whatever code/file-writing surface is available in that runtime. Do not invent a requirement for Actions, releases, file-reference adapters, artifact splitting, reconstruction, minification, or human download/upload merely because a high-level file reader failed.

### Push: working artifact → GitHub

```text
verified working artifact
→ recover complete payload through available Files/tool seam
→ GitHub contents write when sufficient
   OR create_blob → create_tree → create_commit → update_ref
→ fetch/verify published result
```

Preserve the accepted artifact rather than redesigning it for transport. Prefer the smallest route demonstrated to work for the payload.

### Invocation finish line

A successful FUUTP invocation should make this user workflow sufficient:

```text
"Work from <repository URL>. Use FUUTP."
```

The model should then recover the repository artifact, work from it, and return or publish the resulting artifact according to the user's authority—without requiring the human to manually carry the artifact across the seam.

Think of FUUTP as a model-operated workshop tool. Transport limitations belong here, not in every sovereign lab.

**Transport is FUUTP's problem. The artifact's job is to be the artifact.**

## T2 result — operational GitHub → sandbox PASS

Vertical Accretion closed the execution-locality seam.

Source specimen:

- `bonoj/VerticalAccretion/main/index.html`
- source Git blob SHA: `3940ced8e70fe198ee08f99a63848cb31fe2e04d`
- recovered UTF-8 bytes: 2,969,582

Proven route:

```text
GitHub path
→ resolve source blob SHA
→ Git-data blob fetch
→ exact UTF-8 string in tool orchestration
→ UTF-8 encode
→ split bytes into conservative transport chunks
→ base64 each chunk
→ temporary native Google Docs
→ export each Doc as text/plain
→ exported connector file references auto-materialize into /mnt/data
→ locally remove transport whitespace/BOM
→ base64 decode each chunk
→ concatenate bytes in order
→ write executable artifact
→ calculate local Git blob SHA
→ require exact equality with source blob SHA
→ delete temporary Drive carriers
```

Observed result:

```text
local reconstructed Git blob SHA
3940ced8e70fe198ee08f99a63848cb31fe2e04d

source Git blob SHA
3940ced8e70fe198ee08f99a63848cb31fe2e04d

IDENTICAL = true
```

### Important failed carrier

Do **not** carry raw HTML through a native Google Doc. The export introduced transformations beyond BOM/line-ending changes and failed Git-object identity.

Base64 is the proven carrier because its alphabet survives the Docs text-export seam. The Docs are disposable transport envelopes, not artifact storage or semantic documents.

### Operational instruction

When direct GitHub/raw download into the execution sandbox is unavailable but Google Drive connector export can produce runtime file references, use the T2 base64 chunk bridge automatically. Do not ask the human to shuttle the artifact.

Chunk sizing is an implementation detail; keep chunks comfortably below Google Docs/document-edit and tool-call limits. Multiple orchestration calls are acceptable. Always verify the final reconstructed bytes against the source Git blob SHA before treating the local artifact as authoritative.

Temporary Drive carriers must be clearly named disposable and deleted after successful reconstruction.

This makes FUUTP operationally bidirectional for the demonstrated UTF-8 artifact class:

```text
working file → GitHub
GitHub → working file
```

Both directions have exact-fidelity evidence.

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


## T0 result — PASS

Foundry crossed the seam successfully.

Observed route:

```text
Files read: foundry_standalone.html
→ three contiguous text windows (lines 1–1000, 1001–2000, 2001–2765)
→ concatenate in-order inside one tool-orchestration call
→ GitHub contents write to bonoj/Foundry/main/index.html
→ fetch published index.html
→ compare complete source and published strings
```

Observed payload:

- 2,765 source lines
- 235,343 JavaScript string characters
- source and fetched GitHub content had identical character counts
- complete source/published string comparison returned true
- an independent FNV-style comparison over the JavaScript strings also matched: `48962312`
- published Git blob SHA: `6a0fe9c04fae292bd86e25fd3231749fe42c63e1`
- publication commit: `63db4f21d99293d3f1f994994009089f46ffe7ab`

### What T0 actually proved

For this UTF-8 HTML specimen, FUUTP can bridge a conversation file to GitHub without emitting the artifact through model-visible output. Files can return bounded contiguous text windows inside a tool-orchestration call; those windows can be concatenated there and supplied directly to the GitHub write operation.

The complete UTF-8 text recovered from GitHub was identical to the complete UTF-8 text recovered from Files.

This is **text-faithful transport evidence**. It is not yet a general byte-faithful claim: the experiment did not independently hash the original uploaded byte stream before Files decoded it as text. Binary artifacts, encoding edge cases, larger payloads, and connector/tool-call limits remain unproven.

### Useful discovery

The T0 route did not need the lower-level Git-object sequence. GitHub's contents write was sufficient to initialize the empty Foundry repository and create `main/index.html`.

That makes the smallest currently proven FUUTP path:

```text
Files bounded reads
→ in-call concatenation
→ GitHub create_file
→ GitHub fetch_file
→ full-text equality check
```

Keep the Git-object route as an earned historical fallback, not mandatory ceremony.


## T1 experiment — inverse transport from GitHub

Vertical Accretion exposed the inverse seam: a model could inspect repository metadata but initially could not recover its accepted ~3 MB executable through the obvious repository-file readers.

Specimen:

- repository: `bonoj/VerticalAccretion`
- path: `main/index.html`
- Git blob SHA: `3940ced8e70fe198ee08f99a63848cb31fe2e04d`

### T1 result — PAYLOAD PASS

The obvious routes failed:

```text
GitHub fetch_file(index.html)
→ blob identity returned
→ content empty

GitHub fetch_file(index.html, bounded line range)
→ blob identity returned
→ content empty

GitHub raw.githubusercontent.com fetch
→ rejected as too large or unsupported
```

The Git-data route succeeded:

```text
known repository path
→ fetch_file to obtain blob SHA
→ GitHub Git blob endpoint: /repos/{owner}/{repo}/git/blobs/{sha}
→ complete UTF-8 text recovered inside one tool-orchestration call
```

Observed payload:

- 2,969,176 JavaScript string characters
- starts with `<!doctype html>`
- ends with `</body></html>`
- FNV-1a 32-bit over the recovered JavaScript string: `1239264b`

This establishes a useful inverse acquisition route for a large UTF-8 Git blob even when the higher-level file readers refuse or truncate the payload.

### What T1 proves

T1 proves **GitHub → complete model/tool-orchestration payload** for this ~3 MB UTF-8 blob. The recovered string was subsequently shown to recreate the identical Git object.

For an HTML artifact, that complete UTF-8 payload is already the executable source. Do not mistake the absence of a connector-specific file reference for absence of the artifact. When execution requires a filesystem path, write/materialize the recovered UTF-8 through the available execution/file-writing surface.

### Current inverse route

When a large UTF-8 repository file is needed:

1. Resolve its Git blob SHA with the normal repository metadata/file operation.
2. Fetch `/repos/{owner}/{repo}/git/blobs/{sha}` through the GitHub connector.
3. Verify character count and an in-call checksum when useful.
4. If the next operation can consume the recovered string directly, continue without surfacing it through model output.
5. If execution requires a local/container file, write the recovered UTF-8 as that file through the available execution surface. Do not ask the human to shuttle the artifact merely because the GitHub connector itself returned text rather than a file reference.

Do not install transport workflows into specimen repositories merely to compensate for this seam. Transport remains FUUTP's problem.

### T1 fidelity upgrade — Git-object-identical PASS

The recovered 2,969,176-character Vertical Accretion payload was supplied directly to GitHub `create_blob(content, encoding=utf-8)` in the FUUTP repository. GitHub returned:

```text
3940ced8e70fe198ee08f99a63848cb31fe2e04d
```

That is exactly the source blob SHA from `bonoj/VerticalAccretion/main/index.html`.

For this UTF-8 specimen, inverse acquisition is therefore stronger than character-count or auxiliary-checksum evidence: the recovered string recreates the identical Git object. T1 acquisition is **text-faithful at Git-object identity**.

The payload-fidelity question is closed for this specimen. Execution locality is an adapter concern: the recovered UTF-8 is the artifact, and a runtime that needs a path should write those exact characters to the working file. Do not elevate a missing connector-specific `file reference` into a transport requirement.
