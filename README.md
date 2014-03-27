Here are some notes for my Ubuntu packaging in ppa:imincik (https://launchpad.net/~imincik).

# Build order

* libsvg
* libsvg-cairo
* libxml2
* libgeotiff
* libkml (GIS b-deps: NO)
* readosm (GIS b-deps: NO)
* freexl (GIS b-deps: NO)

* proj
* geos

* spatialite

* gdal
* libgdal-ecw
* libgdal-mrsid

* postgis

* libgaiagraphics (GIS b-deps: libgeotiff, libproj, libxml2)
* spatialite-tools
* spatialite-gui

* grass
* libgdal-grass

* mapserver
* qgis
* osm2pgsql
* pg-comparator
