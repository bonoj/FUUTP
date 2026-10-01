# FUUTP

Model-operated exact artifact transport across tool seams.

## Prime directive

Move the accepted artifact without changing it. Human readability is not a design constraint. Prefer compact operational truth for the next model.

## Invocation

The word `FUUTP` authorizes transport, not publication beyond the named destination. Infer direction from context, execute autonomously, verify identity, and do not ask the human to shuttle bytes until every documented exact route has failed at a named primitive.

## Invariants

1. Artifact bytes are sovereign. Never parse/reserialize, minify, normalize, re-encode, recompress, redraw, or otherwise mutate them for transport.
2. Establish expected identity before transport when possible. For Git destinations use Git blob SHA: `sha1("blob "+byteLength+"\\0"+bytes)`.
3. Destination identity MUST equal expected identity before promotion.
4. Once the exact final Git blob exists, bytes are finished moving. Rename/place/promote by Git tree operations only.
5. Carriers are disposable transport state.
6. Transport authority is separate from canonical publication authority.
7. Report the narrowest failed primitive, not a vague platform failure.
8. Preserve newly earned routes/failures here. Delete obsolete prose freely.

## Route selection

### G→R UTF-8 (proven)

GitHub path → blob SHA → Git-data blob fetch → exact UTF-8 → runtime/file surface → recreate Git blob identity → require SHA equality.

Large accepted specimen: `bonoj/VerticalAccretion/main/index.html`, source blob `3940ced8e70fe198ee08f99a63848cb31fe2e04d`.

If direct runtime materialization is unavailable, proven T2 carrier:
UTF-8 bytes → conservative chunks → base64 → disposable native Google Docs → text/plain export → runtime materialization → strip carrier BOM/whitespace from base64 only → decode → concatenate → verify Git blob SHA → delete Docs.

### R→G UTF-8 (proven)

Working text → exact payload → GitHub create_blob/create_file as appropriate → verify destination blob identity → tree/commit/ref promotion.

Small specimen Foundry passed. Multi-megabyte SixCities demonstrated Git-resident carrier transport; carrier→final-blob assembly must still be identity-gated.

### Conversation native binary → GitHub (OPEN)

Current specimen:
- source: conversation image `1681.png`
- target: `bonoj/Crucible/reference/CLARAS_HOME_ARCOLOGIES.png`
- source bytes: 706957
- expected Git blob SHA: `d209d07ebbbaa7bbf64efe077c5fba5c62a01656`

Rejected route:
conversation image → Files image read → returned image payload → GitHub create_blob(base64)
produced blob `f7c3e6b671210cae43f46fba7fb3afc434452d35`, therefore the image-read representation is transformed and MUST NOT be promoted.

Required missing primitive:
byte-transparent conversation-file reference/raw backing file → bytes/base64 or Git blob, without image decode/re-encode.

Preferred probes, in order:
A. Automatically mounted conversation backing file in execution container → read raw bytes → compute expected SHA → bridge raw bytes/base64 directly to GitHub if a tool can consume runtime output without model-visible bulk payload.
B. Byte-transparent connector file reference → storage carrier → raw download/materialization → verify SHA → GitHub.
C. Conservative binary chunks encoded as base64 carriers → exact reassembly → verify SHA → final Git blob.
Do not use image rendering/vision/image-read output as a binary carrier.

## Git promotion

Given verified final blob B and target path P:
resolve target branch head/tree → create tree replacing/adding P with B (and deleting disposable carriers if applicable) → create commit → update ref → fetch P → require blob SHA == B.

Never upload B again merely to move or rename it.

## Failure discipline

A failed identity gate is evidence, not an inconvenience. Keep source untouched, record source identity, candidate identity, exact seam, and next missing primitive. Never silently accept visual/textual equivalence where byte identity is required.

## Archaeology

- UTP ancestor: model context carried complete text into Git create_blob.
- T0 Foundry: bounded Files reads + in-call concatenation + GitHub write; text equality PASS.
- T1 VerticalAccretion: Git-data blob endpoint recovered ~3 MB UTF-8; recreating source Git blob SHA PASS.
- T2 VerticalAccretion: base64 Google-Doc envelopes bridged GitHub payload into runtime; final Git blob SHA PASS. Raw HTML through Docs FAILED because export transformed it.
- T3 SixCities: ~6 MB crossed into Git as carrier chunks. Repository-local Actions promotion was unnecessary and rejected as default architecture.
- T4 Clara Home Arcologies: native image read is NOT byte-transparent; expected `d209d07e…`, transformed candidate `f7c3e6b6…`. Binary file-reference→Git seam remains OPEN.

## Finish line

FUUTP succeeds only when the requested destination resolves to the expected artifact identity. Anything less is a diagnosed crossing, not a completed transport.
