# Robot Dynamics and Friction Characterization

Experimental and modeling work developed as part of a Master's thesis in robotics.

The project develops a reusable workflow for motor and joint characterization as a foundation for model-based compensation and impedance control.

## Current scope

The first stage focuses on a single motor/joint and includes:

- motor communication and unit validation
- velocity and torque characterization
- Coulomb and viscous friction identification
- low-speed / Stribeck characterization
- hysteresis and motion-history analysis
- static / breakaway friction characterization
- 1-DOF dynamic modeling
- model validation
- friction and gravity compensation
- impedance control

The methodology will later be extended to a multi-joint robotic system.

## Repository structure

- `docs/` — experimental summaries and development notes
- `notebooks/` — reproducible analysis notebooks
- `scripts/acquisition/` — experiment acquisition scripts
- `scripts/analysis/` — reusable analysis tools
- `figures/` — selected generated figures
- `config/` — non-sensitive experiment configuration

## Experimental progress

### Day 1
Initial communication, position-control validation, encoder/unit investigation.

### Day 2
Velocity scaling validation, torque-feedback conversion, broad velocity sweep, and Coulomb/viscous friction identification.

### Day 3
High-resolution low-speed friction characterization and ascending/descending hysteresis analysis.

Main observations:

- Coulomb friction is approximately 0.020 Nm.
- Broad-range viscous coefficient is approximately 0.00277 Nm/(rad/s).
- A nonlinear low-speed friction region is present.
- Strong motion-history dependence appears close to zero velocity.
- Hysteresis becomes substantially smaller away from the near-zero regime.

### Next
Torque-controlled static/breakaway friction characterization, followed by development of the 1-DOF dynamic model.

## Data

Raw experimental datasets are intentionally not included in the public repository.

## Status

Work in progress.
