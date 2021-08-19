# Repository peds-midblocks-intersections

This repository serves as the distribution source for data that is used in the paper:

> Zuniga-Garcia, Natalia, Kenneth A. Perrine, and Kara M. Kockelman. Analysis of Pedestrian Crashes at Intersection and Midblock Segment Levels in Texas, under review for Transportation Research Record, 2021.

As referenced in the paper, these data consists of ~1-mile roadway segments that are derived from the 2018 version of the [TxDOT Roadway Invetory](https://www.txdot.gov/inside-txdot/division/transportation-planning/roadway-inventory.html), as well as street intersections that are derived from the same data source, with the assistance of [OpenStreetMap](https://www.openstreetmap.org/).

## Uniform ~1-mile Segments

Since geometry representation is a vital aspect of the uniform 1-mile segments, the 1-mile segments are provided as a ZIP file of an ArcGIS Shapefile. The non-geometric columns are also provided in CSV format. These are provided as the `segs_tx_1mi` table.

The fields are defined as follows:

* **road_gid:** Corresponds with `gid` found in the TxDOT Roadway Inventory, which identifies a roadway. The linear reference along that roadway would then be identified with `ref_begin`. The underlying TxDOT Roadway Inventory segment that overlaps the most with this "uniform" 1-mile segment can be uniquely identified with the `road_gid`/`frm_dfo` combination. Use this to key into TxDOT Roadway Inventory to find street name, road characteristics, etc.
* **ref_begin:** Linear reference (in miles) along the `gid` roadway that marks the beginning of the "uniform" 1-mile segment.
* **ref_end:** Linear reference (in miles) that marks the end of the "uniform" 1-mile segment.
* **seg_count:** Segment number counting from 1 to `seg_total` ordered by `ref_begin`.
* **seg_total:** Number of "uniform" 1-mile segments that comprise the entire length of the `gid` roadway.
* **seg_len:** Length of the specific segment (in miles), which is `ref_end - ref_begin`.
* **frm_dfo:** Exact linear reference key (in miles) into TxDOT Roadway Inventory that represents the underlying segment that overlaps this segment the most.
* **overlap:** Amount of overlap (in miles) to the underlying TxDOT Roadway Inventory segment keyed by the `gid`/`frm_dfo` combination.
* **on_system:** Signifies whether the underlying roadway segment is maintained by TxDOT or if it is a county or city road.
* **center_lat, center_lon:** Convenience coordinates of the center of the roadway geometry.
* **ident:** A string identifier that represents the roadway (e.g. street name or highway name)

## Intersections

Intersections were located using the methodology outlined in the paper referred above. Please contact Kenneth Perrine, owner of this repository, for further details. Intersections are given identifiers and geo-locations. An additional table identifies the two most frequented nearby roadway sections that cross through the intersection, labeled as major and minor approaches.

First, the `ints_tx` table is provided as a shapefile and also a CSV with latitude and longitude coordinates. This is the definitions for the fields:

* **int_id:** 

Second, the `ints_tx_appch` table estimates major and minor cross-streets, provided as a CSV file that is defined with these columns:

* **int_id:** Refers back to the `int_id` identified in `ints_tx.int_id`.
* **road_gid:** Identifies the corresponding roadway as found in `segs_tx_1mi` or the TxDOT Roadway Inventory
* **frm_dfo:** This is used along with `road_gid` to identify the roadway segment in TxDOT Roadway Inventory that corresponds with the location of the intersection. See note in `major` below about caveats.
* **lin_ref:** Linear reference (in miles) along the given roadway that is closest to the location associated with the intersection.
* **major:** Boolean that indicates whether this is the major or minor cross-street for the intersection. Since this is a simplistic characterization, intersections that have a third cross-street are not fully represented here. Also, for divided roadways as depicted in the TxDOT Roadway Inventroy, the directional carriageway is arbitrarily chosen.
