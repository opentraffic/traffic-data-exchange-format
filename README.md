# Traffic Data Exchange Format

Binary location referencing format for exchanging traffic speed data. This repo contains documentation of data format and a reference ProtoBuff implementation. 

Data is stored as web mercator tiles. Segments with start node inside tile are included. 

Tiles with baseline statistics contain cumulative average and quartiles for all collected data. Tiles with current conditions only contain segments with data within specificed timew window. Current conditions tiles can be further filtered to only contain segments showing specified percent change over baseline average for time window.

Baseline tile
```javascript
{
    "header" : {
        // osm commit id from basemap 
        "osmCommitId" : 1,

        // unix timestamp of file creation
        "creationTimestamp" : 2,

        // web mercator tile coordinate for segments contained in this file (startNodeId of all segments falls within tile boundary)
        "tileX" : 3,
        "tileY" : 4,
        "tileZ" : 5
    },
    segments = [ // array of segments
        { // segment 1 data
            "segment" : {
                
                // osm way id
                "wayId" : 112233, 
                // osm node id for start vertex
                "startNodeId" : 445566, 
                // osm node id for end vertex
                "endNodeId" : 778899,

                 // lat,lon pair for start vertex, optional, allows matching when node references change 
                "startLat" : 10.123,
                "startLon" : -100.123,

                // lat,lon pair for end vertex, optional, allows matching when node references change
                "endLat" : 0.125, 
                "endLon" : -100.125,

                // length of segment for refernce, also allows comparison as OSM changes
                "length" : 1023, 
            }

           // cumulative average for all times
            "averageSpeed" : 12.23 
           
            // array of cumulative average by hour of week (Monday Midnight GMT is "hour zero". Nulls represent times with insufficent coverage, fall back to average)
            "hourOfWeekAverages" : [null,null,12.3 ...] 

            // speed at top quartile 
            "topQuartile" : 20.1,
            // quartile for hour of week bin
            "hourOfWeekTopQuartile" : [23.0,20.023...],

            // speed at bottom quartile
            "bottomQuartile" : 9.00;
            // quartile for hour of week bin
            "hourOfWeekBottomQuartile" : [8.2, 7.8...];
        },
        {
          // segment 2
        } ...
    ]
}
```


Current conditions tile
```javascript
{
    "header" : {
        // osm commit id from basemap 
        "osmCommitId" : 1,

        // unix timestamp of file creation
        "creationTimestamp" : 2,

        // web mercator tile coordinate for segments contained in this file (startNodeId of all segments falls within tile boundary)
        "tileX" : 3,
        "tileY" : 4,
        "tileZ" : 5
    },

    "percentChangeThreshold" : 0.05,

    segments = [ // array of segments
        { // segment 1 data
            "segment" : {
                
                // osm way id
                "wayId" : 112233, 
                // osm node id for start vertex
                "startNodeId" : 445566, 
                // osm node id for end vertex
                "endNodeId" : 778899,

                 // lat,lon pair for start vertex, optional, allows matching when node references change 
                "startLat" : 10.123,
                "startLon" : -100.123,

                // lat,lon pair for end vertex, optional, allows matching when node references change
                "endLat" : 0.125, 
                "endLon" : -100.125,

                // length of segment for refernce, also allows comparison as OSM changes
                "length" : 1023, 
            },

            // start/end time window for stats
            "currentWindowStartTimestamp" : 234234,
            "currentWindowEndTimestamp" : 234232,

            // average, top, bottom quartile speeds (m/s) for timewindow
            "currentAverageSpeed" : 12.23 
            "currentTopQuartile" : 23.0,
            "currentBottomQuartile" : 8.2
        },
        {
          // segment 2
        } ...
    ]
}
```
