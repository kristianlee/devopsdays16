#The Mathematics of Reliability

This was one of the highlights of the conference for me. 

I recommend reading through the [presentation](http://avishai-ish-shalom.github.io/the-math-of-reliability/#/), but my notes about the details are below.

I'll try to find video of the talk, the notes don't do it justice!

##Alerting
Imagine a service experiencing 40% error rate, that's monitored in Nagios: 
Max_check_attempts=4
check_interval=15sec

The chance of getting a hard critical alert (i.e. it alerting at first attempt: only **2.6%**

The chances of not getting any alert after:

0.5hr - 45.9% 

1hr - 21.1%

1.5hr - 9.9%

These number's struck me as super-high!! Your users are much more likely to notice an issue before you get an alert. 

###Reliability

Define relaible:
	- "4 nines"
	- Mean time before failure
	- Failures per year
	- QoS

###Failure
Define failure:
	- System operating outside specified parameters
	- In reality though - users are complaining. 

Failure is subjective!

Possible states for detecting failure:
	- Working OK
	- Failure
	- Unknown
	- Fuzzy (we don't know what the correct state is - we have several different clocks, and we don't know what's wrong)

> "The abcense of alerts is not the evidence of proper operation"

Reliability Measures
MTBF - meantime between failures (years per failure)
Applied against a sample. 

**Statistical independence**: if you have a hard drive that goes bad, it doesn't make the other more likely to fail. 
**The hot hand fallacy** - my system isn't going to fail, because it hasn't failed for a long time. 
**The gamblers fallacy** - throw a coin, and get tails 9 times: obviously for the 10th you're going to bet heads right? NO, 50% chance each time. 

>Past performance does not predict future performance. 

Serial reliability: full system is less reliable than its worst part. Best ROI - improve the worst component: improvement is expensive. 

Parallel reliability (redundancy). Reliability of redundant system: Higher ROI with more reliable components. 
Not enough redundancy will reduce your reliability
N+1 rule only true for small clusters
Large clusters more cost effective. 

Statistically Dependent / Correlated Failures: 
	- Shared workload
	- Shared code (you have a race condition in the code which crashes the server, that is more likely to come up with the load on the server. 1 server fails, the LB moves all the traffic to less nodes, the load goes up + the probability of crash also goes up - bad!) 
	- Shared infrastructure

Backup and Operational sub-systems should avoid coupling with primaries. - monitoring system should not share components with production! 

Breathalyzer:
	- 1 out of 1000 drivers is drunk
	- Breath detects all drunks but has 5% false positives
	- Drivers stopped at random

What's the probablbility he's really drunk? Answer is not 95%, it's 2%.

Disable auto failover, greatly reduce false positives or use active/active. 

Microservices - circuit breakers are a must for distributed architectures. 

Queueing delay - throttle the services (queues!) http 503 error code. Apply backpressure. 

>"Reliability is everyone's responsibility"
