Here are some notes for my Ubuntu packaging in ppa:imincik (https://launchpad.net/~imincik).

# Build order

* libsvg
* libsvg-cairo
* libxml2
* libgeotiff
* libkml (GIS b-deps: NO)
* readosm (GIS b-deps: NO)
* freexl (GIS b-deps: NO)

* proj (GIS b-deps: NO)
* geos (GIS b-deps: NO)

* spatialite (GIS b-deps: libgeos, libproj, libfreexl, libxml2)

* gdal (GIS b-deps: libgeos, libproj, libxml2, libspatialite, libfreexl, libkml)
* libgdal-ecw
* libgdal-mrsid

* postgis (GIS b-deps: libgeos, libproj, libxml2, libgdal)

* libgaiagraphics (GIS b-deps: libgeotiff, libproj, libxml2)
* librasterlight (GIS b-deps: libgeotiff, libspatialite, libproj)
* spatialite-tools (GIS b-deps: libspatialite, libgeos, libproj, libfreexl, libreadosm)
* spatialite-gui (GIS b-deps: libspatialite, libgeos, libproj, librasterlite, libgaiagraphics, libfreexl, libxml2)

* grass (GIS b-deps: libgdal, libproj)
* libgdal-grass

* mapserver (GIS b-deps: libgeos, libgdal, libproj, libxml2)
* qgis
* osm2pgsql
* pg-comparator
