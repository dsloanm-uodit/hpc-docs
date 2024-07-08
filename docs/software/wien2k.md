# Wien2k

!!! warning

    Wien2k is licensed software. You must ensure you hold a valid Wien2k licence allowing use of the software on the cluster.

Details of Wien2k can be found at the project page:

[Wien2k.at](ttp://www.wien2k.at/)

## Installation

First, set up a Conda environment in your data directory:

```console
setup-miniconda-env /cluster/<group_name>/<your_directory>/wien2k-env
```

Read and agree to the licence terms to proceed (press ENTER, the 'q' key once you have finished reading, then type "yes" and press ENTER to agree).
Wait for installation to complete.

Activate the newly installed conda environment, update and install prerequisite compilers and libraries:

```console
source "/cluster/<group_name>/<your_directory>/wien2k-env/bin/activate"
conda update -n base -c defaults conda
conda config --add channels conda-forge
conda install gcc gfortran numpy openmpi openblas fftw gnuplot octave ghostscript lapack
```

Create a new installation directory for Wien2k and extract the source code tar files:

```console
cd /cluster/<group_name>/<your_directory>
mkdir wien2k-install
mv WIEN2k_23.2.tar wien2k-install
cd wien2k-install
tar xf WIEN2k_23.2.tar

# Uncompress the files and run the enclosed script to expand the archives
gunzip *.gz
./expand_lapw
```

Once all archives are extracted, run the interactive installer:

```console
./siteconfig_lapw
```

You will receive a series of prompts. Answer as indicated below:

| Prompt                                                              | Response                                                |
| ------------------------------------------------------------------- | ------------------------------------------------------- |
| `It seems you do not have the intel fortran compiler in your path.` | `c`                                                     |
| `Specify a system`                                                  | `LG`                                                    |
| `Specify compilers`                                                 | `gfortran` for `f90 compiler`<br>`gcc` for `C compiler` |

The installer will attempt to search for OpenBLAS and fail. Press `RETURN` to continue for now. At the following prompts:

| Prompt                                                                  | Response                                            |
| ----------------------------------------------------------------------- | --------------------------------------------------- |
| `Specify LIBXC settings`                                                | `N`                                                 |
| `Specify FFTW settings`                                                 | `N`                                                 |
| `Please specify the path of your FFTW installation`                     | `/cluster/<group_name>/<your_directory>/wien2k-env` |
| `Please specify the target achitecture of your FFTW library`            | `lib`                                               |
| `Please specify the name of your FFTW library or accept present choice` | Press `RETURN` to continue.                         |

At `FFTW Summary`, confirm that you see the following:

```console
Your FFTW_OPT are:   -DFFTW3 -DFFTW_OMP -I/cluster/<group_name>/<your_directory>/wien2k-env/include
Your FFTW_LIBS are:  -L/cluster/<group_name>/<your_directory>/wien2k-env/lib -lfftw3 -lfftw3_omp

These options derive from your chosen settings:

FFTWROOT:            /cluster/<group_name>/<your_directory>/wien2k-env/
FFTW_VERSION:        FFTW3
FFTW_LIB:            lib
FFTW_LIBNAME:        fftw3
```

Continue and follow prompts:

| Prompt                                                                                                     | Response                                                                 |
| ---------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------ |
| `Compiler and linker options`                                                                              | `R`                                                                      |
| `Real libraries=`                                                                                          | `/cluster/path/to/data/dir/wien2k-env/lib/libopenblas.so -lpthread`      |
| After being returned to `Compiler and linker options`                                                      | `S`                                                                      |
| `Shared Memory Architecture?`            | `y`                                                             | `y`                                                                      |
| `Do you know/need a command to bind your jobs to specific nodes?`                                          | `N`                                                                      |
| `Do you have MPI, ScaLAPACK, ELPA, or MPI-parallel FFTW installed and intend to run finegrained parallel?` | `N`                                                                      |
| `(Re-)Dimension parameters`                                                                                | `1` and change `NMATMAX` to `100000`<br>`1` and change `NUME` to `10000` |
| `Compile/Recompile programs`                                                                               | `A` then wait                                                            |
| `Perl Path`                                                                                                | Press `RETURN` to continue.                                              |
| `Temp Path`                                                                                                | Press `RETURN` to continue.                                              |

For additional guidance on installing and running Wien2k, official documentation is available at:

* [Wien2k User's Guide](http://www.wien2k.at/reg_user/textbooks/usersguide.pdf)

## Usage

Create a job script containing your desired Wien2k command(s):

```console
nano myjobscript.sh
```

with contents:

```bash
# Job Name
#$ -N Wien2k-Job

# Number of job slots/cores
#$ -pe smp 16

# Job class:
# * short (default) - max runtime of 12 hours
# * long - max runtime of 72 hours
# * 4weeks - max runtime of 4weeks
#$ -jc short

# Change to working directory before running script
# i.e. the directory this script is `qsub`-ed from
#$ -cwd

# Activate your Wien2k Conda environment
source "/cluster/<group_name>/<your_directory>/wien2k-env/bin/activate"

# Wien2k command(s)
run_lapw <parameters as required>
```

Submit the script using `qsub`:

```console
qsub myjobscript.sh
```

Standard and error output for the job will be produced in `.o` and `.e` files.
