---
title: "Check All Agent Skills in Cisco UCCX via CLI 11.6"
date: 2023-01-09
tags: [
    "Networking",
    "Cisco",
]
draft: false
---

# Intro
UCCX 11.6 (and likely above) does not provide a native way in the UI to display all Agents and their levels.  The sql command below can be executed and will toss out all agents with theirs skills to be used.  This can be tossed into Excel to be analyzed.


# Execute
```
run uccx sql db_cra select s.skillname, rsm.competencelevel, r.resourceLoginID,
 r.extension, r.resourceFirstName, r.ResourceLastName,t.teamname from skill s
 inner join resourceskillmapping rsm on s.skillid = rsm.skillid
 inner join resource r on rsm.resourceskillmapid = r.resourceskillmapid join team t on
 r.assignedteamid = t.teamid where s.active = 't' and r.active = 't'
 order by s.skillname, competencelevel, resourceloginid
```

Should output something like:

```
SKILLNAME     COMPETENCELEVEL RESOURCELOGINID EXTENSION  RESOURCEFIRSTNAME  RESOURCELASTNAME  
TEAMNAME

--------------------------------------------------------------------------------------------
Skill1  5     agent1  62121           Agent1  Default
Skill1  5     agent2  62131           Agent2  Default
Skill1  5     arunabh 62000           CIPC    Default

```

Taken from:  https://www.cisco.com/c/en/us/support/docs/customer-collaboration/unified-contact-center-express/118987-check-skill-map-uccx-00.html