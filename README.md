# 30-Feeder Distribution Network Dataset

## Overview

This release provides the topology, electrical parameters, node-level load and renewable-generation data, and fixed candidate interconnection points of a 30-feeder distribution network. The data are consolidated in `30_feeder_complete_dataset.xlsx`.

The original 30-feeder case used for the manuscript's field-data validation was obtained from an urban distribution network in southern China. Its original topology and measurements cannot be released under the utility confidentiality agreement. The workbook in this repository is a nonconfidential reconstruction consistent with the disclosed scale and structure of that field case; it is not the utility's original network or original measurements.

## System Summary

| Item | Value |
|---|---:|
| Nominal voltage | 10 kV |
| Substations | 5 |
| Radial feeders | 30 |
| Feeder nodes | 236 |
| Feeder-internal branches | 206 |
| Substation-to-feeder source connectors | 30 |
| Nodes with renewable generation | 42 |
| Fixed candidate interconnection points | 30 |
| Time steps per node | 24 |
| Node-hour profile records | 5,664 |
| SOP candidate links in the 40-node subset | 3 |

The `x` and `y` fields are schematic coordinates used to draw the network. They are not geographic coordinates and cannot be used to identify the real utility system.

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

One candidate interconnection point is fixed for each feeder and recorded in the `Tie Points` worksheet. In this public reconstruction, the selected point is a terminal node of the corresponding radial feeder. This assignment provides a common connection rule for evaluating the 30 x 29 / 2 = 435 unordered feeder pairs. It does not disclose or reproduce the confidential utility's original interconnection-point locations.

## Selected 40-Node Subset

Four feeders form the 40-node case used elsewhere in the manuscript:

| Feeder | Nodes | Fixed candidate tie node |
|---|---:|---|
| `F04` | 11 | `N011` |
| `F11` | 6 | `N017` |
| `F19` | 11 | `N028` |
| `F27` | 12 | `N040` |

The `selected_for_40_bus_case` and `is_40_bus` fields identify this subset. The `SOP Candidates` worksheet lists its three candidate SOP links.

## Relation to the Table IV Pairwise Experiment

This workbook documents the public test-system inputs. It does not contain the confidential field-network outputs from the 435 full RHC optimizations, the original pair labels, or the reported split of 20 positive and 415 negative feeder pairs.

For a complete reproduction of Table IV, a companion pair-level results file should report, for every unordered feeder pair:

- the two feeder identifiers and their fixed interconnection nodes;
- the baseline RHC and the RHC after enabling the candidate SOP link;
- the common SOP capacity, investment constraint, operating constraints, and time-series inputs;
- the numerical rule and threshold used to assign the positive or negative label; and
- the ISTCD, Pearson-correlation, mutual-information, and Euclidean-distance scores used for comparison.

The similarity measures must not be used to construct the ground-truth labels. The labels must be obtained independently by solving the full RHC optimization model under the same settings for all 435 feeder pairs.

The reported 20/415 label split must not be attributed to this reconstructed workbook unless all 435 optimizations are rerun using these released inputs and the resulting pair-level outputs are added to the public release.
