# Data Dictionary

One row = one shipment. All values are synthetic.
`train.csv` has every column below. `test.csv` withholds the two **targets**
(`transit_days`, `delivered_on_time`). `solution.csv` holds those targets plus
`promised_days`, keyed by `shipment_id`.

## Identifiers & dates
| Column | Type | Description |
|--------|------|-------------|
| `shipment_id` | string | Unique shipment key (e.g. `SHP-42-0001234`). Note: duplicate *rows* exist as a cleaning exercise; de-dupe to one prediction per id. |
| `order_date` | date | When the order was placed. |
| `ship_date` | date | When the shipment was tendered to the carrier. |
| `ship_dow` | int | Day of week of `ship_date` (0 = Monday). |
| `ship_month` | int | Month of `ship_date` (1–12). |

## Shipment attributes
| Column | Type | Description |
|--------|------|-------------|
| `shipment_mode` | cat | `parcel` (~85%) or `ltl_freight` (~15%). |
| `carrier` | cat | Fictional carrier. Parcel: Falcon Post, Summit Express, Northstar Parcel, Cobalt Regional (west/mountain lanes only). Freight: Ironclad Freight, Vela Freight. **Values contain casing/whitespace noise.** |
| `service_level` | cat | Parcel: Ground, Expedited, TwoDay, Overnight. Freight: LTL Standard, LTL Guaranteed. **Contains casing noise.** |
| `origin_metro`, `dest_metro` | cat | Metro hub names. |
| `origin_zip`, `dest_zip` | string | 5-digit ZIP. **Some lost a leading zero (stored as int).** |
| `origin_lat`, `origin_lon`, `dest_lat`, `dest_lon` | float | Hub coordinates. |
| `distance_miles` | float | Great-circle origin→dest distance. |
| `zone` | int | Shipping zone 1–8 derived from distance. |

## Physical & handling
| Column | Type | Description |
|--------|------|-------------|
| `weight_lbs` | float | Actual weight. **~3% missing; ~1% recorded in grams (huge); ~0.5% zero/impossible.** |
| `length_in`, `width_in`, `height_in` | float | Package dimensions. **~2% each missing.** Dim weight = L·W·H / 139. |
| `pallet_count` | int | Pallets (freight; 0 for parcel). |
| `residential_flag` | 0/1 | Residential delivery. |
| `signature_required` | 0/1 | Signature service. |
| `declared_value_usd` | float | Declared value for insurance (0 if none). |

## Context features (known at ship time)
| Column | Type | Description |
|--------|------|-------------|
| `fuel_surcharge_pct` | float | Fuel surcharge fraction in effect. |
| `weather_severity` | float | 0–1 lane weather severity (higher in winter). |
| `carrier_congestion_index` | float | 0–1 network congestion (spikes in Nov–Dec). |
| `promised_days` | int | Carrier's published SLA commitment for the lane/service. |
| `carrier_cost_usd` | float | Billed shipping cost. Provided in all splits; useful for the Track C carrier recommendation and a cost-model stretch. |

## Targets (withheld in test.csv)
| Column | Type | Description |
|--------|------|-------------|
| `transit_days` | int | **Track A target.** Realized ship→delivery days. |
| `delivered_on_time` | 0/1 | **Track B target.** 1 if `transit_days ≤ promised_days`. |

## Signal you should expect to find
- `service_level` is the strongest driver of transit (Overnight < TwoDay < Expedited < Ground < LTL).
- Carriers differ systematically in speed **and** reliability (e.g. an economy carrier is cheap but late more often; a premium carrier is fast and reliable but costs more).
- Distance/`zone`, weekend ship days, peak season (Nov–Dec) congestion, and weather all push transit up.
- `carrier_cost_usd` tracks billable weight, zone, service, and carrier pricing — strong, learnable structure for a cost model.
