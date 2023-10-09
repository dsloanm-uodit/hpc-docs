# Accessing the Cluster

## Registering a Life Science Directory Account

As a prerequisite to cluster access, users must register a Life Sciences Directory (LSD) account through the following web form:

[Life Sciences Directory Registration](https://lsd.lifesci.dundee.ac.uk/register/internal)

!!! note

    The registration page is available only from within the University of Dundee campus or using the University VPN. See [here](https://www.dundee.ac.uk/guides/using-vpn) for details for connecting to the VPN.

## Enabling Cluster Access

Once you have a LSD account, raise a service desk ticket at:

[Contact the University Service Desk using Help4U | University of Dundee](https://www.dundee.ac.uk/guides/contact-university-service-desk-using-help4u)

requesting access to the cluster and providing the following details:

  * Your LSD account name
  * The name and contact details of your group leader
  * (If possible) the path to your group's fileset on the cluster, e.g. `/cluster/mygroupfileset`

Once your ticket is processed, your LSD account will be granted the appropriate access to the cluster.

## Connecting

Interactive access to the cluster is achieved through an SSH (Secure SHell) client, such as the terminal `ssh` command on Mac and Linux or [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) on Windows. For University-managed systems, PuTTY can be found on [AppsAnywhere](https://myapps.dundee.ac.uk/).

With the SSH client of your choice, connect to `login.compute.dundee.ac.uk` using your LSD credentials. For example, using the terminal-based client:

```console
ssh <lsd-username>@login.compute.dundee.ac.uk
```

then enter your password when prompted.
