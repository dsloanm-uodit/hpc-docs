# GROMACS

GROMACS is a free and open-source molecular dynamics package. Details can be found at the project page:

[GROMACS webpage](https://www.gromacs.org/)

General documentation can be found at:

[GROMACS documentation](https://manual.gromacs.org/current/index.html)

## Installation

!!! note

    These steps have been tested with GROMACS v2024.2

Retrieve and extract the GROMACS source code:

```bash
wget https://ftp.gromacs.org/gromacs/gromacs-2024.2.tar.gz
tar xf gromacs-2024.2.tar.gz
```

Set up a miniconda environment, editing path as required:

```bash
setup-miniconda-env /cluster/<group_name>/<your_directory>/gromacs-env
source /cluster/<group_name>/<your_directory>/gromacs-env/activate
```

Update and install prerequisites:

```bash
conda update -n base -c defaults conda
conda config --add channels conda-forge
# Compiler version 10.4.0 required as CUDA-11 does not support GCC > 10
conda install gcc=10.4.0 gxx=10.4.0 cmake
```

Open an interactive job on a GPU node and reactivate conda:

```bash
qrsh -adds l_hard hostname gpu*
source /cluster/<group_name>/<your_directory>/gromacs-env/activate
```

Set environment variables so CUDA is picked up by GROMACS at configuration time:

```bash
export PATH=/usr/local/cuda-11/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda-11/lib64:$LD_LIBRARY_PATH
```

Create the directory where GROMACS is to be installed, editing path as required:

```bash
mkdir -p /cluster/<group_name>/<your_directory>/gromacs-install
```

Perform the build:

```bash
cd gromacs-2024.2
mkdir build
cd build

# Set -DCMAKE_INSTALL_PREFIX to the directory where GROMACS is to be installed.
# Adjust this line further to enable/disable features as required.
cmake .. -DGMX_GPU=CUDA -DGMX_BUILD_OWN_FFTW=ON -DCMAKE_INSTALL_PREFIX=/cluster/<group_name>/<your_directory>/gromacs-install
make
make install
```

GROMACS will now be installed in `/cluster/<group_name>/<your_directory>/gromacs-install` with the `gmx` binary located in the `bin` subdirectory.

## Usage

Create a job script containing your desired command(s):

```bash
nano myjobscript.sh
```

with contents:

```bash
# Job Name
#$ -N GROMACS-Job

# Number of job slots/cores
#$ -pe smp 4
# Number of GPUs
#$ -l gpu=1

# Job class:
# * short (default) - max runtime of 12 hours
# * long - max runtime of 72 hours
# * 4weeks - max runtime of 4weeks
#$ -jc short

# Change to working directory before running script
# i.e. the directory this script is `qsub`-ed from
#$ -cwd

# Activate your Conda environment
source "/cluster/<group_name>/<your_directory>/gromacs-env/bin/activate"
# Add GROMACS install to your PATH
export PATH=/cluster/<group_name>/<your_directory>/gromacs-install/bin:$PATH

# Command(s)
gmx <parameters as required>
```

Submit the script using `qsub`:

```bash
qsub myjobscript.sh
```

Standard and error output for the job will be produced in `.o` and `.e` files.
