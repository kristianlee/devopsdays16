#An ElasticSearch Cluster Named George Armstrong Custer

This talk was based around telling the story of how 'Trax' had dealt with a misbehaving elasticsearch cluster when it got scaled up.  

I'll try to find the slides (I had a link, but the link is broken) - this was a great example of a 'story' based tech talk that worked really well (used General Custer to illustrate (literally!) their point. 

##Problem
- They built an ES cluster, it was working well for their logs, and so they though, why not use it for everything search-related. 
- When they brough more information in though, they found that ES started crashing - the fix was to restart the ES node (they set up some automated reboots too). 
- Obviously not practical, so they started collecting the stats for the ES service from the ES API. 
- Nothing obvious in the stats, so what to do:

##Diagnosis
- They brought up a new cluster with only the smaller logs ingesting. 
- Found that when they compared the stats across the 2 clusters, it appears the JVM heap's garbage collection was to blame. 
- Whenever the JVM heap went above a level, the service would crash. 

##Resolution
- They set up alerting, so that whenever the JVM heap went above a certain level, they could alert on it. 
- When the alerts came through, they would either remove unused indices, or add extra nodes to the cluster. 
- They also realised that given that ES monitoring comes from the ES API it's not much good if ES has crashed. They therefore started measure API calls through a load balancer, so that they'd know when something was up. 