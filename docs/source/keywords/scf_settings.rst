.. _keywords/scf_settings:

=======================================
SCF Convergence Parameters (&self)
=======================================

Overview
========

The ``&self`` namelist controls self-consistent field (SCF) loop settings,
including density mixing, convergence criteria, and iteration limits.

Parameters
==========

mixtype
-------

**Type:** Character string

**Purpose:** Type of density mixing algorithm

**Allowed values:** 'linear', 'broyden'

**Default:** 'linear'

**Example:**

.. code-block:: fortran

   mixtype = 'broyden'

**Linear Mixing:**

.. math::

   n^{(i+1/2)} = \beta n_{\text{new}}^{(i+1)} + (1-\beta) n_{\text{old}}^{(i)}

- Simple and robust
- Can be slow to converge
- Good for unstable systems

**Broyden Mixing:**

Uses information from multiple previous densities to predict better mix.
Much faster convergence in most cases.

**Guidance:**

- For first calculation: try 'linear'
- For production: 'broyden' usually much better
- If 'broyden' oscillates: switch to 'linear'

**Related code:** ``source/mix.f90::type mix``

**See also:** :ref:`theory/scf_cycle`

beta
----

**Type:** Real

**Purpose:** Density mixing parameter

**Typical range:** 0.1-0.8

**Default:** 0.5

**Example:**

.. code-block:: fortran

   beta = 0.3  ! Conservative

**Meaning:**

- New density weight in mixing formula
- Larger β → larger step (faster but riskier)
- Smaller β → smaller step (slower but more stable)

**Guidance:**

- Start with β = 0.5
- If oscillating: reduce to 0.2-0.3
- If converging too slowly: increase to 0.6-0.7

**Notes:**

- For 'linear' mixing: critical parameter for convergence speed
- For 'broyden' mixing: less critical (auto-adjusted somewhat)

**Related code:** ``source/mix.f90::mix_charge()``

broyden_history
---------------

**Type:** Integer

**Purpose:** Number of previous densities kept for Broyden mixing

**Typical range:** 3-20

**Default:** 10

**Example:**

.. code-block:: fortran

   broyden_history = 8

**Meaning:**

- Higher number: uses more history → better prediction
- Lower number: faster but less information

**Guidance:**

- Start with 10 (reasonable balance)
- If convergence stalls: increase to 15-20
- If very slow: reduce to 5-8

**Notes:**

- Only used if ``mixtype = 'broyden'``
- Increases memory usage (~10% for large systems)
- Significant impact on convergence rate

**Related code:** ``source/mix.f90``

nstep (also in &self)
---------------------

**Type:** Integer

**Purpose:** Maximum number of SCF iterations

**Typical range:** 50-200

**Default:** 100

**Example:**

.. code-block:: fortran

   nstep = 80

**Notes:**

- Calculation stops when either iteration limit or convergence reached

conv_thr
--------

**Type:** Real (Ry)

**Purpose:** Charge density convergence tolerance

**Typical range:** 1e-6 to 1e-4

**Default:** 1e-5

**Example:**

.. code-block:: fortran

   conv_thr = 1.0e-5

**Meaning:**

.. math::

   \sum_i |q_i^{(n+1)} - q_i^{(n)}| < \text{conv\_thr}

**Guidance:**

- 1e-5 or better: Production quality
- 1e-4: Reasonable for most purposes
- 1e-6+: Overkill; diminishing returns

**Notes:**

- Smaller tolerance = more iterations needed
- Check in &control; may override &self value

SCF Convergence Behavior
========================

**Typical sequence:**

.. code-block:: text

   Iteration 1: ΔQ = 0.5 Ry
   Iteration 2: ΔQ = 0.1 Ry (5x better)
   Iteration 3: ΔQ = 0.02 Ry (5x better)
   ...
   Iteration 10: ΔQ = 0.5e-5 Ry (converged!)

**Exponential convergence (good):**

- ΔQ decreases by constant factor each iteration
- Typical factor: 3-10 (depends on mixing)

**Linear convergence (OK):**

- ΔQ decreases linearly
- Slower but still acceptable

**Oscillation (bad):**

- ΔQ alternates between small and large values
- Usually indicates mixing parameter too aggressive
- Solution: reduce β, switch to linear mixing

**Stagnation (very bad):**

- ΔQ plateaus at high value, doesn't decrease
- Usually indicates problem with Hamiltonian or structure
- Solution: check element/potential parameters, cluster geometry

Troubleshooting SCF Convergence
===============================

**Problem: Doesn't converge at all**

Solutions:
- Reduce β from 0.5 to 0.2
- Switch from 'broyden' to 'linear' mixing
- Check element parameters and lattice constant

**Problem: Converges slowly (100+ iterations)**

Solutions:
- For 'linear': increase α to 0.6-0.7
- Try 'broyden' mixing instead
- Check if problem inherently difficult (bad initial guess)

**Problem: Oscillates between states**

Solutions:
- Reduce α significantly (0.2-0.3)
- Use 'linear' mixing (more stable)
- Increase Broyden history (if using Broyden)

**Problem: Energy or forces unphysical**

This usually indicates non-convergent SCF.
Solutions:
- Ensure conv_thr is tight enough
- Check Hamiltonian parameters
- Verify cluster size adequate (increase r2)

Advanced SCF Options
====================

**Accelerated convergence techniques:**

Some codes support:

- Kerker mixing (k-dependent weighting)
- Pulay mixing (residual minimization)
- Anderson mixing

RS-LMTO currently supports linear and Broyden;
others may be added in future versions.

**Temperature effects:**

If non-zero temperature (``&energy``):

- SCF may converge more slowly
- Smeared Fermi surface → fewer oscillations
- Can sometimes help or hinder

**Constraint fields:**

For magnetic impurity or fixed-magnetization calculations:

- May need different mixing/convergence
- Less stability if constraints are too tight

Provenance
==========

SCF parameters defined in:

- **Type definition:** ``source/self.f90::type self``
- **Mixing:** ``source/mix.f90::type mix``, ``mix.f90::mix_charge()``
- **SCF loop:** ``source/self.f90::process()``
- **Convergence check:** ``source/self.f90::converge_scf()``

See Also
========

- :ref:`keywords/control_parameters` - Related: nsp, llsp, lld
- :ref:`theory/scf_cycle` - Detailed SCF theory
- :doc:`../user_guide/examples` - Example SCF parameter sets
- :doc:`../user_guide/input_files` - Input file format
