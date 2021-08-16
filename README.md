# Repository peds-midblocks-intersections

This repository serves as the distribution source for data that is used in the paper:

> Zuniga-Garcia, Natalia, Kenneth A. Perrine, and Kara M. Kockelman. Analysis of Pedestrian Crashes at Intersection and Midblock Segment Levels in Texas, under review for Transportation Research Record, 2021.

As referenced in the paper, these data consists of ~1-mile roadway segments that are derived from the 2018 version of the [TxDOT Roadway Invetory](https://www.txdot.gov/inside-txdot/division/transportation-planning/roadway-inventory.html), as well as street intersections that are derived from the same data source, with the assistance of [OpenStreetMap](https://www.openstreetmap.org/).

## Uniform ~1-mile Segments

Since geometry representation is a vital aspect of the uniform 1-mile segments, The 1-mile segments are provided as a ZIP file of an ArcGIS Shapefile.

```sql
CREATE TABLE uniform_segs_1mi (
  road_gid integer,
  ref_begin real,
  ref_end real,
  seg_count integer,
  seg_total integer,
  seg_len real,
  frm_dfo_cl numeric,
  overlap real,
  on_system boolean,
  center_lat real,
  center_lon real,
  geog geography,
  PRIMARY KEY (road_gid, ref_begin)
);
```

The fields are defined as follows:
* **road_gid:** Corresponds with `gid` found in the TxDOT Roadway Inventory, which identifies a roadway. The linear reference along that roadway would then be identified with `ref_begin`. The underlying TxDOT Roadway Inventory segment that overlaps the most with this "uniform" 1-mile segment can be uniquely identified with the `road_gid`/`frm_dfo_cl` combination. Use this to key into TxDOT Roadway Inventory to find street name, road characteristics, etc.
* **ref_begin:** Linear reference along the `gid` roadway that marks the beginning of the "uniform" 1-mile segment.
* **ref_end:** Linear reference that marks the end of the "uniform" 1-mail segment.
* **seg_count:** Segment number counting from 1 to `seg_total` ordered by `ref_begin`.
* **seg_total:** Number of "uniform" 1-mile segments that comprise the entire length of the `gid` roadway.
* **seg_len:** Length of the specific segment, which is `ref_end - ref_begin`.
* 
