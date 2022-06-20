---
title: "Introduction"
teaching: 10
exercises: 0
questions:
- "What is luminosity?"
- "What is the difference between instantaneous and integrated luminosity?"
- "Why is knowing the luminosity important?"
- "How is the luminosity measured?"
objectives:
- "Know what luminosity is and why it is important"
- "Know how luminosity is measured"
keypoints:
- "Luminosity is a measure of how many collisions are delivered to and recorded by the detector."
- "Instantaneous luminosity is usually expressed as the number of collisions per square centimeter per second."
- "Integrated luminosity is the integral of instantaneous luminosity over time and is a measurement of data size. It is usually expressed in units of inverse cross section."
- "Knowing the luminosity is important to determining and measuring accelerator and detector performance and operation. It is also an essential component for measuring cross sections and for setting limits on beyond-SM processes."
- "Measurement of luminosity is done with several systems in the detector."
---

## Luminosity and its importance

The luminosity of a collider is the number of collisions per area (i.e. cross section) per time. The typical unit is $$ \mathrm{cm^{-2} s^{-1}} $$. This luminosity is sometimes referred to
as the "instantaneous" luminosity $$ \mathcal{L}_{inst} $$. The "integrated" luminosity is the integral of the instantaneous luminosity over time:

$$ \mathcal{L}_{int} = \int \mathcal{L}_{inst} dt $$

The integrated luminosity is a measurement of data size and is usually expressed in units of inverse cross section such as $$ \mathrm{fb^{-1}} $$.

The importance of knowing the integrated luminosity cannot be overstated. It is an esssential component for measuring cross sections and for setting upper limits on new physics processes beyond
the Standard Model. You can't determine well how much you see (or don't see) coming out of the collisions if you don't know accurately and precisely what came in.

## Measuring luminosity
CMS has several systems for measuring luminosity including making use of the hadronic forward calorimeter and the pixel detector. There have also been several dedicated luminometers installed over the course of operation of CMS.

The readout rate of quantities like hits in a detector (like a luminometer) is given by $$ R $$ and is given by the product of the instantaneous luminosity $$ \mathcal{L}_{inst} $$ and the visible cross section $$ \sigma_{vis} $$:

$$ R = \mathcal{L}_{inst} \sigma_{vis}$$

The determination of $$ \sigma_{vis} $$ is carried out in so-called van der Meer (VdM) scans. In these scans the two colliding beams are separated and then moved across each other.
The rate $$ R $$ is then determined as a function of beam separation to measure the beam overlap width.

The instantaneous luminosity $$ \mathcal{L}^{i}_{inst} $$ for
a single colliding bunch $$ i $$ can be determined from the beam overlap width and
known beam parameters such as the number of protons in each colliding bunch for each beam and the LHC orbit frequency. Then a value for $$ \sigma_{vis} $$ can be obtained.

A VdM scan is a special beam condition but this value of $$ \sigma_{vis} $$ still holds for regular colliding beam conditions (since the detector acceptance has in-principle not changed). Therefore this value along with $$ R $$ can be used to subsequently determine $$ \mathcal{L}_{inst} $$.

In practice the performance of the luminometers can degrade over time due to effects such as radiation damage. CMS is capable of conducting fast luminosity scans with small beam separation, so-called "emmittance scans". These scans, performed similarly to VdM scans,
can take just a few minutes and are typically performed at the beginning and at the end of a fill.

{% include links.md %}
