# Transit Buffer Map

## Prerequisites

Install Python 3, set up a virtualenv, and activate it. Then, run `pip install
-r requirements.txt`.

Optionally, install jq to pre-process data, which will speed things up.

## Download Data

Run the following commands to download the source data.

```
esri2geojson https://gis.hennepin.us/arcgis/rest/services/Maps/PROPERTY/MapServer/0 hennepin-county-property-map.geojson
esri2geojson https://maps.co.ramsey.mn.us/arcgis/rest/services/MapRamsey/MapRamseyOperational_AttributedParcel/MapServer/4 ramsey-county-property-map.geojson
esri2geojson https://gis.anokacountymn.gov/anoka_gis/rest/services/Parcels/MapServer/0 anoka-county-property-map.geojson
wget https://svc.metrotransit.org/mtgtfs/gtfs.zip
```

## Preprocess Data

This step discards unused fields in the GeoJSON files to speed up loading them
in GeoPandas.

```
jq -c '{"type": .type, "features": [ .features | .[] | {"geometry": .geometry, "type": .type} ]}' hennepin-county-property-map.geojson > hennepin-county-property-map-cleaned.geojson
jq -c '{"type": .type, "features": [ .features | .[] | {"geometry": .geometry, "type": .type} ]}' ramsey-county-property-map.geojson > ramsey-county-property-map-cleaned.geojson
jq -c '{"type": .type, "features": [ .features | .[] | {"geometry": .geometry, "type": .type} ]}' anoka-county-property-map.geojson > anoka-county-property-map-cleaned.geojson
```

## Run the Notebook

Run `jupyter-notebook`, open the notebook file, and run its cells. Loading,
projecting, intersecting, and plotting the geometry will each take a few
minutes.
