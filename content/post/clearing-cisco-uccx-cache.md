---
title: "Clearing Cisco UCCX CUIC Cache"
tags: [
    "VOIP",
]
date: 2022-05-04
draft: false
---

CUIC is available by navigating to the node IP with port 8444 (https://<nodename/ip>:8444)

The following information was directed to me to be done by Cisco TAC is a case where I had very slow performance in CUIC and our Wallboard software on certain UCCX Node.  According to Cisco this can be done during the day but out of caution I performed this in off hours.  This was also done on both nodes

If the queries to the CUIC database or Wallboard are running slow we may need to clear the Cache to speed up queries.  

##### To restart Database services (not production impact)
```
utils service restart Cisco Unified CCX Database
``` 
##### To clear the CUIC cache
```
run sql delete from cuic_data:cuicdataset
run sql delete from cuic_data:cuicstatisticsdatasets
run sql delete from cuic_data:cuicstatisticsviews
run sql delete from cuic_data:cuicdatasetreplicated
```
 
##### Restart CUIC services
```
utils service restart Cisco Unified Intelligence Center Reporting Service
utils service restart Cisco Unified Intelligence Center Serviceability Service
```
 
##### Restart live data communications
```
utils service restart Cisco Unified CCX Socket.IO Service
```

##### Execute following command from CLI to sync all users and permissions
```
utils uccx syncusers
utils uccx synctocuic 
```