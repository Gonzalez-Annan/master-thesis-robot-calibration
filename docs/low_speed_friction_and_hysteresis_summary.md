# Day 03 — Low-Speed Friction and Hysteresis Characterization

## Objective

Day 3 focused on resolving the motor's low-speed friction behavior that could not be characterized reliably from the broad velocity sweep performed on Day 2. The work had two stages:

1. a high-resolution steady-state low-speed sweep to investigate Stribeck-type behavior;
2. an ascending/descending velocity experiment to determine whether friction depends only on instantaneous velocity or also on recent motion history.

The experiments were performed on the unloaded Synapticon motor using ROS 2 velocity control. Motor torque was derived from the drive torque feedback using the previously confirmed rated torque of **2.222 Nm**.

---

## 1. High-resolution low-speed friction experiment

### Acquisition

Three complete sweeps were recorded over the range

\[
-0.2 \leq \omega \leq 0.2\ \mathrm{rad/s},
\]

with dense sampling near zero velocity down to

\[
|\omega| = 0.0025\ \mathrm{rad/s}.
\]

The dataset contained:

- **374,436 samples**
- **3 complete sweeps**
- **35 commanded points per sweep**
- median sampling rate approximately **200 Hz**
- no missing values

Long settling and recording times were used at the smallest velocities. Mean velocity and accumulated shaft position confirmed sustained net motion even at the lowest command values, although instantaneous velocity variation was large relative to the command magnitude.

### Main result

The experiment resolved a repeatable nonlinear low-speed friction region. Torque magnitude was elevated extremely close to zero, decreased over the first few hundredths of a radian per second, and then increased again.

A fully free Stribeck fit gave approximately:

```text
tau_c  = 0.019401 Nm
tau_s  = 0.020189 Nm
w_s    = 0.013073 rad/s
b      = 0.009758 Nm/(rad/s)
offset = 0.000314 Nm
n      = 2.6635
R²     = 0.999696
RMSE   = 0.000351 Nm
```

The fit was numerically excellent, but the fitted viscous coefficient was much larger than the broad-range Day 2 estimate. This indicated parameter coupling caused by attempting to identify viscous drag from a very narrow velocity interval.

A second fit therefore fixed the better-established Day 2 values

```text
tau_c = 0.019903 Nm
b     = 0.002773 Nm/(rad/s)
```

and fitted only the low-speed terms. The resulting constrained model gave approximately:

```text
tau_s  = 0.020622 Nm
w_s    = 0.003909 rad/s
offset = 0.000314 Nm
n      = 1.2779
R²     = 0.999438
RMSE   = 0.000477 Nm
```

The constrained model is more physically defensible, but the fitted `tau_s`, `w_s`, and `n` values had large uncertainty. Therefore Day 3 supports the **existence of a low-speed nonlinear friction transition**, but not a unique high-confidence identification of all classical Stribeck parameters.

A further observation was that sweeps 2 and 3 agreed much more closely than sweep 1 in the smallest positive velocity region. This suggested that startup, thermal state, lubrication state, shaft position, or motion history could be influencing the measured low-speed torque.

---

## 2. Hysteresis / motion-history experiment

### Motivation

The steady-state sweep assumed a memoryless friction relation

\[
	au_f=f(\omega).
\]

The observed sweep-to-sweep variation and zero-speed behavior suggested that this assumption might fail near rest.

To test this, the same low-speed range was traversed both toward larger speed magnitude and back toward zero. Torque at identical commanded velocities could therefore be compared under different motion histories.

### Acquisition

The experiment contained:

- **547,308 samples**
- **3 repeats**
- four complete sequences:
  - `positive_up`
  - `positive_down`
  - `negative_down`
  - `negative_up`
- median sampling rate **199.995 Hz**

Measured velocity followed commanded velocity closely across the complete range, including the smallest nonzero commands.

### Main result

The experiment demonstrated measurable history dependence.

The largest branch separation occurred at

\[
|\omega|=0.0025\ \mathrm{rad/s}.
\]

For negative rotation:

```text
mean branch gap = +8.0517 mNm
repeat std      =  1.8837 mNm
```

This is a large effect compared with the approximately **20 mNm Coulomb-friction magnitude** identified on Day 2 and was repeatable across all three trials.

For positive rotation at the same speed:

```text
mean branch gap = +3.3823 mNm
repeat std      =  6.0168 mNm
```

The positive-side result was not repeatable enough to assign a unique hysteresis value at that point.

A smaller but systematic hysteresis effect was observed over approximately

\[
0.02 \lesssim |\omega| \lesssim 0.075\ \mathrm{rad/s}.
\]

In the `0.02–0.05 rad/s` region, the mean signed branch differences were approximately:

```text
negative: -0.794 mNm
positive: -0.780 mNm
```

Above approximately **0.1 rad/s**, branch separation became small.

### Zero-speed history dependence

The strongest evidence of memory dependence was observed at zero command. Depending on preceding motion direction, mean zero-command torque was roughly:

```text
+24 to +31 mNm
or
-15 to -34 mNm
```

The sign and magnitude therefore depended strongly on the preceding motion state.

This means friction torque at rest cannot be represented by one unique value of

\[
	au_f(0).
\]

---

## Modeling consequence

The Day 2 and Day 3 experiments support retaining the following model as the nominal **sliding-friction model**:

\[
	au_f(\omega)
=
	au_c\,\operatorname{sgn}(\omega)
+b\omega
+	au_{\mathrm{Stribeck}}(\omega).
\]

The identified broad-range values remain approximately:

```text
Coulomb friction:
tau_c ≈ 0.0199 Nm

Viscous coefficient:
b ≈ 0.00277 Nm/(rad/s)
```

However, Day 3 shows that the very-low-speed and pre-sliding regime is not purely memoryless. Near zero velocity, torque also depends on previous motion or an internal friction/contact state.

For the thesis, the Coulomb + viscous + Stribeck model will therefore be treated as the nominal sliding model, with its limitation around zero velocity documented explicitly. Its adequacy will later be evaluated based on compensation and impedance-control performance rather than by forcing the near-zero data into an unnecessarily complex model.

---

## Day 3 conclusion

Day 3 achieved three important results:

1. the low-speed nonlinear/Stribeck region was experimentally resolved;
2. the classical Stribeck parameters were shown to be only weakly identifiable when fitted independently over the narrow low-speed range;
3. a dedicated ascending/descending experiment confirmed genuine motion-history dependence, concentrated primarily near zero velocity.

The findings explain why a single steady-state friction-versus-velocity curve cannot fully describe the motor around rest.

The next experiment will use **torque control** to characterize static/breakaway friction directly. Because Day 3 demonstrated strong motion-history dependence at rest, the breakaway protocol must use controlled and repeatable pre-conditioning before every torque ramp.
