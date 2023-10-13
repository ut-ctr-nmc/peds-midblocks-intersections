# Repository peds-midblocks-intersections

This repository serves as the distribution source for data that is used in the paper:

Zuniga-Garcia, Natalia, Kenneth A. Perrine, and Kara M. Kockelman. Predicting Pedestrian Crashes in Texas’ Intersections and Midblock Segments, under review, Sustainability.

As referenced in the paper, these data consists of ~1-mile roadway segments that are derived from the 2018 version of the [TxDOT Roadway Invetory](https://www.txdot.gov/inside-txdot/division/transportation-planning/roadway-inventory.html), as well as street intersections that are derived from the same data source, with the assistance of [OpenStreetMap](https://www.openstreetmap.org/).

## Data Files Location

Because of file size limits that are a part of GitHub accounts, the large data files are located here: [https://utexas.box.com/v/peds-midblocks-intersections](https://utexas.box.com/v/peds-midblocks-intersections).

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

## Uniform ~0.1-mile Segments

There is also another dataset with the same schema as the ~1-mile segments, but representing the TxDOT Roadway Inventory chunked into segments that are approx. 0.1 miles in length. They are provided under the name `segs_tx_01mi`.

## Intersections

Intersections were located using the methodology outlined in the paper referred above, detailed in the [peds-crash-techvol repository](https://github.com/ut-ctr-nmc/ped-crash-techvol/blob/master/doc/intersections.md#attempt-3-using-openstreetmap). Please contact Kenneth Perrine, owner of this repository, for further details. Intersections are given identifiers and geo-locations. An additional table identifies the two most frequented nearby roadway sections that cross through the intersection, labeled as major and minor approaches.

### ints_tx

First, the `ints_tx` table is provided as a shapefile and also a CSV with latitude and longitude coordinates. This is the definitions for the fields:

* **int_id:** Arbitrarily-assigned, unique identifier for each intersection
* **signal:** Boolean signifying whether the corresponding OpenStreetMap intersection has a signal flag associated with it
* **junction:** Boolean signifying if the intersection is actually an expressway junction (e.g. a merging point for an on-ramp). Please note that in the lastest methodology, intersection-finding around expressways is subject to a number of errors. To increase reliability for idenifying traditional urban intersections, it may be appropriate to filter out all records where junction is set.
* **signal_mid:** Boolean signifying whether the intersection marks the location of a midblock signal, where the signal likely does not serve a crossing of two or more public roads. This may be set in places where a midblock crosswalk is signalized.
* **descr:** A string identifier for the intersection. Normally, this would be the name of the major cross-street followed by an ampersand and the name of the minor cross-street. In many cases, this is NULL, indicating uncertainty in determining the cross-streets for the intersection. It is intended that this will be corrected in a future rendition of this data set.
* **lat/lon** or **center:** Location estimated for the intersection

### ints_tx_appch

Second, the `ints_tx_appch` table estimates major and minor cross-streets, provided as a CSV file that is defined with these columns:

* **int_id:** Refers back to the `int_id` identified in `ints_tx.int_id`.
* **road_gid:** Identifies the corresponding roadway as found in `segs_tx_1mi` or the TxDOT Roadway Inventory via `gid`
* **frm_dfo:** This is used along with `road_gid` to identify the roadway segment in TxDOT Roadway Inventory that corresponds with the location of the intersection. See note in `major` below about caveats.
* **lin_ref:** Linear reference (in miles) along the given roadway that is closest to the location associated with the intersection. This is `NULL` in the few places where disjoints in the TxDOT Roadway Inventory geometry prevented the estimation of the linear reference.
* **major:** Boolean that indicates whether this is the major or minor cross-street for the intersection. Since this is a simplistic characterization, intersections that have a third cross-street are not fully represented here. Also, for divided roadways as depicted in the TxDOT Roadway Inventroy, the directional carriageway is arbitrarily chosen.
