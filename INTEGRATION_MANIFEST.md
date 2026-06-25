# Integration Manifest

## Target Repository

`learnsyslab/safe-control-gym`

## Copy These Paths Into a Branch

```text
safe_control_gym/controllers/networked/
examples/networked_control/
tests/controllers/networked/
docs/networked_control.md
README_NETWORKED_PETC_PR.md
```

## Minimal PR Scope

This contribution adds an optional wrapper. It does not alter existing
controllers, environments, tasks, or experiment code paths unless a user imports
`safe_control_gym.controllers.networked`.

## Smoke Commands

```bash
python examples/networked_control/run_dynamic_petc.py --max-delay-steps 3
python -m pytest tests/controllers/networked -q
```

After installing the full safe-control-gym dependencies, run:

```bash
python examples/networked_control/networked_lqr_experiment.py \
  --algo lqr \
  --task cartpole \
  --overrides ./examples/lqr/config_overrides/cartpole/cartpole_stab.yaml \
              ./examples/lqr/config_overrides/cartpole/lqr_cartpole_stab.yaml
```

## Before/After Protocol

Use the same seed, task, controller, and number of steps.

```text
Before: base controller with fresh observation every step.
After A: base controller wrapped with fixed periodic delayed transmission.
After B: base controller wrapped with DynamicPETCTrigger.
```

Report both task and network metrics:

```text
task return or RMS tracking error
constraint/safety violation count when available
transmission count
event rate
mean stale steps
max queue depth
mean eta
mean trigger margin
```

## Reviewer-Facing Claim

The module provides a reusable engineering abstraction for networked control
evaluation under sampled, delayed, event-triggered communication. It does not
claim closed-loop ISS for arbitrary safe-control-gym tasks unless the user
supplies compatible task-specific certificate bounds.
