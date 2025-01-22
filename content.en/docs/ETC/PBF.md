---
type: 'docs'
bookCollapseSection: false
bookHidden: false
bookToC: true
bookComments: false
bookSearchExclude: false
bookFlatSection: true
weight: 1
---
# What is PBF?

PBF means “Protocolbuffer Binary Format”.

PBF is
1. intended as an alternative to the XLM format.
2. about half of the size of a gzipped planet.
3. about 30% smaller than a bzipped planet.
4. about 5x faster to write than a gzipped planet.
5. about 6x faster to write than a bzipped planet.
6. designed to support future extensibility and flexibility.
> What is gzipped/bzipped planet?

Files have the extension **`*.osm.pbf`.**

The reference implementation of PBF is **the Osmosis implementation**, split into two parts, (1)the Osmosis-specific part contained in the Osmosis repository at and (2)an application-generic part at.
* This application-generic part is used to (1)build the `osmpbf.jar` as used in Osmosis and other Java-based PBF readers and also (2)contains the master definition of the PBF protocol buffer definitions(`*.proto` files).

The underlying file format is chosen to support random access at the ‘fileblock’ granularity. Each ‘fileblock’ is independently decodable and contains a series of encoded ‘PrimitiveGroups’ with each PrimitiveGroup containing ~8k OSM entities in the default configuration.
> It means that OSM entities are encoded into fileblocks.

# References
* https://wiki.openstreetmap.org/wiki/PBF_Format
