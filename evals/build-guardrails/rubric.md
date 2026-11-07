# Build Guardrails evaluation rubric

Start a fresh agent session for each [case](cases.yaml). Provide the skill and the case prompt only. Compare the response with the case expectations after the run.

| Dimension | Strong evidence |
| --- | --- |
| Context and coverage | Considers relevant lenses and ties material risks to the feature, data flow, or agent action. Gives brief reasons for meaningful exclusions. |
| Risk and timing | Prioritizes proportionate controls and explains which unresolved decisions affect implementation, merge, or release. |
| Enforcement and bypass | Gives each material control an invariant, trigger, and effective enforcement point. Identifies bypass paths and independent coverage where needed. |
| Failure behavior and verification | Defines violation and checker-failure handling. Provides observable tests for allowed, rejected, and applicable failure or bypass paths. |
| Evidence and authority | Separates proposed, implemented, and verified controls. States assumptions, accountable roles, and any required decisions without inventing policy. |

Score each dimension **0** (missing or harmful), **1** (partial), or **2** (strong). Pass at **8/10** or higher with at least **1** in enforcement and verification and no case-specific critical failure. Record one piece of evidence per score. Rerun the failed case and a contrasting case after a change.
