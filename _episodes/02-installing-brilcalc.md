---
title: "Installing brilcalc"
teaching: 10
exercises: 0
questions:
- "What do I use to calculate luminosity myself?"
- "How do I install the tools needed to calculate luminosity?"
objectives:
- "Know what tool to use to calculate luminosity"
- "Know how to install this tool in the CMS Open Data environment"
keypoints:
- "`brilcalc` is a command-line tool provided by the CMS BRIL group for calculating luminosity"
---

The CMS Beam Radiation Instrumentation and Luminosity (BRIL) group provides a command-line tool called `brilcalc`.
With `brilcalc` one can calculate luminosity.

## Installation

If you are using the VM, or if are using the docker container and have the cvmfs file system installed locally, do the following to install `brilcalc`.

Set your PATH:

~~~
export PATH=$HOME/.local/bin:/cvmfs/cms-bril.cern.ch/brilconda/bin:$PATH
~~~
{: .bash}

Install BRIL software using `pip`:

~~~
pip install --user brilws
~~~
{: .bash}

At the end of a successful installation you should see the messages:

~~~
Successfully built brilws
Installing collected packages: brilws
Successfully installed brilws
~~~
{: .output}

Check the installation by running the command:

~~~
brilcalc --version
~~~
{: .bash}

which should output a version number.

{% include links.md %}
