# Release notes for NEST 2.2.x

NEST 2.2 contains substantial improvements and many new features. The most important ones are:

* Better speed, scaling, and memory footprint
* Support for connection set algebra (CSA)
* Data driven network generation 

## NEST 2.2.2 Release Notes

* New models and devices: 
  * `iaf_psc_alpha_multisynapse`
  * `iaf_psc_exp_multisynapse` 
  * `mcculloch_pitts_neuron`
  * `ginzburg_neuron`
  * `spin_detector`
  * `sinusoidal_gamma_generator`
* Updated models and devices:
  * `smp_generator` is replaced by `sinusoidal_poisson_generator`, which sends an '''individual''' spike train to each of its targets by default; be wary when updating your code! In order to replicate the old behavior of the `smp_generator` (all targets receive the same spike train), set `/individual_spike_trains` to `false` on the `sinusoidal_poisson_generator` model before creating a generator node.
  * `iaf_psc_alpha`: bugfixes
  * `stdp_dopamine_synapse`: bugfixes
* PyNEST: fixes to the `DataConnect()` interface
* Topology: 
  * more efficient `GetTargetNodes()` implementation
  * lognormal distribution for parameter values
* Various bugfixes to the kernel, SLI, build system and other areas

## NEST 2.2.1 Release Notes

NEST 2.2.1 is primarily a bugfix release, resolving several bugs in the build system, PyNEST, topology module and built-in models. NEST 2.2.1 has been verified to be compatible with the latest released PyNN 0.7.5.

## NEST 2.2.0 Release Notes

### Better speed, scaling, and memory footprint

Many of NEST's major data structures have been re-written to improve speed, scalability and memory footprint. 

The most significant changes concern the simulation kernel and the way it represents networks internally. As a result, NEST has a considerably lower memory consumption and improved scaling, in particular when using a large number of cores. The full details and theory behind these changes are published in two papers:

* Helias et al. Front. Neuroinform. (2012) http://dx.doi.org/10.3389/fninf.2012.00026
* Kunkel et al. Front. Neuroinform. (2012) http://dx.doi.org/10.3389/fninf.2011.00035

* Many connect functions now use OpenMP to connect neurons in parallel.
* OpenMP has also replaced Pthreads as a default for multi-threaded simulations, yielding better scaling.
* SLI, the built-in simulation language interpreter of NEST, is now up to 5 times faster.
* The topology library has been re-written, greatly improving its speed and reducing memory requirements.

### Support for connection set algebra (CSA)

NEST 2.2.0 supports the Connection Set Algebra by Mikael Djurfeldt (http://dx.doi.org/10.1007/s12021-012-9146-1). The Connection Set Algebra is a powerful notation that allows to formulate complex network architectures in a concise manner. The following examples illustrates how the CSA is used to connect a random network:

    import csa
    import nest
    
    # Random connectivity with connection probability 0.1
    cs = csa.random(0.1)
    
    # Create two neuron populations
    pop1 = nest.LayoutNetwork("iaf_neuron", [16])
    pop2 = nest.LayoutNetwork("iaf_neuron", [16])
    
    # Connect them using the connection generator cs
    nest.CGConnect (pop0, pop1, cs)

### Data driven network generation

NEST 2.2.0 has a new function `DataConnect` which allows the efficient connection and parametrization of connections from connection data. `DataConnect` will efficiently create and parameterize synapses.

The new function `GetConnections` allows to efficiently retrieve the afferent and efferent connections of a neuron or the connections between groups of neurons. The combination of `GetConnections` and `DataConnect` allows users to retrieve, save, and restore the synaptic state of a network. This is particularly useful in models with synaptic plasticity and learning.

### Topology library

The topology library supports the creation of spatially organized networks, e.g. for models of the visual system. NEST 2.2.0 supports 3-dimensional networks, where neurons are placed in a volume rather than on a sheet. The following example shows how to connect a layer to itself with a Gaussian distance-dependent probability profile:

    import nest
    import nest.topology as topo
    
    # Specify layer structure and connectivity profile
    layer_spec = {'columns': 30, 'rows': 30, 'extent': [3.0, 3.0], 'elements': 'iaf_neuron'} 
    conn_spec = {'connection_type': 'convergent',
                 'mask': {'circular': {'radius': 3.0}},
                 'kernel': {'gaussian': {'p_center': 1.0, 'sigma': 0.5}},
                 'weights': 1.0, 'delays': 1.0}
    
    # Create layer and connections
    layer = topo.CreateLayer(layer_spec)
    topo.ConnectLayers(layer, layer, conn_spec)
    
    # Visualize all targets of neuron at center of layer
    topo.PlotTargets(topo.FindCenterElement(layer), layer)

There is also a new API to add user defined connection kernels. Please refer to the updated user manual and examples for more details.

### Detailed list of changes

* Kernel and PyNEST changes
  * Major improvements to the memory consumption and scaling properties of the simulation kernel as described in Helias et al. (2012) and Kunkel et al. (2012).
  * NEST now supports the connection generator interface, an interface allowing external modules to generate connectivity, see http://software.incf.org/software/libneurosim.
  * Support for the Connection Set Algebra (CSA) by Mikael Djurfeldt has been added, see http://software.incf.org/software/csa and http://dx.doi.org/10.3389/conf.fninf.2011.08.00085.
  * The new command `GetConnections` allows the fast retrieval of connections and will replace the slow and memory intensive `FindConnections`; `FindConnections` is deprecated and will be removed in future releases.
  * The new command `DataConnect` allows to efficiently create network connections from data, e.g. when synapse parameters are explicitly given.
  * Connection objects are now represented as Python lists or NumPy arrays; code which relies on the old connection dictionaries will have to be changed.
  * The function `GetNodes` has been removed, one has to explicitly use either `GetLocalNodes`, or `GetGlobalNodes`.
  * Support for node addresses has been removed and with it the functions `GetAddress` and `GetGID`.
  * Threading support for OpenMP has been added and is the default now.
  * Many connect routines and node calibration are now parallel, using OpenMP.
  * New function `CGConnect` for connecting neurons using connection generators (see doc/conngen.txt).
  * New function `abort`, which terminates NEST and all its MPI processes without deadlocks.
* Topology module changes
  * Major rewrite which resulted in improved performance and reduced memory requirements for freely placed neurons.
  * Topology now supports 3-dimensional layers.
  * New API for adding your own kernel functions.
  * 'Nested' layer layout (subnets within subnets) is no longer supported; this was previously discouraged for performance reasons.
  * Composite layers (layers with multiple nodes per position) no longer contain subnets.
  * Semantics of `GetElement` have changed, in particular a list of GIDs is returned for a composite layer, where previously a single subnet GID would be returned.
  * `GetPosition`, `Displacement` and `Distance` now only work for nodes local to the current MPI process.
  * See the updated Topology User Manual for details.
* SLI Interpreter improvements
  * The SLI Interpreter has been optimized for speed and memory; in particular handling and lookup of names is much faster now.
  * SLI now supports fixed size vectors of doubles and integers. The new types are called `IntVector` (/intvectortype) and `DoubleVector` (/doublevectortype). 
  * NumPy arrays are automatically converted to SLI vectors to conserve memory and CPU time.
  * SLI has new functions `arange`, `zeros` and `ones` to easily create vectors.
  * The vector types support all common math operations.
  * New functions `DictQ` and `SubnetQ` to test the argument types for being a dictionary or a subnet, respectively.
* Miscellaneous changes
  * The installation prefix should now be given explicitly, since installing to the default `/usr/local` is strongly discouraged.
  * Substantial documentation updates; a large number of broken documentation cross-references has been resolved.
  * Fixed numerical instabilities of the AdEx models (`aeiaf_cond_exp` and `aeiaf_cond_alpha`).
  * New significantly faster binomial random number generator replaced the previous implementation in librandom (#390).
  * New wrapper for the GSL binomial random deviate generator under the name `gsl_binomial`.
  * Support for IBM BlueGene and K supercomputers (configure with --enable-bluegene=l/p/q).
  * The command `setenvironment` has been removed; this functionality was broken on Mac OS X for quite some time.
  * Much improved test coverage for SLI, PyNEST and MPI in particular.
  * Many new SLI and PyNEST examples and updates for the existing ones.
  * Code quality of NEST and all examples is continuously monitored now using CI (http://dx.doi.org/10.3389/fninf.2012.00031).

### Known issues

* The simulation progress indicator does not work with OpenMP and some error messages are unreadable.
* On multiarch systems (i.e. 64-bit Red Hat Linux) one has to manually move all PyNEST-related files from $PREFIX/lib/python2.6/site-packages to $PREFIX/lib64/python2.6/site-packages; this will be resolved in next releases. This also breaks `make installcheck`.
* Known compatibility problems with PyNN are now fixed in the 0.7 svn branch, which will be released early next year. 

### Contributors

* Sacha van Albada
* Giuseppe Chindemi
* Moritz Deger
* Markus Diesmann
* Mikael Djurfeldt
* Håkon Enger
* Jochen Martin Eppler
* Marc-Oliver Gewaltig
* Moritz Helias
* Susanne Kunkel
* Abigail Morrison
* Eilif Muller
* Hans Ekkehard Plesser
* Sven Schrader
* Tom Tetzlaff
* Yury V. Zaytsev
