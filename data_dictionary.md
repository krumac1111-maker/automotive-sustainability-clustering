# Data Dictionary (Manufacturer-level dataset)

This dictionary documents the manufacturer-level variables constructed by aggregating vehicle-level WLTP/Euro 6 rows.

## Raw dataset columns used
| Raw column | Meaning (dataset) | Used for |
|---|---|---|
| `Manufacturer` | Brand/manufacturer name | Grouping key |
| `Fuel Type` | Fuel descriptor (e.g., Petrol, Diesel, Electricity, hybrid strings) | Create `fuel_cat` |
| `WLTP CO2` | WLTP CO₂ emissions | Build `mean_co2` |
| `WLTP Metric Combined` | Combined WLTP metric measure (dataset-defined) | Build `efficiency` |
| `wh/km` | Electric energy consumption | Optional (checked/cleaned; not required in your current feature set) |

## Derived variables (used in clustering)
| Variable | Type | Unit | How it is constructed | Why it matters |
|---|---|---:|---|---|
| `n_variants` | integer | count | Number of vehicle rows per `Manufacturer` (`size`) | Indicates sample size per manufacturer |
| `mean_co2` | numeric | gCO₂/km | Mean of `WLTP CO2` per manufacturer | Tailpipe carbon emissions proxy |
| `efficiency` | numeric | dataset-defined | Mean of `WLTP Metric Combined` per manufacturer | Energy efficiency proxy |
| `share_bev` | numeric | proportion (0–1) | Mean of `(fuel_cat == "BEV")` per manufacturer | Clean-transition proxy (BEV adoption) |
| `share_hybrid` | numeric | proportion (0–1) | Mean of `(fuel_cat == "Hybrid")` per manufacturer | Transition pathway via hybridisation |
| `share_ice` | numeric | proportion (0–1) | Mean of `(fuel_cat == "ICE")` per manufacturer | Reliance on conventional powertrains |

## Fuel category rule (`fuel_cat`)
The notebook maps `Fuel Type` into adoption categories:
- If fuel type equals **Electricity** → `BEV`
- Else if fuel type contains the word **Electricity** → `Hybrid` (plug-in / electricity-containing label)
- Else if fuel type is **Petrol** or **Diesel** → `ICE`
- Else → `Other` (not used in the BEV/Hybrid/ICE shares unless you include it)

## Notes
- If you decide to include electric efficiency in future, you can create an additional feature from `wh/km` (e.g., mean `wh/km` for BEVs only), but keep it consistent across manufacturers and handle missing values carefully.
