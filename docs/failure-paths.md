# Failure and Closure Notes

- Stop the affected wave on failed prechecks or validation.
- Document when to retry, restore a configuration, revert a checkpoint, use a full backup, or intervene manually.
- Do not assume every update supports rollback.
- Record actions, results, evidence, exceptions, and outstanding work in the change record (and durable local evidence).
- Remove successful VM checkpoints after the agreed observation period and verify their merge.
- If ServiceNow PDI is unavailable, retain durable workflow definition and evidence in this repository and AAP job artifacts.
