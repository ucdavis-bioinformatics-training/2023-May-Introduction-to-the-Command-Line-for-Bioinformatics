# Controlling Environments with Conda

## What is conda and why do we care about it?
- Conda is a cross platform (Windows, Linux, MacOSx) environment management software. 
- Works for any language, version, and library as specified by the user. 
- Allows for easy control and maintenance of multiple environments with different specifications.
- Allows for software packages and their dependencies to be easily loaded "automatically" via channels such as Bioconda, which specifically hosts a variety of bioinformatics software.  


First, let's install Conda. We will be installing a version of conda called "miniconda" which is a minimal installation. Go to the [Miniconda Homepage](https://docs.conda.io/en/latest/miniconda.html). For Ubuntu on Windows, choose the Linux 64-bit installer. For Macs, choose the 64-bit bash installer. Do not click on the link, but instead right click (or your equivalent) on the link and copy the link. Now in your terminal, go to your home directory and use wget to download the installer.

    wget <your copied link>

This is a shell script that will install Miniconda for us. We will need to make it executable:

    chmod u+x <your script file>

Now run the script:

    ./<your script file>

You will be asked some questions about where to install and about initialization. Once the install is finished, Windows 10 users will need to do an extra step before proceeding. You will need to open a *Powershell* (not Ubuntu) and type the following commands. Mac users skip this step.:

    wsl --shutdown
    wsl

This will shutdown your current Ubuntu terminals and you will need to open a new one.

Now, we need to add the path to the conda executable to the PATH variable.

    export PATH=<path to miniconda>/bin:$PATH

Now we will be able to use conda to install many different kinds of software. Let's install [hclust2](https://github.com/SegataLab/hclust2). I like to install software using conda virtual environments, which is basically a way to have installations of many different softwares without having dependency issues because each software is installed in its own separate environment with all of the dependencies. 

First add the bioconda channel to conda. A channel is basically a package repository, and "bioconda" is a repository of many bioinformatics software.:

    conda config --add channels bioconda

Then search for hclust2:

    conda search hclust2

Now let's create a conda virtualenv and install hclust2 into it.

    conda create -y -n hclust2-1.0.0 hclust2

Once this is done, we need to activate the environment to use it:

    source activate hclust2-1.0.0

You will notice that your prompt will change when the activation is complete. Now you have access to hclust2:

    hclust2.py --help