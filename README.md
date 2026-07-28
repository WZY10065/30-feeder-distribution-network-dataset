<img width="2552" height="1218" alt="image" src="https://github.com/user-attachments/assets/76be654c-4279-4982-8a84-b59bc6e2d8ba" /># 30-Feeder Distribution Network Dataset

## Reference

[1] Y. Wu, T. Yu, and Z. Wang, “Spatio-Temporal Complementarity-Driven Planning for Cost-Effective Renewable Hosting Capacity Enhancement in Flexible Power Distribution Networks,” International Journal of Electrical Power & Energy Systems, 2026.

## Overview

This repository provides the publicly available 30-feeder distribution-network data and interactive topology viewer used to support the feeder-pair screening and 40-bus case studies in the manuscript [1]. This release provides the topology, electrical parameters, node-level load and renewable-generation data, and fixed candidate interconnection points of a 30-feeder distribution network. The data are consolidated in `30_feeder_complete_dataset.xlsx`.

The source case was obtained from a 10 kV urban distribution network in southern China. To comply with the utility confidentiality agreement, this public release does not contain identifiable geographic information, the utility's original topology, or original measurements.

<img width="2552" height="1226" alt="image" src="https://github.com/user-attachments/assets/cd14a857-2a73-4384-ad1e-2040bcc440cf" />


## System Summary

| Item | Value |
|---|---:|
| Nominal voltage | 10 kV |
| Substations | 5 |
| Radial feeders | 30 |
| Feeder nodes | 236 |
| Mean nodes per feeder | 7.867 |
| Feeder-internal branches | 206 |
| Substation-to-feeder source connectors | 30 |
| Nodes with renewable generation | 42 |
| Fixed candidate interconnection points | 30 |
| Time steps per node | 24 |
| Node-hour profile records | 5,664 |
| SOP candidate links in the 40-node subset | 3 |

The `x` and `y` fields are schematic coordinates used to draw the network. They are not geographic coordinates and cannot be used to identify the real utility system.

## Interactive Network Viewer

The interactive 30-feeder topology viewer is available at:

https://wzy10065.github.io/30-feeder-distribution-network-dataset/

The viewer supports node-ID, mean-load, and mean-renewable labels; highlighting of the four feeders that form the 40-bus case; display of SOP candidate links and fixed candidate interconnection points; and node-level 24-hour load and renewable-output profiles.

## Workbook Contents

| Worksheet | Data rows | Description |
|---|---:|---|
| `Overview` | - | Dataset scale and workbook contents. |
| `Data Dictionary` | - | Field definitions, data types, and units. |
| `Substations` | 5 | Substations and schematic coordinates. |
| `Feeders` | 30 | Feeder roots, fixed tie nodes, topology counts, load totals, renewable totals, and 40-node selection flags. |
| `Nodes` | 236 | Node attributes, load parameters, renewable parameters, schematic coordinates, and selection flags. |
| `Node Averages` | 236 | Mean load and mean renewable output for each node. |
| `Node Profiles 24h` | 5,664 | Hourly active load and renewable output for every node. |
| `Branches` | 236 | Source connectors and feeder-internal branch parameters. |
| `Tie Points` | 30 | One fixed candidate interconnection point for each feeder. |
| `SOP Candidates` | 3 | SOP candidate links in the selected 40-node subset. |

Each data worksheet is a flat table. Identifiers such as `substation_id`, `feeder_id`, and `node_id` link the tables. Missing identifiers are represented by blank cells.

## Load and Renewable-Generation Data

The `Nodes` worksheet contains the peak and 24-hour mean active load of each node, its load category, renewable technology, reference renewable capacity, and renewable-capacity upper bound. The `Node Profiles 24h` worksheet contains `load_mw` and `renewable_mw` for hours 1-24.

The workbook distinguishes the displayed physical-scale load values from the values used by the planning model:

```text
model_peak_load_mw = peak_load_mw x optimization_load_scale
model_mean_load_mw = mean_load_mw x optimization_load_scale
optimization_load_scale = 0.1
```

Power and capacity values are reported in MW or MVA, voltage in kV, line length in km, and branch resistance and reactance in ohm.

## Fixed Interconnection Points

One candidate interconnection point is fixed for each feeder and recorded in the `Tie Points` worksheet. In this public dataset, the selected point is a terminal node of the corresponding radial feeder. This assignment provides a common connection rule for evaluating the 30 x 29 / 2 = 435 unordered feeder pairs. It does not disclose the confidential utility's original interconnection-point locations.

## Selected 40-Node Subset

Four feeders form the 40-node case used elsewhere in the manuscript:

| Feeder | Nodes | Fixed candidate tie node |
|---|---:|---|
| `F04` | 11 | `N011` |
| `F11` | 6 | `N017` |
| `F19` | 11 | `N028` |
| `F27` | 12 | `N040` |

The `selected_for_40_bus_case` and `is_40_bus` fields identify this subset. The `SOP Candidates` worksheet lists its three candidate SOP links.


The similarity measures must not be used to construct the ground-truth labels. The labels must be obtained independently by solving the full RHC optimization model under the same settings for all 435 feeder pairs.

The reported 20/415 label split must not be attributed to this public workbook unless all 435 optimizations are rerun using these released inputs and the resulting pair-level outputs are added to the public release.
