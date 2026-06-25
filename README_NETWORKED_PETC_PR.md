# PR: Networked Dynamic PETC Controller Wrapper

## Purpose

This overlay adds a practical networked-control wrapper to safe-control-gym. It
lets existing controllers run behind sampled, delayed, event-triggered
communication channels.

## Files to Add

```text
safe_control_gym/controllers/networked/
  __init__.py
  channel.py
  metrics.py
  scheduler.py
  trigger.py
  wrapper.py
examples/networked_control/run_dynamic_petc.py
examples/networked_control/networked_lqr_experiment.py
tests/controllers/networked/
  test_delay_channel.py
  test_trigger.py
  test_wrapper.py
docs/networked_control.md
```

## Public API

```python
NetworkedControllerWrapper(controller, observation_dim, sample_period_steps,
                           max_delay_steps, delay_sampler, trigger)
DynamicPETCTrigger(error_weight, signal_weight, eta_decay, eta_growth)
DelayChannel(name, dim, sample_period_steps, max_delay_steps, delay_sampler)
```

## Before/After Demonstration

The minimal demonstration compares a direct proportional controller against the
same controller wrapped by dynamic PETC and bounded random delays:

```bash
python examples/networked_control/run_dynamic_petc.py --max-delay-steps 3
```

For a full safe-control-gym PR, the same wrapper can be applied to the existing
LQR/MPC examples by replacing `ctrl` with `NetworkedControllerWrapper(ctrl, ...)`
before constructing `BaseExperiment`.

## Review Checklist

- The base controller API is unchanged.
- Existing controllers do not import the networked package.
- Delays are measured in sample steps, not seconds.
- The delay channel preserves FIFO delivery order.
- The dynamic trigger is evaluated only at sampling instants.
- The example reports communication metrics separately from task metrics.
