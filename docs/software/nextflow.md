# Nextflow

Nextflow is a workflow manager for scientific computing. Details can be found at:

[Nextflow.io](https://www.nextflow.io)

Community pipelines for use with Nextflow are available at:

[nf-co.re](https://nf-co.re)

A configuration profile for the University of Dundee cluster is maintained in [the central nf-core repository](https://github.com/nf-core/configs/blob/master/docs/uod_hpc.md). To make use of the profile, Nextflow should always be run with argument `-profile uod_hpc`.

## Installation

!!! note

    These steps should be followed only once to set up Nextflow on your account.
    See the [Usage](#usage) section below for how to run Nextflow once it is installed.

First, set up a Conda environment in your data directory:

```console
setup-miniconda-env /cluster/<group_name>/<your_directory>/nextflow-env
```

Read and agree to the licence terms to proceed (press ENTER, the 'q' key once you have finished reading, then type "yes" and press ENTER to agree).
Wait for installation to complete.

Activate the newly installed conda environment and install [the Nextflow Conda package](https://anaconda.org/bioconda/nextflow):

```console
source "/cluster/<group_name>/<your_directory>/nextflow-env/bin/activate"
conda install -y -c bioconda nextflow
```

Set environment variable `NXF_SINGULARITY_CACHEDIR` to a path in your data directory. This instructs Nextflow to store Singularity images in the given directory rather than re-download them for every run:

```console
export NXF_SINGULARITY_CACHEDIR="/cluster/<group_name>/<your_directory>/nxf-singularity-cache"
```

Perform a test run to confirm installation was successful:

```console
$ nextflow run hello
N E X T F L O W  ~  version 23.10.0
Pulling nextflow-io/hello ...
 downloaded from https://github.com/nextflow-io/hello.git
Launching `https://github.com/nextflow-io/hello` [distraught_ramanujan] DSL2 - revision: 1d71f857bb [master]
executor >  local (4)
[5a/0b0d65] process > sayHello (1) [100%] 4 of 4 ✔
Ciao world!

Hello world!

Hola world!

Bonjour world!
```

For convenience, append the `export NXF_SINGULARITY_CACHEDIR` and conda activation commands to your `.bashrc` file to avoid having to run both on each log-in or include both in every job script:

```console
echo export NXF_SINGULARITY_CACHEDIR='"/cluster/<group_name>/<your_directory>/nxf-singularity-cache"' >> ~/.bashrc
echo source '"/cluster/<group_name>/<your_directory>/nextflow-env/bin/activate"' >> ~/.bashrc
```

Note that this may increase the time taken for a log-in to complete.

For additional guidance on installing Nextflow, official documentation is available in:

* [Nextflow installation instructions](https://www.nextflow.io/docs/latest/getstarted.html#installation)
* [nf-core installation instructions](https://nf-co.re/docs/usage/installation)

## Usage

Create a job script containing your desired Nextflow command, always including `-profile uod_hpc` to ensure the correct cluster configuration is applied. For example:

```console
nano myjobscript.sh
```

with contents:

```bash
# NOTE: The following two lines should be omitted if they are already appended to your .bashrc
export NXF_SINGULARITY_CACHEDIR="/cluster/<group_name>/<your_directory>/nxf-singularity-cache"
source "/cluster/<group_name>/<your_directory>/nextflow-env/bin/activate"

nextflow run nf-core/sarek -profile uod_hpc -r 3.1.1 --input ./samplesheet.csv --outdir ./results ...<further parameters as required>...
```

Submit the script using `qsub`:

```console
qsub myjobscript.sh
```

Standard and error output for the job will be produced in `.o` and `.e` files, such as `myjobscript.sh.o1400340` and `myjobscript.sh.e1400340`, as it runs.

Pipelines require no explicit installation. When using one for the first time, it will be automatically downloaded as detailed in the [the nf-core documentation](https://nf-co.re/docs/usage/installation#pipeline-code).

For scientific reproducibility, it is recommended that option `-r <revision_name>` be provided to specify the version of the pipeline whenever a command is run. Additional discussion is available in the [reproducibility section of the documentation for the sarek pipeline](https://nf-co.re/sarek/3.3.2/docs/usage#reproducibility). Note that the `-r` option is not specific to sarek and can be used with other pipelines.

