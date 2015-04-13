# Traffic Data Exchange Format Spec

Binary data location referencing format for exchanging traffic speed data. This repo contains documentation of data format and a reference ProtoBuff implementation.

```javascript
[ // array of segments
    { // segment 1 data
        "wayId" : "112233", // osm way id
        "startNodeId" : "445566", // osm node id for start vertex
        "endNodeId" : "778899", // osm node id for end vertex
        "startLat" : "10.123", // lat,lon pair for start vertex, optional, allows matching when node references change
        "startLon" : "-100.123",
        "endLat" : "10.125", // lat,lon pair for end vertex, optional, allows matching when node references change
        "endLon" : "-100.125",
        "length" : "1023", // length of segment for refernce, also allows comparison as OSM changes
        "trafficSpeeds" : {
            "average" : 12.23 // cumulative average for all times
            "hourOfWeekAverages" : [null,null,12.3 ...] // array of cumulative average by hour of week (Monday Midnight GMT is "hour zero". Nulls represent times with insufficent coverage, fall back to average)
        }
    },
    {
      //segment 2...
    }
]
```
