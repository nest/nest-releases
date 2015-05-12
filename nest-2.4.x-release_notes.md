# Release notes for NEST 2.4.x

NEST 2.4 contains many new features, neuron models and general improvements. The most important ones are:

* A Python 3.0 compatible re-implementation of the Python interface.
* A new framework for setting up connections and their parameters.
* A new spike detection mode with greatly improved I/O performance.
* More flexible framework for working with random distributions.
* Support for connectivity-generating libraries through libneurosim.

## NEST 2.4.2 Release Notes

This is a bugfix and maintenance release for 2.4.1. Users are advised to update their installation as soon as possible. The release contains the following improvements and fixes for minor bugs:
* Updated MUSIC examples to use the new Connect framework and allow `music_channel` as an alias for `receptor_type` during connection setup.
* Turned PyNEST deprecation warning into a decorator and beautified URLs in the output to allow IPython Notebooks to parse and link them properly.
* Improved performance of SLI functions `token_s` and `symbol_s` by a factor of 1000 by replacing a loop by an explicit function call.
* Made CyNEST handle parameters passed as Unicode strings in all Python versions by using `basestring` instead of `str` for Python versions < 3.
* Fixed segmentation fault produced by additional command line arguments for Python when PyNEST was imported.

### Contributors for this release

* Hannah Bos
* Oliver Breitwieser
* Jochen Martin Eppler
* Jan Hahne 
* Frank Michler
* Alex Peyser

## NEST 2.4.1 Release Notes

This is a bugfix and maintenance release for 2.4.0, in which the Topology Module contains a bug which leads to the creation of too few connections (N/num_threads instead of N) when using the divergent connection_type in ConnectLayers in a multi-threaded mode. Users are advised to update their installation as soon as possible.

Here's a detailed list of changes:

* Fix multi-threaded creation of divergent connections in Topology Module and add a regression test for this.
* Add `SetNumRecProcesses` to PyNEST to enable the Global spike detection mode.
* Update Toplogy Manual for NEST 2.4 and to be Py3k compatible.
* Use proper syntax for getting default synapse model in the `ConnectionGenerator` module.
* Update the reference in the `README.txt` of the microcircuit model by Potjans&Diesmann (http://dx.doi.org/10.1093/cercor/bhs358).
* Remove special characters from `iaf_chs_2007` that confused the copyright-header checker.

## NEST 2.4.0 Release Notes

<span style="color:#990000">The Topology Module of this version of NEST contains a bug which leads to the creation of too few connections (N/num_threads instead of N) when using the divergent connection_type in ConnectLayers in a multi-threaded mode. Please update your installation at least to NEST 2.4.1.</span>

### Re-implementation of PyNEST

The Python interface to NEST (PyNEST) has been re-implemented from scratch. The new implementation (CyNEST) is based on Cython and provides the following improvements over the previous version:

* Support for Python 2.6, 2.7, and 3.x
* Installation now works properly on all platforms
* Better extensibility and maintainability

See Zaytsev & Morrison (2014, http://dx.doi.org/10.3389/fninf.2014.00023) for details of the new interface.

Minor changes to PyNEST are:

* PyNEST's visualization.plot_network() can now create PNGs or PDFs
* Importing SciPy after NEST does not cause segfaults anymore

Please note that CyNEST requires Cython version 0.19.2 or higher to regenerate the source files (only for developers). This can be installed using `pip install --user --upgrade cython`.

### New routines for setting up connectivity

The `Connect` function of NEST has been completely re-written in order to support a more flexible setup of connectivity. In particular this means that connectivity is now specified using a rule (all-to-all, one-to-one, fixed-indegree, fixed-outdegree, fixed-total-number, or pairwise-bernoulli) and a dictionary with synapse parameters. All connection parameters can now be randomized already during connection setup by specifying the random distribution.

In addition to more flexibility, the new framework is faster due to a massive clean-up of the code and OpenMP parallelization throughout.

Please see Connection Management for more information about the new connection routines and how to transition your simulation scripts from the old functions to the new framework.

If you are using SLI, all your scripts will be working without changes. PyNEST's `Connect` function was renamed to `OneToOneConnect`. `Connect` now uses the new semantics as explained in the documentation on the website of the NEST Initiative.

The new `Connect` function is provided as a technology preview. This means that while the basic user interface will stay, the functionality and performance will be further extended in future releases.

### Global spike detector

In parallel simulations on very large machines without local disks, saving recorded spikes often leads to performance problems, because each spike detector will write out one file per virtual process. This problem is now ameliorated by a new spike detection mode, which can be activated by the users of such machines. In the new mode, all spike detectors are allocated on a distict set of processes.

See Global spike detection mode for details on the new recording mode and how to activate it.

### Random number generators and distributions

In the process of implementing the new connection routines, we found several performance problems in NEST's librandom and inconsistencies with PyNN (http://neuralensemble.org/PyNN/). To solve these problems, we changed several aspects of librandom.

The random deviate generators in NEST have been extended and modified to support random initialisation of synapse parameters for the new connection routine, and to achieve greater similarity between NEST and PyNN. For most users, these changes only add new features. Scripts using `uniformint` or `normal_clipped` need to be adapted as explained in Random numbers in NEST

To achieve consistent results for both global and local spike detector mode, the seed of the global RNG was changed from n_vp+1 to 0. To allow the comparison of new results with those obtained with earlier versions of NEST, this seed has to be set to n_vp+1 manually. See Global spike detection mode for details.

If NEST is compiled with support for GSL, `gsl_rng_knuth2002` is now used instead of `knuthlfg` as per-thread and global random number generator (RNG) to improve performance. Both RNGs create identical sequences, so new simulation results are still comparable to old ones.

#### Support for connectivity-generating libraries

In NEST 2.4, the direct implementation of the ConnectionGenerator interface has been replaced in favor of support for libneurosim (see http://software.incf.org/software/libneurosim). This allows to couple all connectivity-generating libraries (e.g. the Connection-set algebra; http://software.incf.org/software/csa) with NEST, which support libneurosim.

See Djurfeldt et al. (2014, http://dx.doi.org/10.3389/fninf.2014.00043) for details about the new interface.

### New models and model improvements

* `stdp_facetshw_synapse` mimics the restrictions of the neuromorphic HMF developed in the context of FACETS and BrainScaleS.
* `aeif_cond_alpha_RK5`, which is independent of the GSL by using a custom version of the RK5 solver.
* `aeif_cond_alpha_multisynapse` is a variant of the exponential integrate-and-fire model with multiple synaptic time constants.
* `quantal_std_synapse` is a probabilistic synapse model with short term plasticity (Fuhrmann et al. 2002, http://dx.doi.org/10.1152/jn.00258.2001).
* `iaf_chs_2007` is a spike-response model (Carandini et al. 2007, http://dx.doi.org/10.1167/7.14.20).
* `iaf_chxk_2008` is a conductance based leaky integrate-and-fire neuron model (Casti et al. 2008, http://dx.doi.org/10.1007/s10827-007-0053-7).
* Weighted excitatory and inhibitory input spikes are recordable in `iaf_psc_exp` and `iaf_psc_delta`.
* Excitatory and inhibitory synaptic currents are recordable in `iaf_psc_exp` and `iaf_psc_alpha`.
* Recovery variable `U_m` is recordable in `izhikevich`.
* `spike_detector` now throws an error for spike times of 0.

### Improved quality and documentation

Compared to the 2.2.2 release, we have increased the number of unit tests from 270 to 418. This improvement guarantees the continued quality of NEST on the computer of the user and allows to find and fix problems quickly. To run the testsuite after installation, run `make installcheck` from the build directory.

Many examples have been updated and extended to demonstate the usage of NEST. In addition, we have added the full microcircuituit model by Potjans & Diesmann (2014, http://dx.doi.org/10.1093/cercor/bhs358).

In order to prevent memory leaks by forgotten arguments on the SLI stack, PyNEST now can do stack checking and runs its own testsuite in this mode.

### New SLI functions and improvements

* Added an `eval` function to execute SLI code in strings.
* `SetMaxBuffered` which sets the max buffered parameter of a MUSIC input port.
* `eq_dv` and `eq_iv` to test double and int vector equality.
* `cva` applied to an array now leaves array elements as they are if they can not be converted to an array.
* `round` not converts to double if applied on an integer.
* `cvi` is identity operation for integers.
* `Take` now also works for strings.

### Deprecated functionality

The following SLI/PyNEST functions are obsolete will be removed in the next version:

* The functionality of the functions `(Random)DivergentConnect` and `(Random)ConvergentConnect` is now integrated into `Connect`. See Connection Management for documentation on how to convert your scripts to the new syntax.
* The old connect framework in PyNEST (`OneToOneConnect`) will be removed in favor of the new connect framework.
* `FindConnections` is superseded by `GetConnections`.
* Support for plain POSIX threads will be removed in favor of OpenMP.

### Contributors since NEST 2.2

* Claudia Bachmann
* Hannah Bos
* Ekatarina Brocke
* David Dahmen
* Moritz Deger
* Markus Diesmann
* Mikael Djurfeldt
* Håkon Enger
* Jochen Martin Eppler
* Marc-Oliver Gewaltig
* Moritz Helias
* Tammo Ippen
* Jakob J. Jordan
* Susanne Kunkel
* Abigail Morrison
* Mikael Naveau
* Daniel Peppicelli
* Alexander Peyser
* Thomas Pfeil
* Hans Ekkehard Plesser
* Wolfram Schenck
* Maximilian Schmidt
* Jannis Schuecker
* Sacha van Albada
* Yury V. Zaytsev
