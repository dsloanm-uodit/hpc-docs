# Nextflow

Nextflow is a workflow manager for scientific computing. Details can be found at:

[Nextflow.io](https://www.nextflow.io)

Community pipelines for use with Nextflow are available at:

[nf-co.re](https://nf-co.re)

## Installation

!!! note

    These steps should be followed only once to set up Nextflow on your account.
    See the [Usage](#usage) section below for how to run Nextflow once it is installed.

First, install a Conda environment in your data directory:

```console
setup-miniconda-env /cluster/<group_name>/<your_directory>/nextflow-env
```

Read and agree to the licence terms to proceed (press ENTER, the 'q' key once you have finished reading, then type "yes" and press ENTER to agree).
Wait for installation to complete.

Activate the newly installed conda environment:

```console
source "/cluster/<group_name>/<your_directory>/nextflow-env/bin/activate"
```

Set up Conda to use [Bioconda](https://bioconda.github.io/):

```console
conda config --add channels defaults
conda config --add channels bioconda
conda config --add channels conda-forge
conda config --set channel_priority strict
```

Install [the Nextflow Conda package](https://anaconda.org/bioconda/nextflow):

```console
conda install -c bioconda nextflow
```

Create a `nextflow.config` configuration file:

```console
cat > nextflow.config<< EOF
params {
  config_profile_description = 'University of Dundee Compute Cluster'
  config_profile_contact = 'Infrastructure and Research Computing Team'
  config_profile_url = 'https://uod-hpc.readthedocs.io/en/latest/software/nextflow/'
}

process {
  executor = 'sge'
  penv = 'smp'
  queue = 'all.q'

  // Workaround for:
  // https://github.com/nextflow-io/nextflow/issues/2449
  withName: '.*' { memory = null }
  withName: '.*' { time = null }
}

executor {
  max_memory = 128.GB
  max_cpus = 28
  max_time = 72.h
}
EOF
```

This instructs Nextflow as to the scheduler and job parameters to use for the cluster. Further details are available [in the Nextflow documentation](https://www.nextflow.io/docs/latest/executor.html#sge).

!!! note

    Manual creation of a configuration file will be replaced with use of "-profile" at run time.
    We are currently working to include the University of Dundee cluster in [the central nf-core configuration repository](https://github.com/nf-core/configs/)

Perform a test run to confirm installation was successful:

```console
$ nextflow run hello
N E X T F L O W  ~  version 23.04.4
Launching `https://github.com/nextflow-io/hello` [mighty_ride] DSL2 - revision: 1d71f857bb [master]
executor >  sge (4)
[5a/cc164e] process > sayHello (2) [100%] 4 of 4 ✔
Hello world!

Bonjour world!

Hola world!

Ciao world!
```

Append the activation command to your `.bashrc` file to avoid having to source your conda environment on each log-in:

```console
echo source '"/cluster/<group_name>/<your_directory>/nextflow-env/bin/activate"' >> ~/.bashrc
```

For additional guidance on installing Nextflow, official documentation is available in:

* [Nextflow installation instructions](https://www.nextflow.io/docs/latest/getstarted.html#installation)
* [nf-core installation instructions](https://nf-co.re/docs/usage/installation)

## Usage

Run your Nextflow command as desired, for example:

```console
nextflow run nf-core/sarek -r 3.1.1 --input ./samplesheet.csv --outdir ./results ...<further parameters as required>...
```

Pipelines require no explicit installation. When using one for the first time, it will be automatically downloaded. Further details are available in [the nf-core documentation](https://nf-co.re/docs/usage/installation#pipeline-code).

For scientific reproducibility, it is recommended that option `-r <revision_name>` be provided to specify the version of the pipeline whenever a command is run. Additional discussion is available in the [reproducibility section of the documentation for the sarek pipeline](https://nf-co.re/sarek/3.3.2/docs/usage#reproducibility). Note that the `-r` option is not specific to sarek and can be used with other pipelines.
