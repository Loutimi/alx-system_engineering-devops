Image url: https://imgur.com/undefined





**Flow of Events – Secure Infrastructure**

1. User types https://www.foobar.com in their browser.
   
2. Browser queries DNS — A record for www points to the load balancer public IP.
   
3. Firewall 1 (in front of load balancer) filters incoming requests to only allow HTTPS (port 443).
   
4. The load balancer (HAProxy) terminates SSL using the installed SSL certificate for www.foobar.com.
   
5. Request is sent to one of the application servers based on the load-balancing algorithm.
   
6. Firewall 2 (in front of application servers) only allows traffic from the load balancer.
   
7. Application servers query the MySQL Primary-Replica cluster for data.
   
8. Firewall 3 (in front of database) only allows traffic from the application servers.
   
9. Monitoring clients on all servers send logs and metrics to a monitoring service like Sumo Logic.
   
10. Response goes back to the load balancer → firewall → user via HTTPS.





**Why Add Each Element**

1. Firewalls (3):

   F1: Protects the load balancer from unwanted traffic.

   F2: Protects application servers from direct external access.

   F3: Protects the database from unauthorized connections.
   
2. SSL Certificate: Encrypts communication between the user and your site so data is private and tamper-proof.
   
3. Monitoring Clients: Track server health, performance, errors, and security incidents in real time.



**Firewalls – Purpose**

* Block or allow traffic based on defined rules (ports, IPs, protocols).



* Help prevent unauthorized access or attacks.



**Why HTTPS**

* Encrypts traffic, preventing eavesdropping and tampering.



* Increases user trust and is required for many web features (e.g., HTTP/2, browser security policies).



**Monitoring – Purpose**

* Detects downtime, performance degradation, and security issues.



* Helps identify problems before they affect users.



**How Monitoring Works**

* A monitoring agent/client on each server collects logs, metrics (CPU, memory, QPS), and sends them to a central service like Sumo Logic.



* Data is sent over a secure connection for analysis and alerting.



To monitor web server QPS (Queries Per Second):



* Configure Nginx to log request counts.



* Monitoring agent scrapes Nginx access logs or metrics endpoints.



* Dashboard shows QPS trends and triggers alerts if it spikes unexpectedly.



**Issues with This Infrastructure**

1. SSL Termination at Load Balancer

   Traffic between the load balancer and app servers is unencrypted.

   If the internal network is compromised, data can be intercepted.
   
2. Only One MySQL Write Node

   The Primary DB is a single point of failure for writes.

   If it fails, the site cannot process any writes until failover.
   
3. All Servers with Same Components

   Running database, web server, and application server together on each machine can cause resource contention.

   A spike in one service (e.g., DB CPU usage) can degrade all others.
   
