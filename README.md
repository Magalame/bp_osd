# BP+OSD: A decoder for sparse quantum codes
A C library implementing belief propagation with ordered statistics post-processing for decoding sparse quantum codes as described in [arXiv:2005.07016](https://arxiv.org/abs/2005.07016). 

## Build
To build, run the following commands from the repository root.

```
mkdir build
cd build
cmake ../
make
cd ..
```

## Alist files

This library uses the [alist](http://www.inference.org.uk/mackay/codes/alist.html) sparse format for storing parity check matrices. A python script for converting Numpy matrices to alist files can be found in "src_python" directory.
Example alist files for various quantum codes can be found in the "codes" directory.


## Running the BP+OSD Sim
The C++ source file for the BP+OSD sim script can be found at "sim_scripts/bp_osd_decode.cpp".
The example program to decode a [[400,16,6]] random QLDPC code can be run (from the repository root) with the following command

```
build/bp_osd_decode sim_scripts/mkmn_input_test.json sim_scripts/output
```

The general syntax for running the sim script is as follows

```
./build/bp_osd_decode <path_to_input_file.json> <path_to_output_directory>
``` 



### Input file structure
The bp_osd_decode script takes JSON files as input. An example input file is shown below:
```c
{
  "hx_filename": "codes/random/mkmn/hgp_mkmn_16_4_6_hx.alist",
  "lx_filename": "codes/random/mkmn/hgp_mkmn_16_4_6_lx.alist",
  "code_label": "mkmn_16_4_6",
  "input_seed": 149,
  "bit_error_rate": 0.05,
  "target_runs": 1000,
  "osd_order": 40,
  "max_iter": 50,
  "osd_method": "combination_sweep"
}
```

Where

- "hx_filename": path to hx parity check alist file
- "lx_filename": path to lx logical matrix alist file
- "code_label": whatever you like
- "input_seed": the input seed for the Mersenne twister random number generator
- "bit_error_rate": the bit error rate
- "target_runs": the number of runs you would like to simulate
- "osd_order": the osd order parameter
- "max_iter": the iterations depth for BP. If this is set to 0, then the value is set to N, where N is the code block length.
- "osd_method": Currenlty three options are available: 1) "combination_sweep"; 2) "exhaustive"; 3) "osd_0". The details of these techniques are described in [arXiv:2005.07016](https://arxiv.org/abs/2005.07016).

### Output
The output is saved to the directory specified in the second command line argument. The output file format is JSON and the filename is autogenerated.


## Using this library in another project

To use this library in another project copy and paste the repository into your project root. The library can then be imported by adding the followig to your cmake file:
```c
project(import_example)
add_subdirectory(bp_osd_c)
add_executable(import_example main.cpp)
target_link_libraries(import_example bp_osd)
```

## Software
This library makes use of the following software:
- [Sofware for low density parity check codes](https://github.com/radfordneal/LDPC-codes), Radford M. Neal
- [JSON for modern C++](https://github.com/nlohmann/json), Niels Lohmann
- [The Mtwister C library](https://github.com/ESultanik/mtwister), Evan Sultanik

## Licence

MIT License

Copyright (c) 2020 Joschka Roffe

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
