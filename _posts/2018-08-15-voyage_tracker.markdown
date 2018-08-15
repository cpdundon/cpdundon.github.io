---
layout: post
title:      "Voyage Tracker"
date:       2018-08-15 17:26:43 +0000
permalink:  voyage_tracker
---

Over time, I have joined several sailing clubs.  They have a fleet of club boats and lend them to members – annual membership dues cover the cost of the fleet.  Sailing clubs are a lot of fun and you meet great people on the water.    

I built a tool to assist club management in tracking the status of their boats.  
* Who took out the equipment 
* Was damage done
* What equipment is in service
* Who sailed on a particular voyage
* What role did the sailors play on board
* When was a particular crew on board

There are three entities that interact in this application: Skippers, Voyages, and Boats.  A skipper leads a voyage on a boat.  A skipper has a username, eMail, and encrypted password.  A voyage references a skipper and a boat.  It also contains voyage specific fields.  A boat has a boat number, boat name, manufacturer, and additional boat specific fields.  

There are many voyages and a skipper can be linked to zero or more of them.  By the same token, a boat is linked to zero or more voyages.  Running the numbers on this pattern, we discover that it is the well known “Many to Many Through” relationship.  It is reasonably complex and fun to work with.

My main challenge was deleting the entities.  If I delete a skipper then all of her voyages are orphans.  This means that a voyage in the database referencing the deleted skipper is missing the skipper information.  Missing information is not desirable and downright bad.  Boats had the orphan problem too.  

On boats, I decided to add a flag called in_service.  If a boat is available to the fleet then that flag is set to On (true).  Otherwise it is Off (false).  The boat, however, can not be scuttled, e.g. destroyed or sunk.  You can, however, take it out of service.

Skippers are immortal – once in the system, a skipper can not be removed.  When a skipper exits the club, how do you handle it?  This is not really accounted for as the application is intended for a private network.  If someone leaves a club, they will not be able to get on premises to cause problems with the software.  This is a very strong assumption.  As an alternative, I could have set up a special system_admin user class who could disable logon for skippers.  The time to set up multiple user classes with secure views would have been prohibitive so I did not do it.  It could be added at a later time.

This feels like a solid proof of concept and I am looking forward to input from friends and family.

https://github.com/cpdundon/voyage_tracker_sinatra
