# Availability

How resistant a system is to failures (Fault Tolerance)

You measure availability by % of a systems uptime in a given year.

## Nines

Since any sort of deviation from 100% is bad, we measure availability in nines.

- 99% - 2 nines - 3.65 days/year
- 99.9% - 3 nines - 8.77 hours/year
- 99.95% - 3 and a half nines - 4.38 hours/year
- 99.99% - 4 nines - 52.60 minutes / year
- 99.999% - 5 nines - 5.26 minutes / year
- 99.99999 - 7 nines - 3.16 seconds / year

5 nines is the gold standard.

## SLA/SLO

Service level agreement (SLA) - collection of guarantees given to a customer by a service provider made up by multiple SLOs.

Service level objective (SLO) - a guarantee given to a customer by a service provider such as guaranteeing availability.

Example - Cloud Spanner (Google Cloud) SLA

- Regional Instances - 4 nines
- Multi-Regional Instances - 5 nines

If they don't meet their SLAs, you will receive a percentage of your money back.

## Redundancy

Helps us eliminate single points of failure by duplicating different parts of our system.

If the server is getting over loaded, add more servers and a load balancer to distribute traffic to different servers.

Well what if our load balancer gets over loaded? Add more load balancers! 🤷‍♀️

There are two kinds of redundancy: passive and active redundancy and I personally think that the names are counterintuitive; however, it makes more sense when thinking of leader election for active redundancy.

### Passive Redundancy

All load balancers and servers are being used at the same time. If one server or load balancer dies the other servers will handle the load until that machine is fixed.

Think of a twin engine aircraft. If one dies, the airplane can still fly.

### Active Redundancy

There may be many servers but only one is being used at at given time. If it fails the rest of the machines will reconfigure themselves go through leader election to route traffic to a new server.
