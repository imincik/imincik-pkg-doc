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
* spatialite (--enable-lwgeom=yes) (GIS b-deps: + liblwgeom)

* libgaiagraphics (GIS b-deps: libgeotiff, libproj, libxml2)
* librasterlight (GIS b-deps: libgeotiff, libspatialite, libproj)
* spatialite-tools (GIS b-deps: libspatialite, libgeos, libproj, libfreexl, libreadosm)
* spatialite-gui (GIS b-deps: libspatialite, libgeos, libproj, librasterlite, libgaiagraphics, libfreexl, libxml2)

* grass (GIS b-deps: libgdal, libproj)
* libgdal-grass

* mapserver (GIS b-deps: libgeos, libgdal, libproj, libxml2)
* qgis (GIS b-dep: grass, libgeos, libgdal, libproj, libspatialite, libspatialindex, libosgearth)
* osm2pgsql (GIS b-deps: libgeos, libxml2, libproj)
* pg-comparator


## SPATIALITE
* can't build with --enable-lwgeom=yes for the first time, because of circular dependency between spatialite, postgis and gdal. First build without lwgeom and after building postgis, build again with lwgeom.
* pyspatialite language binding only works with SpatialLite 3.0.1 (the last version before SpatiaLite amalgamation builds were deprecated). In order to use spatialite 4 from Python, sqlite 3 dynamic module loading must be used. sqlite3 package must be build with --enable-dynamic-extensions and python-pysqlite2 without 'define=SQLITE_OMIT_LOAD_EXTENSION'
* example Python code:

```
from pysqlite2 import dbapi2 as sqlite3

conn = sqlite3.connect(":memory:")
conn.enable_load_extension(True)
cur = conn.cursor()
cur.execute("SELECT load_extension('libspatialite.so.5')")
cur.execute("SELECT ST_Length(ST_GeometryFromText('LINESTRING(30 10, 10 30, 40 40)'))")
print cur.fetchone()[0]
conn.close()
```


## QGIS
Special upload procedure for QGIS:

git co release-2_2  
git fetch upstream  
git log ..upstream/release-2_2  
git merge upstream/release-2_2  
git co imincik/precise/release-2_2  
git merge release-2_2  

QGISVERSION=2.2.1  

DATE=$(date +%Y%m%d)  
CHANGESET=$(git rev-parse --short HEAD)  
DEBVERSION=+git$DATE~$CHANGESET~precise  

dch --newversion "${QGISVERSION}-1${DEBVERSION}1" "New release."  
git add debian/changelog  
git ci -m "Debian changelog update."  
git push --all  
git reset --hard  
git clean --force -dx  

debuild -S -sa -i -I  
dput ppa:imincik/gis-dev ../qgis_${QGISVERSION}${DEBVERSION}1_source.changes

## OSM2PGSQL
Package is taken from https://launchpad.net/~kakrueger/+archive/openstreetmap/+packages and rebuilded.
