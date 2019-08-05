# Loadbalancers

# Types of Load Balancers
1. Application LB
2. Network LB
3. Classic LB

# Application LB
- Best suited for HTTP and HTTPS type traffic
- Operating at application layer, it enables user to create advances request routing
- It allows user to send certain web request (based on url) to certain servers

# Network LB
- Best suited for situations where extreme performance is required
- Operating at network level, it is able to handle millions of requests with very low latency

# Classic LB
- It is a legacy LB that amazon does not recommend
- It is best of both the worlds (app and n/w), as one can use simple layer-7 specific features like x-forwarded and sticky sessions also choose to purely reply on n/w based routing

# ELB
- **Health checks** : An ELB periodically checks all registered instances for it's health
	- This health check can be personalised by setting up a custom health checkup
	- Based on health, instances are either in *InService* or *OutOfService*
- Every loadbalancer *have their own DNS name*
	- It's IP Address is not visible
- **Error 504** : loadbalancer throws this error when there is some issue within the server
	- This could be because either the web server or the database layer is failing
- **X-Forwarded-For Header** : On using LBs al the request forwarded through it has the origin IP of LB instead of client.
	- In order for application to get the origin IP, X-Forwarded-For Header must be enabled.
	- A header will be included with the origin IP.
- **Sticky Sessions** : Sticky sessions allow to route traffic from a particular user to the same instance.
	- All requests during a session will be routed to the same instance allowing application to save user state during that session.
- **Cross Zone Load Balancing** : Loadbalancing by default is only possible within single availability Zone
	- With crosszone load balancer enabled, it is possible to send load over multiple zones within the same Region
- **Path Patterns** : In ALB, you can forward requests based on URL path.
	- This is useful when there are multiple backend services fulfiling different purposes
	- e.g: general requests to one target group (www.myurl.com) and request to render images to another target group (www.myurl.com/images)



