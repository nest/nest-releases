# Release notes for NEST 2.0.x

## Version 2.0.0 final (2012-10-23)

* New neuron models: `ginzburg_neuron` [1] and `izhikevich` [2]
* Fixed build failure when compiling with GCC 4.6+
* Tests that use GSL now return success if NEST is compiled without GSL support
* Fixed regression that restricted the number of neurons connected to `spike_detectors` and `music_out_proxys` to 128
* Fixed typo in Topology User Manual
* Changed license from proprietary NEST License to GNU GPL v2 or later

[1] Iris Ginzburg, Haim Sompolinsky. Theory of correlations in stochastic neural networks (1994). PRE 50(4) p. 3171
[2] Eugene Izhikevich, Simple Model of Spiking Neurons, IEEE Transactions on Neural Networks (2003) 14:1569-1572

## Version 2.0 RC4 (2011-03-24)

* Fixed installation of the topology testsuite.
* `spike_generator` now checks all spike times for grid-compatibility when setting them. This avoids crashes during simulations in multi-threaded scenarios.
* The Hill-Tononi model (`ht_neuron`) now is absolutely refractory while the spike current ''g_spike'' is active. This is the correct behavior after careful re-reading of the paper and inspection of the original source code.
* PyNEST now works again with Python 2.4
* MyModule now builds and installs again

## Version 2.0 RC3 (2011-02-25)

### Improvements and bug fixes

* Multiple `multimeters`s can now be connected to a neuron (thanks to Pierre Yger for pointing out this problem).
* `multimeter` now has an accumulator mode, summing values across all recorded nodes for each time step.
* New SLI functions DoubleQ, IntegerQ to check numbers for being of type double, integer, respectively.
* New SLI function FiniteQ to check if an object is a finite number.
* Set the `precise_times` flag of `spike_detector` to `true` and `precision` to 15 if precise spiking neuron model are in the network and the user did not set this properties explicitly.
* PyNEST `voltage_trace` now works together with multimeter.
* PyNEST can now be built without NumPy. Please note, however, that this leads to a certain performance penalty.

### Documentation

* New and extended documentation on parallel and distributed computing.
* Small corrections and new examples in the topology user manual.

### Bug fixes

* Re-added MUSIC support, which got lost in an earlier release. See Using NEST with MUSIC.
* SLI functions `shrink` and `reserve` now work as expected.
* PyNEST testsuite now correctly shows the number of errors ''and'' failures.
* PyNEST `raster_plot` / `voltage_trace` now correctly handles parameters and grayscale plotting again.

### Known issues

* Two test of the built-in testsuite currently fail. The reason for this is known and this will be fixed soon in RC4.

## Version 2.0 RC2 (2010-11-26)

* Topology: Change semantics of anchor supplied with kernel functions: anchor now gives displacement relative to driver neuron, as for free masks.
* Topology plotting functions improved now respect mask and kernel anchors and have a more flexible interface
* New Topology user manual (PDF: Topology_UserManual_2.0-RC2.pdf)
* Modified topology_func() to be Python 2.5 compatible
* Corrected memory leak in spike_generator for sending weighted spikes.

## Version 2.0 RC1 (2010-11-11)

### New functionality

* New models `stdp_dopamine_synapse` and `volume_transmitter` to add support for dopamine-modulated spike-timing dependent plasticity, as described in Potjans W, Morrison A and Diesmann M (2010) ''Enabling functional neural circuit simulations with distributed computing of neuromodulated plasticity''',  [http://www.frontiersin.org/computational_neuroscience/10.3389/fncom.2010.00141/abstract Front. Comput. Neurosci. 4:141].
* New model: `iaf_psc_exp_ps`, a leaky integrate-and-fire neuron with exponential postsynaptic currents (canoncial implementation). It uses the bisectioning method for the approximation of the threshold crossing. See Hanuschkin A, Kunkel S, Helias M, Morrison A and Diesmann M (2010) ''A general and efficient method for incorporating precise spike times in globally time-driven simulations'', [http://dx.doi.org/10.3389/fninf.2010.00113 Front. Neuroinform 4:113].
* ConnPlotter is now part of the official NEST release. For more information on ConnPlotter, see Nordlie E, Plesser HE (2009) ''Visualizing neuronal network connectivity with connectivity pattern tables'', [http://dx.doi.org/10.3389/neuro.11.039.2009 Front Neuroinform 3:39].
* NEST now supports runtime interoperability with other applications via the [http://software.incf.org/software/music MUSIC interface]. For details on the design, the usage and the performance of the interface, see Djurfeldt M, Hjorth J, Eppler JM, Dudani N, Helias M, Potjans TC, Bhalla US, Diesmann M, Kotaleski JH and Ekeberg Ö (2010) ''Run-Time Interoperability Between Neuronal Network Simulators Based on the MUSIC Framework'', [http://dx.doi.org/10.1007/s12021-010-9064-z Neuroinformatics 8(1):43-60]. Please note that since the publication of the article, we changed the name of `music_{in,out}_proxy` to `music_event_{in,out}_proxy` and added support for receiving continous data and string messages. For examples, please see Using NEST with MUSIC.
* New model: `sli_neuron` for rapid prototyping of neuron and device models in SLI.
* New SLI function `Rotate` for arrays <pre>SLI ] [-2 -1 0 1 2] 2 Rotate ==&#10;[0 1 2 -2 -1]&#10;SLI ] [-2 -1 0 1 2] -2 Rotate ==&#10;[1 2 -2 -1 0]</pre>
* New SLI functions for strings: `ToUppercase`, `ToLowercase` <pre>SLI ] (MiXeD CaSe) ToUppercase dup ==&#10;(MIXED CASE)&#10;SLI [1] ToLowercase ==&#10;(mixed case)</pre>
* SLI function `Sort` now also supports arrays of strings <pre>SLI ] [(a) (A) (b) (C)] Sort == &#10;[(A) (C) (a) (b)]</pre>
* New SLI functions for getting local nodes: `GetLocalLeaves`, `GetLocalNodes`, `GetLocalChildren`
* New SLI function `ScanThread` to execute a function to corresponding elements of n arrays.
* New SLI function StringQ for string type testing.
* The `ht_generator` has been renamed `smp_generator` (sinusoidally modulate Poisson generator).

### Improvements

* Devices now collect data across all threads. This eliminates the need for extended addresses and loops over the different threads to collect the data internally. The following listing illustrates the old way of retrieving the membrane potential from a `voltmeter` in a simulation with 4 threads: <pre>[0 1 2 3] { vm GetAddress exch append GetStatus /events get /V_m get} Map Flatten</pre> The new way to retrieve the data looks like this: <pre>vm GetStatus /events get /V_m get</pre> or using the hierarchical get operator in short: <pre>vm [/events /V_m] get </pre> The only change for user code is that the property `filename` of all recording devices has been renamed to `filenames` and contains one filename per thread now.
* All neuron models now support `multimeter`. Support for all other analog recorders has been removed. `voltmeter` has been added as `multimeter` preconfigured to record voltages.
** User documentation: Analog recording with multimeter
** Developer documentation: Converting models to support multimeter recording
* NEST now invokes `MPI_Abort()` in case of errors in the script. This means that errors terminate all connected processes, which minimizes the danger of dead-locks on the cluster.
* Improved PyNEST's `RandomConvergentConnect` and `RandomDivergentConnect` by moving loops to SLI and by adding options to allow/disallow autapses/multapses from within PyNEST. See PyNEST#Connections for help on the usage.
* First tests for MPI. If you are using distributed computing and want to run the testsuite, you have to add the function `/mpirun` to the NEST config file `~/.nestrc`. Please see $PREFIX/share/doc/nest/examples/nestrc.sli for an example implementation.
* Cleanup of the benchmarks in Brette_et_al to conform to the new API and produce consistent results.
* PyNEST submodule `voltage_trace` now supports voltmeters that recorded from multiple neurons.
* Added raw count histogram to `correlation_detector`.
* GSL error tolerance now configurable for aeif models, and models throw exceptions on implausible state values
* Improved performance for communicating off-grid spikes in distributed setups

#### Improved Topology Module

The Topology Module has been improved, including some changes that change the behavior.
* The Topology PyNEST user interface is now much closer to the general PyNEST interface.
* `ConnectLayer` is now called `ConnectLayers`
* All `nest.topology` functions now require lists of GIDs as input, not "naked" GIDs
* There are a number of new functions in `nest.topology`, I tried to write good doc strings for them
* For grid based layers (ie those with `/rows` and `/columns`), '''we have changed the definition''' of `extent`: Previously, nodes were placed on the edges of the extent, so if you had an extend of 2 (in x-direction) and 3 nodes, these had x-coordinates -1, 0, 1. The grid constant was extent/(num_nodes - 1). Now, we define the grid constant as extent/num\_nodes, center the nodes about 0 and thus add a space of half a grid constant between the outermost nodes and the boundary of the extent. If you want three nodes at -1,0,1 you thus have to set the extent to 3, i.e., stretching from -1.5 to 1.5. The main reason for this change was that topology always added this padding silently when you used periodic boundary conditions (otherwise, neurons are the left and right edge would have been in identical locations, not what one wants).
* Functions computing connection probabilities, weights and delays as functions of distance between source and target nodes now handle periodic boudary conditions correctly.
* Masks with a diameter larger than the diameter of the layer they are applied to are now prohibited by default. This avoids multiple connections when masks overwrap.

### Documentation

* Updated the PyNEST documentation on the NEST homepage to reflect the current API.
* Updated all pages related to the helpdesk. This includes a face-lift for all pages as well as the addition of letter links to the alphabetical index.
* Updated the SLI tutorial on the homepage to reflect the current API.
* Updated the example networks on the homepage and added the resulting plots.
* Updated and extended the role and seeding of random number generators in NEST. See Random numbers in NEST.
* Added example for repeated stimulation using 'origin': pynest/examples/repeated_stimulation.py
* Added example of how to record data from multimeter to file: pynest/examples/multimeter.py
* Added documentation for using NEST with MUSIC.

### Bug fixes

* Setting impossible values for number of virtual processes and threads is now prohibited.
* `step_current_generator` now keeps its last value to the end of simulation.
* Negative rates are now prohibited in `poisson_generator_ps`.
* Fixed several possible crashes in `ht_neuron`.
* Fixed static destruction problem in `iaf_cond_alpha_mc`.
