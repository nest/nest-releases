# Release notes for NEST 1.9.x

## Version 1.9.8718 (2010-06-09)

* The SLI scanner now correctly handles DOS encoded files (trac #431)
* Frozen nodes now can handle incoming spikes correctly (SVN r8664)
* NEST now compiles with the Portland Group Compiler (SVN r8654)
* Added `multimeter` support to the models `iaf_psc_alpha`, `hh_cond_exp_traub`, `aeif_cond_exp`, `iaf_cond_exp_sfa_rr`, `iaf_cond_exp`, `iaf_psc_exp`, and `aeif_cond_alpha` (SVN r8607, SVN r8653)
* SLI now has a `mathexecutive` mode for infix notation (SVN r8644)
* Fixed bootstrap of example `MyModule` (SVN r8642)
* Fixed misleading use of `t_spike` in `ht_neuron` (SVN r8638)
* Fixed dependency tracking in `Makefile`s (SVN r8618)
* Removed support for IDL (trac #322, SVN r8617)
* `spike_detector` now also can record weights (SVN r8614)
* Improved `spike_generator`: `spike_weights` now allows sending weighted spikes (SVN r8612, SVN r8613)
* Progress report now includes the realtime factor of the simulation and actually runs from 0 to 100% (trac #391, SVNr8608)
* Testsuite now uses a temporary directory to store results (trac #347, SVN r8604)
* `data_path` and `data_prefix` can now be set via the environment variables `NEST_DATA_PATH` and `NEST_DATA_DIR` (SVN r8602)
* `ModelModule` is now called `ModelsModule` (SVN r8588)
* Topology is now a `SLIModule` and loaded by default (SVN r8584)
* Support for infix pure functions in SLI (SVN r8568)
* Build improvements for BlueGene/P (SVN r8563)

## Version 1.9.8558 (2010-01-05)

* PyNEST now correctly loads on MacOS Snow Leopard (SVN r8556)
* Portability improvements for MacOS 10.4 (SVN r8541)
* Configure now supports `--without-gsl` and `--with-gsl-prefix` is `--with-gsl` now (trac #270, trac #387)
* `ResetKernel` now correctly regenerates the `modeldict` (trac #333)
* SLI: `CreateMany` is now gone and `Creat` has additional arguments for number and parameters of the new nodes (trac #369)
* Improved error handling in `iaf_cond_alpha_mc` (trac #385)
* Loading modules now works more reliably (trac #366, trac #367)
* PyNEST: `voltage_trace` and `raster_plot` now work with current versions of Matplotlib (trac #372)

## Version 1.9.8498 (2009-11-16)

* New model: `mat2_psc_exp` based on Kobayashi et al. (2009) Front. Comput. Neurosci. 3:9. (doi:10.3389/neuro.10.009.2009)[http://dx.doi.org/10.3389/neuro.10.009.2009]
* Fix Python symbol lookup error when using OpenMPI (SVN r8491)
* New model: `iaf_tum_2000`, which is the same as the old `iaf_psc_exp` and still has two refractory times (SVN r8480)
* Removed second refractory time from `iaf_psc_exp` (SVN r8480)
* Fixed memory leak caused by `opendir` (SVN r8470)
* Topology: added functions for plotting connections (SVN r8461)
* Model documentation now explains what types of events neurons and devices can send/receive and synapses can transmit (SVN r8444)
* Removed CPEX from the communicator to improve performance and scaling when using OpenMPI over Infiniband (SVN r8442)
* PyNEST: new function `sli_func()` for easier wrapping of SLI (trac #370)
* Testscripts can now also be written in Python in addition to SLI
* New workflow for connection management (SVN r8411, see the documentation at Connection_Management)
* `pulse_packet_generator` draws a new spike volley for each target and supports overlapping pulse packets (trac #244, trac #359)

## Version 1.9.8401-2 (Bug fix release, 2009-09-10)

* Fixed a build error in `MyModule` example

## Version 1.9.8401-1 (Bug fix release, 2009-09-07)

* Install the PyNEST component of the topology

## Version 1.9.8401 (2009-08-24)

* PyNEST now handles numpy integer types equally (SVN r8390)
* Information on the `helpdesk` is now advertised more prominently
* The PyNEST component of the topology now is a submodule of PyNEST. Use `import nest.topology` to use it. (SVN r8352)
* Support for MPI-parallel testscripts in the testsuite
* New commandline switch `-c` to execute SLI code directly (trac #353)
* `LambertW` function replaces the fudge factor in the examples (SVN r8336)
* XDR support is now disabled (SVN r8328)
* Configure now supports `--without-pthread` (SVN r8322)
* Fixed some errors in the `brunel-delta-nest.py` example (trac #344)

## Version 1.9.8316 (2009-06-19)

* The topology manuals are now also installed (SVN r8309)
* Improved our build system to work better with all versions of libtool
* Fixed PyNEST example `brunel-delta-nest.py` (SVN r8313)
* New SLI command `test` to run the testsuite from within NEST (SVN r8305)

## Version 1.9.8300 (2009-06-11)

* Added information on the `helpdesk` to READMEs and help page (trac #331)
* PyNEST: convert Numpy array scalars properly to SLI datums

## Version 1.9.8291 (2009-06-10)

* Use the included libtool and libltdl versions in all cases
* Fixed PyNEST to install properly under Python 2.6

## Version 1.9.8276 (2009-06-05)

* Fixed build environment to work reliably with all versions of libtool

## Version 1.9.8257 (2009-05-28)

* New model: `parrot_neuron_ps` for precise spike times (SVN r8242)
* Some minor build fixes if `$DESTDIR` is set (SVN r8249)
* PyNEST now catches conversion errors from NumPy (SVN r8248)
* `hh_cond_exp_traub` now reports correct conductances (SVN r8253)
* To disable PyNEST, use `--without-python` instead of `--disable-pynest`

## Version 1.9.8236 (2009-04-23)

* `iaf_psc_alpha_canon` now accepts connections from `dc_generator` (trac #326)
* Fixed some examples (SVN r8148, SVN r8150, SVN r8153)
* Fixed bug calculating wrong `nu_th` in `brunel-alpha` scripts (SVN r8159)
* Fixed a bug in the PyNEST error handler (SVN r8163)
* `make installcheck` now also runs the PyNEST testsuite (SVN r8169, SVN r8173)
* PyNEST can now be initialized with `--quiet` (SVN r8176, SVN r8177)
* PyNEST: `voltage_trace` now can reconstruct times from the data (SVN r8202)
* New SLI function `SyncProcesses` to do exactly that (SVN r8203, SVN r8206)
* New models: Multicompartmental IAF neuron `iaf_cond_alpha_mc` (SVN r8207) and `multimeter` to record multiple quantities from neurons (SVN r8219)
* New test scripts for RNGs and distributed simulations

## Version 1.9.8128 (2009-03-13)

* The build problems of 1.9.8120 are fixed (SVN r8127)

## Version 1.9.8120 (2009-03-11)

''THIS VERSION WAS RETRACTED DUE TO BUILD PROBLEMS. USE 1.9.8128 INSTEAD''

* Improved unit tests and three new test scripts (SVN r8093)
* Automatic installation of testscripts from modules (SVN r8098)
* New SLI functions: `asin`, `acos` (SVN r8087), `Function` (SVN r8100)
* Building on BlueGene/L now also uses `regexp.h` (trac #272)
* PyNEST: `SetStatus` now handles `dict` and `string` arguments correctly (SVN r8095)
* PyNEST: We now also understand array selections like a[:,1] (SVN r8110)
* The SLI function `Sort` now also works on array of integers (SVN r8117)
* `RandomDivergentConnect` now has weight and delay arguments as has been documented for quite some time (SVN r8118)

## Version 1.9.8075 (2009-02-19)

* Membrane potential may now be initialized to arbitrary values (SVN r8050, trac #310)
* GNU Scientific Library v 1.11 or later recommended, required for `aeif_cond_alpha` (SVN r8046, trac #311)
* PyNEST: Error reporting now really works (trac #309)
* New model: `hh_cond_exp_traub` (SVN r8029)
* `GetConnections` now returns the complete connectivity of a node, regardless of its thread and process (SVN r8030, SVN r8036)
* User documentation is now world-readable after installation (trac #318)

## Version 1.9.8017 (2009-02-03)

* `GetConnections` now returns connectivity data from all processes (trac #305)
* New SLI commands `CompileMath` and `ExecuteMath` to allow standard infix notation of math expressions (SVN r7937)
* New SLI command `Inline` to optimize procedures before execution (SVN r7947)
* Fixed a memory leak in `Token`s assignment operator. Especially with repeated simulation runs this has been disastrous (SVNr7966)
* `Tmpnam` warning at end of compilation is gone (SVN r7976)
* Precise models now have an example script (SVN r7981)
* PyNEST: `voltage_trace` and `raster_plot` now support a timeunit (SVN r8005)
* PyNEST: Added the functions `help` and `helpdesk` (SVN r8011)
* Fixed usage of GSL solvers in all models (SVN r7995)
* Topology now loaded automatically (SVN r7928)

## Version 1.9.7926 (2009-01-14)

* PyNEST: Error reporting now works more reliably (SVN r7902, SVN r7903)
* PyNEST: Linking on MacOS X now works again (trac #301)

## Version 1.9.7905 (2008-12-18)

* Added topology library back to the trunk (SVN r7863). For details see Topological_Connections
* Added `aeif_w_meter` and corresponding infrastructure (trac #295)
* PyNEST: `SetStatus` now supports any iterable (SVN r7879)
* Model and synapse documentation now contains information about the suported events (r7889, r7893)

## Version 1.9.7857 (2008-11-27)

* Initial public release of NEST 2.0 beta
* Major changes since NEST 1.0.13:
  * Support for parallel and distributed simulations using threads and MPI
  * Much more flexible synapse system that allows dynamic synapses
  * A new user interface via the Python programming language (PyNEST)
  * Significant performance and scaling improvements
  * Tons of bugs fixed
