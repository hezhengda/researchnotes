Quantum Espresso
================

Error in routine stres_hub (1): non-symmetric stress contribution
-----------------------------------------------------------------

This error is caused by using :code:`vc-relax`, :code:`tstress` and :code:`mixing_fixed_ns` together.

**Solution**: Just use :code:`scf` for calculation, and set :code:`tstress=False`, also with :code:`mixing_fixed_ns = 50` as a starting point. The error will be gone and you will get more accurate initial wavefunction for your further simulations.
