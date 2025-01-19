# github-action-multiple-ci-workflow-playground

Testing multiple required workflow with same job name

##  Use case

You want multiple CI workflow to be marked as required before merging a PR.
A more detailed example can be found on this [tuono repository issue](https://github.com/tuono-labs/tuono/issues/336).

To achieve this I performed the following steps:

- I added an step with `ci_ok` key and `OK` name on each required workflow.
- Then, on the `main` branch ruleset I added **only one "OK" match**.

> [!CAUTION]
> This causes the following unwanted behavior:
> A merge is allowed even if one of the need jobs is still in progress, as long as at least one `ci_ok` job has already completed.
>
> **Example:** If one job takes 3-4 minutes while others complete after 30 seconds, the merge might is permitted prematurely.
>
> **Reason:** The ruleset requires all `ci_ok` jobs to pass, but if no `ci_ok` job is in progress (because dependent jobs are incomplete), it can lead to inconsistent merge conditions.

---

> [!TIP]
> To be sure that all `ci_ok` jobs are required to merge have all workflows each `ci_ok` job should have a unique name.
> By having unique names each job can have an exact match via the ruleset matcher.
> Refer to:
>
> - [workflows file in this repository](https://github.com/marcalexiei/github-action-multiple-ci-workflow-playground/tree/main/.github/workflows)
> - [`main` branch ruleset of this repository](https://github.com/marcalexiei/github-action-multiple-ci-workflow-playground/rules?ref=refs%2Fheads%2Fmain)

---

> [!NOTE]
> As an alternative having a unique CI workflows could be more straightforward, but sometime, like in `tuono` this could not be feasible because
