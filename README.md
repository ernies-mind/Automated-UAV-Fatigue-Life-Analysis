# Automated-UAV-Fatigue-Life-Analysis
A Python pipeline that automates the structural fatigue methodology used on certified rotorcraft, applied to a multirotor UAV arm.

-----------------------------------------------------------------------------
The Problem:
Fatigue analysis answers: how long can this part fly before it cracks? In industry the workflow is manual — an FEA setup for every load case, stresses walked through S-N curves in a spreadsheet. A 15-case spectrum means 30 hand-built runs, repeated for every design revision.

This pipeline automates the whole chain. Change the geometry or mission, rerun one notebook, get a new service life.

-----------------------------------------------------------------------------

How It Works:

Flight telemetry (synthetic)                  From test stand data or UAV telemetry

Rainflow cycle counting                       ASTM E1049, per flight segment


Fatigue load spectrum                         Set / Case / F_min / F_max / cycles


Automated FEA                                 Python templates the CalculiX deck, runs the solver, parses σ₁ per case
        
S-N curve + mean stress correction            MMPDS, 7075-T6


Miner's Rule damage summation                 D = Σ(n/N) → Service life = 1/D

-----------------------------------------------------------------------------

In a real world scenario, flight data can be extracted from actual flight logs or test stand data. 

That data is then processed using python to categorize the UAV's mission profile and load spectrum.

The load spectrum data is used as inputs for the FEA process.

In FEA, a minimum and max load are obtained using the load spectrum data.

The max and min principal stresses are used to find an equivalent stress per MMPDS and the applicable material. 

The equivalent stress is used to find the fatigue life using Miner's Rule.

Voila... You have your recommended service life.

-----------------------------------------------------------------------------

Verification

Headless pipeline reproduces FreeCAD's own hot-spot stresses on the untouched deck <!-- % -->
σ(10 N)/σ(1 N) = 10.000 — proves linearity; solver loop and unit-load scaling are equivalent
C3D10 elements, locally refined and converged at the critical fillet <!-- % -->

-----------------------------------------------------------------------------

Results



D = … per design life → service life = … flight hours
Damage drivers: ground-air-ground and gust cycles — rare large cycles out-damage millions of small ones

-----------------------------------------------------------------------------

Why It Matters

The expensive part of fatigue work isn't the math — it's the manual FEA bookkeeping. This automates it using the real substantiation methodology (adapted from the author's helicopter equipment fatigue experience), with a CSV artifact at every stage so the final answer traces back to counted cycles and solver output.

-----------------------------------------------------------------------------

Limitations & Future Work

Synthetic single-mission data · thrust-only quasi-static loading · single-segment S-N · no scatter factor (a certifiable life divides by 4–8×, FAA AC 27-1B). Future: mission mix, scatter/reliability treatment, PSD-based spectral fatigue (Dirlik), test correlation.
