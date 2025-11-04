# NAMD with mass updates

The patches supplied in this repository permit arbitrary atomic mass updates within NAMD3 [1]. These mass updates, for instance, provide an easy interface for introducing hydrogen mass repartitioning (HMR) [2] or light TIP3 [3] without requiring updates to the PSF file.

The motivation by this patch are three-fold. First, arbitrarily changing the PSF file can be clunky, and there is a preference to making mass changes "on the fly." Second, NAMD only identifies hydrogen atoms correctly if their masses are between 1-3.5 amu. In light TIP3, hydrogen atoms have a mass of 0.186496 amu, which causes them to be identified as Drude particles. Third, NAMD only identifies oxygen atoms correctly if their masses are between 14-18 amu. If a hydrogen mass of 3.024 amu is used in HMR, this can decrease the oxygen mass below this cutoff. This will ultimately cause SETTLE to be disabled for water.

## Installation

First, the NAMD3 source should be downloaded from https://www.ks.uiuc.edu/Development/Download/download.cgi?PackageName=NAMD. Currently only patches for NAMD\_3.0.1 or NAMD\_3.0.2 are provided. 


Second, unpack NAMD3 and enter its directory.

```
tar xzf NAMD_3.0.2_Source.tar.gz
cd NAMD_3.0.2_Source
```

Third, download the appropriate patch file into the same directory and run the patch. 

```
wget ...
patch -p1 < NAMD_3.0.2_mass_updates.patch
```

That's it!

## Use

An example system is given in the "example" directory, including the implementation of light water or HMR.

Mass updates are enabled in the NAMD configuration file by setting

```
massUpdates yes
```

It is also required to specify a file indictating which masses will be updated. This file is a text file with a number of values exactly equal to the number of atoms. Following the order of atoms in the system, the values in this file indicate mass updates. Mass updates will only occur if a value greater than 0 is provided. If a value of 0 is provided, no mass updates will be performed and the mass from the PSF file will be retained.

```
massUpdatesFile <massUpdatesFile>
```

## References

[1] Phillips, J. C. et al. (2020) Scalable molecular dynamics on CPU and GPU architectures with NAMD. *J. Chem. Phys.* 153(4): 044130. 
[2] Feenstra, K. A., Hess, B., & Berendsen, H. J. C. (1999) Improving efficiency of large time-scale molecular dynamics simulations of hydrogen-rich systems. *J. Comput. Chem.* 20(8): 749-895. 
[3] Jiménez, J. G. R., Fábián, B., & Hummer, G. (2024) Faster sampling in molecular dynamics simulation with TIP3P-F water. *J. Chem. Theory. Comput.* 20(24): 11068-11081.  

