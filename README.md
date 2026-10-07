# Robot Dynamics and Friction Characterization

Experimental characterization, modeling, and control work developed as part of a Master's thesis in robotics.

The project focuses on building a reusable workflow for identifying motor and joint dynamics as a foundation for model-based compensation and impedance control.

## Project scope

The first stage focuses on a single motor-driven joint and includes:

- communication and unit validation
- velocity and torque characterization
- Coulomb and viscous friction identification
- low-speed / Stribeck characterization
- hysteresis and motion-history analysis
- static / breakaway friction characterization
- current-to-torque characterization
- position and load dependence
- 1-DOF dynamic modeling
- model validation on unseen data
- model-based compensation
- impedance control

The methodology is intended to be reusable and later extendable to multi-joint robotic systems.

## Repository structure

```text
.
├── docs/
│   ├── system_setup_and_unit_validation.md
│   ├── velocity_torque_characterization.md
│   ├── low_speed_friction_and_hysteresis_summary.md
│   └── ...
│
├── protocol/
│   ├── 2026-10-05.md
│   ├── 2026-10-06.md
│   ├── 2026-10-07.md
│   └── ...
│
├── notebooks/
│   ├── low_speed_friction_analysis.ipynb
│   ├── hysteresis_analysis.ipynb
│   └── ...
│
├── scripts/
│   ├── acquisition/
│   └── analysis/
│
├── figures/
├── config/
├── requirements.txt
└── .gitignore

