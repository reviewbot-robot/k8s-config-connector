# APIHubAPI Types Journal

## Shortcomings & Solutions

### Correcting the Reference Pattern

* **Context:** The direct KRM types, identity, and reference implementation for `APIHubAPI` under `apis/apihub/v1alpha1` have been audited.
* **Observation:** The reference pattern for `APIHubAPIRef` was initially using `refs.NormalizeWithFallback` to fall back to resolving identity from the `spec`. However, this fallback mechanism is unsafe because `APIHubAPI` is a direct/greenfield resource that is guaranteed to populate `status.externalRef` upon successful reconciliation. Relying on fallback to the `spec` risks resolving identities for resources that are not yet ready or created in GCP.
* **Solution/Validation:** Corrected `APIHubAPIRef.Normalize` to strictly delegate to `refs.Normalize(ctx, reader, r, defaultNamespace)`. Ran local validation checks via `go test ./apis/apihub/...` to ensure correct compilation and test coverage. All reference patterns and tests now correctly align with the standard direct resource architecture.
