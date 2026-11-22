# Conformance

| | |
|---|---|
| **Spec version** | 1.4.0 |
| **Level** | L2 (core + batching, caches, injection, safety) |
| **Reference** | `restlesshq/node` (`@restlessai/sdk`) |
| **Driver** | `python -m restless._conformance` |

Declared in `src/restless/_version.py` (META-001).

## Verifying

The harness and vectors live in the reference SDK, so the commands below
assume it is checked out as a sibling (`../node`), which is how
`setup.sh` in the install repo arranges things. The vectors in `spec/` here
are a pinned copy, so `python -m unittest` alone works without it.


```sh
# in-process, no Node needed
PYTHONPATH=src python -m unittest discover -s tests

# the shared cross-language harness
PYTHONPATH=src node ../node/spec/harness/run-vectors.mjs \
  -- python -m restless._conformance

# differential fuzz against the reference implementation
PYTHONPATH=src node ../node/spec/harness/fuzz.mjs \
  --ref  "node ../node/spec/driver/.build/node.js" \
  --test "python -m restless._conformance" \
  --iterations 20000
```

Current status: **269 vectors, 262 passed, 0 failed, 7 skipped.** Zero
