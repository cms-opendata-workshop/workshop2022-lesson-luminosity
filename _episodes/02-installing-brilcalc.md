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

To install brilcalc, run

```
docker run -it --name brilws gitlab-registry.cern.ch/cms-cloud/brilws-docker
```

Alternative ways of using brilcalc can be found at [http://opendata.cern.ch/docs/cms-guide-luminosity-calculation](https://opendata.cern.ch/docs/cms-guide-luminosity-calculation#test-brilcalc).

Check the installation by running the command:

~~~
brilcalc --version
~~~
{: .bash}

which should output a version number.

{% include links.md %}
