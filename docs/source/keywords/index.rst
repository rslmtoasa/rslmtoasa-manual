.. _keywords/index:

=======================================
Input Keywords Reference
=======================================

Complete alphabetical and categorical listing of all RS-LMTO-ASA input parameters.

Quick Navigation
================

.. toctree::
   :maxdepth: 2

   control_parameters
   lattice_geometry
   basis_parameters
   energy_mesh
   scf_settings
   exchange_correlation
   output_options

Parameters by Namelist
======================

**&control**

Main calculation control parameters. See :ref:`keywords/control_parameters`.

**&lattice**

Crystal structure and geometry. See :ref:`keywords/lattice_geometry`.

**&energy**

Energy mesh and Fermi level settings. See :ref:`keywords/energy_mesh`.

**&self**

Self-consistent field convergence. See :ref:`keywords/scf_settings`.

**&element**

Atomic parameters (element database).

**&par**

LMTO tight-binding potential parameters. See :ref:`keywords/basis_parameters`.

Quick Reference Table
=====================

Most commonly used parameters:

.. list-table::
   :widths: 20 15 40
   :header-rows: 1

   * - Parameter
     - Type
     - Typical Value
   * - nsp
     - int
     - 1 (scalar-rel), 2 (collinear)
   * - max_iterations
     - int
     - 50-100
   * - llsp
     - int
     - 60-100
   * - lld
     - int
     - 60-100
   * - alat
     - real
     - system-dependent (Å)
   * - dq_tol
     - real
     - 1e-5 to 1e-4
   * - mixing
     - str
     - 'linear' or 'broyden'
   * - alpha
     - real
     - 0.3-0.5
   * - channels_ldos
     - int
     - 300-500

Alphabetical Listing (All Parameters)
=====================================

**A**

- ``alpha`` - Mixing parameter (density mixing)
- ``alat`` - Lattice constant

**B**

- ``broyden_history`` - Number of previous densities to keep (Broyden mixing)

**C**

- ``center_band`` - LMTO orbital center energy
- ``channels_ldos`` - Number of energy mesh points

**D**

- ``dq_tol`` - Charge density convergence tolerance

**E**

- ``energy_max`` - Upper energy limit
- ``energy_min`` - Lower energy limit

**F**

- ``fermi`` - Fermi energy (initial guess or fixed)
- ``fix_fermi`` - Fix Fermi level to value

**L**

- ``lld`` - Recursion cutoff for d electrons
- ``llsp`` - Recursion cutoff for s/p electrons
- ``lmax`` - Maximum orbital angular momentum

**M**

- ``max_iterations`` - Maximum SCF iterations
- ``mixing`` - Density mixing type

**N**

- ``nbulk`` - Number of bulk atoms in cluster
- ``nlim`` - Cluster size control
- ``nsp`` - Type of calculation (relativistic, collinear/non-collinear)
- ``nx, ny, nz`` - Cluster dimensions in lattice coordinates

**O**

- ``idos`` - LDOS output type
- ``orb_pol`` - Include orbital polarization

**R**

- ``random_vec_num`` - Number of stochastic vectors for moment calculation
- ``r2`` - Cluster cutoff radius squared

**S**

- ``sws`` - Wigner-Seitz radius (atomic sphere)

**T**

- ``temperature`` - Electronic temperature (Kelvin)
- ``txc`` - Exchange-correlation functional type

**V**

- ``verbose`` - Enable verbose output
- ``ws_r`` - Wigner-Seitz radius (input format)

**W**

- ``width_band`` - LMTO bandwidth parameter

See Full Reference Pages
========================

For detailed descriptions, constraints, and examples, see:

- :ref:`keywords/control_parameters` - Calculation control (nsp, max_iterations, etc.)
- :ref:`keywords/lattice_geometry` - Structure (alat, lattice type, cluster size)
- :ref:`keywords/basis_parameters` - LMTO basis (lmax, center_band, width_band)
- :ref:`keywords/energy_mesh` - Energy integration (channels_ldos, energy range)
- :ref:`keywords/scf_settings` - SCF convergence (mixing, dq_tol, max_iterations)
- :ref:`keywords/exchange_correlation` - XC functional (txc)
- :ref:`keywords/output_options` - Output control (post_processing, idos)

Cross-References
================

Each parameter page includes:

- **Purpose** - Physical meaning and use
- **Allowed values** - Valid ranges, defaults
- **Units** - What units are expected
- **Example** - Typical usage
- **Related parameters** - Cross-links to related options
- **Code location** - Where in source code it's used

Provenance
==========

Parameter definitions and defaults come from:

- ``source/control.f90`` - Control parameters type and defaults
- ``source/lattice.f90`` - Lattice parameters
- ``source/energy.f90`` - Energy mesh parameters
- ``source/self.f90`` - SCF parameters
- ``source/namelist_generator.f90`` - Namelist generation utilities
- Example files: ``example/*/*.nml`` - Typical values

See Also
========

- :ref:`user_guide/input_files` - Input file syntax and format
- :doc:`../user_guide/examples` - Worked examples with parameter settings
- :doc:`../theory/scf_cycle` - Understanding SCF-related parameters
- :doc:`../theory/recursion_method` - Recursion cutoff parameters
