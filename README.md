# SRGarden

## Introduction, sources and licences

This repository contains Shared Risk Groups (SRGs) calculated for the networks of the T-Garden network repository, along with a few other selected networks.

> [!IMPORTANT]
> The SRG simulation data can be freely reused under the **CC BY 4.0** licence. If you use it, please cite the following article:
>
> *Citation to be added once the article is published.*

The repository contains deterministic SRGs and probabilistic SRGs (PSRGs) for a total of **176** networks from a variety of sources, broken down as follows:

| Source | Networks | Licence |
|---|---:|---|
| **[T-Garden](https://tgarden.net/)** | **160** | |
| ├ Generated | 116 | CC BY 4.0 |
| ├ [Topology Zoo](http://www.topology-zoo.org/) | 33 | CC BY 4.0 |
| └ [SNDlib](https://sndlib.put.poznan.pl/) | 11 | ZIB Academic License |
| **[JSACdata](https://github.com/jtapolcai/regional-srlg)** | **8** | GNU General Public License |
| **[TopoHub](https://topohub.org/)** | **8** | MIT License |
| **Total** | **176** | |

### Terminology

| Term | Meaning |
|---|---|
| **SRG** | Shared Risk Group: a set of network elements that can fail together as a result of a single event. |
| **PSRG** | Probabilistic SRG: an SRG with an associated failure probability. |
| **SRLG** | Shared Risk Link Group: an SRG made up of **links** only. |
| **SRRG** | Shared Risk Resource Group: an SRG made up of **nodes and links**. |
| **FP** | Failure probability. |

## Directory structure and compression

The repository contains two top-level directories, one for each simulation type (deterministic and probabilistic).

Each of these is divided into subdirectories by network source. T-Garden networks are further divided by their original source (Generated, Topology Zoo, SNDlib).

The subdirectories contain one archive per network.

> [!NOTE]
> The archives are compressed with **zstd**, so make sure zstd is installed before extracting them.

To extract an archive, use:

```bash
tar -xvf <network_name>.tar.zst
```

If your version of tar does not recognise zstd archives, decompress and extract in two steps instead:

```bash
zstd -d <network_name>.tar.zst
tar -xvf <network_name>.tar
```

## Archive contents and SRG types

Each archive is named after the network it covers and contains `.json` files. Separate files are provided for SR**L**Gs (links only) and SR**R**Gs (nodes and links).

> [!TIP]
> **File naming:** every SRRG file has the same name as its SRLG counterpart, with the `l` replaced by an `r`.
> For example, `ksrlg_5` → `ksrrg_5`, and `psrlg_uniform_FP` → `psrrg_uniform_FP`.

In each `.json` file, the SRG list is stored under one of the following keys, depending on the simulation type and result type: `srgs`, `psrgs`, `srlgs`, `psrlgs`, `srlg_resilience_xs` (x = 6–14), etc.

> [!NOTE]
> All other keys only record the parameters used for the run. They are kept for reproducibility and do not contain SRG data.

### Deterministic SRG archives

| Contents | Files | SRRG variant |
|---|---|:---:|
| SRLGs affecting a given number of nodes (x) | `ksrlg_x`<br>x = 0, 1, 2, 3, 4, 5, 10, 20, 50 | ✅ |
| SRLGs based on graph distance: links that can be reached in at most x hops | `srlg_graph_distance_h_x`<br>x = 2, 3, 4, 5, 10 | ✅ |
| Maximum-union SRLGs: the union of all SRLGs produced by disk-shaped events, across all event sizes. Disk **radii** range from 1 to 500 km in small increments. | `max_union_srlg` | ✅ |
| SRLG buckets, grouped by event area (see [Bucket ranges](#bucket-ranges)). Each event appears only in the smallest bucket it fits. ⚠️ **Incomplete, see the warning below.** | `srlg_x+`<br>x = 0, 500, 2000, 8000, 50000, 200000 | ✅ |
| SRLG buckets, **fixed**: include all events, regardless of shape. Each event still appears only in the smallest bucket it fits. | `srlg_x+_fixed`<br>x = 0, 500, 2000, 8000, 50000, 200000 | ✅ |
| SRLG buckets, **processed**: include each event in the first bucket it appears in and in every other bucket where it counts as a maximal SRLG, so an event can appear in more than one bucket. These files also contain all events from the fixed variants. | `srlg_processed_x+`<br>x = 0, 500, 2000, 8000, 50000, 200000 | ❌ |

> [!WARNING]
> Most disk-shaped events are missing from the plain `srlg_x+` bucket files, so results based on them may be inconclusive. Use the `srlg_x+_fixed` files for complete SRLG buckets, or the `srlg_processed_x+` files to get every bucket an event belongs to.

#### Bucket ranges

Bucket `x+` contains events with an area of at least x km² and less than the next bucket's lower bound:

| Bucket | Event area |
|---|---|
| `0+` | 0 – < 500 km² |
| `500+` | 500 – < 2,000 km² |
| `2000+` | 2,000 – < 8,000 km² |
| `8000+` | 8,000 – < 50,000 km² |
| `50000+` | 50,000 – < 200,000 km² |
| `200000+` | ≥ 200,000 km² (no upper bound) |

### PSRG archives

| Contents | Files | SRRG variant |
|---|---|:---:|
| Earthquake PSRLGs at different resilience levels. Some files contain empty lists, meaning that no PSRLGs resulted from the runs with the given parameters. | `psrlg_resilience_x_earthquake_FP`<br>x = 6–14 | ✅ |
| Uniform PSRLGs | `psrlg_uniform_FP` | ✅ |
| Population-based PSRLGs | `psrlg_population_FP` | ✅ |
| Independent and combined PSRLGs | `psrlg_independent_FP`<br>`psrlg_population+independent_FP`<br>`psrlg_uniform+independent_FP`<br>`psrlg_earthquake+independent_FP` | ✅ |
| Postprocessed independent and combined PSRLGs, in which each link's FP also includes the FPs of its endpoint nodes | `psrlg_independent_postprocessed_FP`<br>`psrlg_population+independent_postprocessed_FP`<br>`psrlg_uniform+independent_postprocessed_FP`<br>`psrlg_earthquake+independent_postprocessed_FP` | ❌ |

---

### The SRGarden team
