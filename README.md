# T-Garden-SRGs

## Introduction, Sources, Licences

This repository contains SRGs calculated for the T-Garden network repository along with a few other select networks. 

The SRG simulation data can be freely reused with **CC-By 4.0** licence, please cite the following article:
*Not yet released*

The repository contains Deterministic SRGs and PSRGs for a total of **176** networks from a variety of sources, with the following breakdown:

- T-Garden **160** - https://tgarden.net/
    - Generated **116** - **CC-By 4.0**
    - TopologyZoo **33** - http://www.topology-zoo.org/ - **CC-By 4.0**
    - SNDLib **11** - https://sndlib.put.poznan.pl/ - **ZIB Academic Licence**
- JSACdata **8** - https://github.com/jtapolcai/regional-srlg - **GNU General public Licence**
- TopoHub **8** - https://topohub.org/




## Directory structures, Compression

The repository contains two directories that separate results based on the simulation type that resulted in the SRGs contained in the directories.

Each directory is further divided based on the source of the networks, T-Garden networks are separated based on their original sources.

The subdirectories contain the archives of the SRGs grouped by networks.

The files have been compressed using zstd compression, make sure zstd is installed.

To uncompress, use:

```Bash
tar -xvf <network_name>.tar.zst
```

or, if an explicit script is needed and/or tar fails to identify zstd, the following can be used:


```Bash
zstd -d <network_name>.tar.zst
tar -xvf <network_name>.tar
```

## Archive contents, SRG types

The archives are separated based on simulation type:
- Deterministic SRGs
- PSRGs

There are also separate files for SR**L**Gs (links) and SR**R**Gs (resources).

All archives are named after the network they focus on, all archives contain .json files, where the SRG lists are present under one of the following tags, based on the simulation type and the result types: srgs, psrgs, srlgs, psrlgs, srlg_resilience_xs (x = 6..14), etc. Other tags are purely informative, and are a result of the parameters that were used for the runs, they serve the purpose of reproductibility but do not actually contribute to the SRG data.



The Deterministic SRG archives contain the following files:
- SRLGs that affect a given nodecount
    - `ksrlg_x (x = 0, 1, 2, 3, 4, 5, 10, 20, 50)`
- SRLGs based on graph distance (links that can be reached in at most x steps)
    - `srlg_graph_distance_h_x (x = 2, 3, 4, 5, 10)`
- Maximum union SRLGs, which contain all SRLGs affected by disk shaped events, regardless of their sizes. The sizes for these simulations range from 1 to 500 kms, with small incremental steps
    - `max_union_srlg`
- SRLG buckets for events of area extent between current bucket size and next bucket size. (bukcet 500+ covers all events from 500 km2 up to 2000 km2) All events appear in their smallest size bucket only. For an event having multiple appearances, the processed version should be considered. ***Most disk events are ommited, the results may be unconclusive. See fixed variants for full SRG buckets***
    - `srlg_x+ (x = 0, 500, 2000, 8000, 50000, 200000)`
- SRLG buckets fixed version, incorporates all events, regardless of shape. Events still only appear in their smallest size bucket.
    - `srlg_x+_fixed (x = 0, 500, 2000, 8000, 50000, 200000)`
- SRRG variants of all SRLGs up until this point
- SRLG buckets processed, that inclorporates duplicates for any event, by including the given event in not only their first appearance bucket but also all buckets where the event could be considered maximal SRLG. It also contains all events from the fixed versions.
    - `srlg_processed_x+ (x = 0, 500, 2000, 8000, 50000, 200000)`
    


The PSRG archives contain the following files:
- Earthquake PSRLGs with different resiliency levels - some files can contain empty lists, meaning, that with the provided parameters, the runs resulted in no PSRLG lists:
    - `psrlg_resilience_x_earthquake_FP (x = 6..14)`
- Uniform PSRLGs 
    - `psrlg_uniform_FP`
- Population based PSRLGs 
    - `psrlg_population_FP`
- Independent and Combined PSRLGs
    - `psrlg_independent_FP`
    - `psrlg_population+independent_FP`
    - `psrlg_uniform+independent_FP`
    - `psrlg_earthquake+independent_FP`
- SRRG variants of all SRLGs
- Postprocessed Independent and Combined PSRLGs with link FP values also incorporating endpoint FPs 
    - `psrlg_independent_postprocessed_FP`
    - `psrlg_population+independent_postprocessed_FP`
    - `psrlg_uniform+independent_postprocessed_FP`
    - `psrlg_earthquake+independent_postprocessed_FP`åå






### The T-Garden team

