snap-python
===========

1. Install SWIG for your platform (see below).  Swig should be able to run from the command-line.

2. Checkout the snap-python repository as well as the SNAP C++ repository.

		git clone git@github.com:snap-stanford/snap-python.git
		git clone git@github.com:snap-stanford/snap.git

2. Then, run `make` from the top-level of `snap-python`. This will make the SNAP code into a Python module, using SWIG and run some Python tests in the `test` directory. Finally, it installs SNAP in Python's user libraries directory.

		cd snap-python
		make
		cd swig
		python setup.py install --user

	From a Python interpreter, you should be able to import `snap` module:

		$ python
		>>> import sys
		>>> sys.path.append("swig")
		>>> import snap

3. There are some examples in the `examples` directory.  For example, to run benchmarks:

		$ cd examples
		$ python benchmark.py -h
		usage: benchmark.py [-h] [-v] [-r RANGE] [-e EDGES_DEG] [-d] [-t GRAPH_TYPES]
		                    [-n NUM_ITERATIONS] [-o OUTPUT_FILE] [-g] [-w]

		optional arguments:
		  -h, --help            show this help message and exit
		  -v, --verbose         increase output verbosity
		  -r RANGE, --range RANGE
		                        range (4-6) (10^4 to 10^6 nodes)
		  -e EDGES_DEG, --edges_deg EDGES_DEG
		                        range of degrees (e.g "2-3" => (10^1 to 10^3 edges per
		                        node)
		  -d, --deterministic   deterministic benchmark
		  -t GRAPH_TYPES, --graph_types GRAPH_TYPES
		                        Graph types, comma separated. Available: rand_ungraph,
		                        rand_ngraph, rmat, pref, sw
		  -n NUM_ITERATIONS, --num_iterations NUM_ITERATIONS
		                        number of iterations
		  -o OUTPUT_FILE, --output_file OUTPUT_FILE
		                        file to output results
		  -g, --generate        generate new graphs
		  -w, --write_graph     save graph
		$ python benchmark.py -v -g -r 4-6	# needs about 4.3GB RAM and 4 min to run


SWIG Installation
-----------------

### Linux

Follow the instructions from SWIG's website: download, configure and make, [SWIG files](http://www.swig.org/download.html).  Or, use your built-in installer (a CentOS example):

	sudo yum install swig

### Mac OS X

swig-1.3.12 and later support OS-X/Darwin.

0. If you have ``homebrew``, simply hit ``brew install swig`` in terminal and ignore the rest of the instructions. Otherwise, download the Unix sources, configure, and build from the command terminal. This has been tested on 10.8.2. The following is adopted from [ColourBlomb](http://blog.colourbomb.net/?p=49).

1. Download the Unix source from http://swig.org/download.html

2. Moving to the terminal, extract the files from the tarball and move to the root directory of the SWIG install:

		cd /Developer/SWIG
		tar -xf swig-2.0.4.tar.gz
		cd swig-2.0.4

3. Run `./configure`.  This will produce an error if you don't have the PCRE (Perl Compatible Regular Expressions) library package installed. 
This dependency is needed for configure to complete. Either:
	- Install the PCRE developer package on your system (preferred approach).
	- Download the PCRE source tarball, build and install on your system
	as you would for any package built from source distribution.
	- Use the `Tools/pcre-build.sh` script to build PCRE just for SWIG to statically
	link against. Run `Tools/pcre-build.sh –help` for instructions.
	(quite easy and does not require privileges to install PCRE on your system)
	- Configure using the `–without-pcre` option to disable regular expressions support in SWIG
	(not recommended).
	See `config.log` for more details.
		
			make
			sudo make install

4. PCRE should now have successfully installed so move to the swig install directory and try `./configure` again:

		cd ../swig-2.0.4
		./configure

	This time no errors are thrown so try and install:

		make
		sudo make install

5. Once this has completed test that SWIG has installed correctly, type `swig` into the terminal and hopefully you’ll get the response:
  Must specify an input file. Use `-help` for available options.

SWIG Benchmarks
-----------------
Example SWIG programs using the SNAP Ringo for multi-attribute edges are in the `examples` directory.  The benchmark program `benchmark.py` performs a series of functions on the graph data, including node/edge iteration, degree checks, clustering coefficients, largest weakly and strongest components, etc.  For R-MAT graphs with 1 million nodes and 10 million edges, this takes on average: 

- On CentOS 6.3 with 2.66 GHz processor, 19.71 sec to generate a new graph and and 17.49 sec to run the tests.
- On Mac OSX 10.8 with 2.6 GHz processor, 13.95 sec to generate and 15.06 sec to run the tests.
	
To run a benchmark test you can run the following command:

	python benchmark.py --verbose -n 5 --range 4-7 --type rmat --generate

