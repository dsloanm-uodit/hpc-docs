# Accessing the Cluster

!!! note

    Cluster access is available only from within the University of Dundee campus or using the University VPN. See [here](https://www.dundee.ac.uk/guides/using-vpn) for details for connecting to the VPN.

## Enabling Cluster Access

Raise a service desk ticket at:

[Contact the University Service Desk using Help4U | University of Dundee](https://www.dundee.ac.uk/guides/contact-university-service-desk-using-help4u)

requesting access to the cluster and providing the following details:

  * Your University of Dundee account name
  * The name and contact details of your group leader
  * Project Name
  * Project ID
  * The path to your group's fileset on the cluster, e.g. `/cluster/mygroupfileset` or `/actives/path/to/fileset` (see below if a new group fileset is required)

Once your ticket is processed, your account will be granted the appropriate access to the cluster.

Note that a Life Sciences Directory (LSD) account is no longer a prerequisite for cluster access.

### Requesting a New Group Fileset

If your group does not have an existing fileset, please also include in the service desk ticket:

  * The name of the intended fileset owner
  * Accounts of users needing access
  * School name
  * Department name
  * Total size of initial data and future growth requirements
  * Backup requirements

## Connecting

Interactive access to the cluster is achieved through an SSH (Secure SHell) client, such as the terminal `ssh` command on Mac and Linux or [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) on Windows. For University-managed systems, PuTTY can be found on [AppsAnywhere](https://myapps.dundee.ac.uk/).

With the SSH client of your choice, connect to `login.compute.dundee.ac.uk` using your credentials. For example, using the terminal-based client:

```console
ssh <dundee-username>@login.compute.dundee.ac.uk
```

then enter your password when prompted.

## Usage

Useful sources of info for getting started:

  * [The DAG HPC Workshop slide set](https://dag.compbio.dundee.ac.uk/workshops/HPC_Apr2021.pptx)
  * [The UoD Compute Cluster Teams channel](https://teams.microsoft.com/l/team/19%3a63a2d1d10e5346c79d8b35dec6006a40%40thread.tacv2/conversations?groupId=4153042c-375d-4caa-a654-d691f65da8bb&tenantId=ae323139-093a-4d2a-81a6-5d334bcd9019)
