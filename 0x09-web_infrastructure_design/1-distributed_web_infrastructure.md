Image url: https://imgur.com/7IaObef



**Flow of Events – User to Website**

1. User types www.foobar.com in their browser.
   
2. Browser queries DNS, A record for www points to the load balancer’s IP.
   
3. The load balancer (HAProxy) receives the request and distributes it between two application servers running Nginx and the app code.
   
4. Both app servers connect to a MySQL Primary-Replica database cluster for data access.

   -Writes (INSERT, UPDATE, DELETE) go to the Primary.

   -Reads (SELECT) can be served by the Replica to spread the load.
   
5. Response travels back through the load balancer to the user.





**Why Add Each Element**

1. Load Balancer (HAProxy): Distributes incoming requests to prevent overloading a single app server and provide redundancy.
   
2. Second Application Server: Increases capacity and provides failover if one app server fails.
   
3. Database Primary-Replica Cluster: Improves read performance and provides redundancy. If the Primary fails, the Replica can take over.





**Load Balancer Distribution Algorithm**

Round Robin: sends each incoming request to the next server in line in rotation. Balances traffic evenly without complex logic.

Example: Request 1 → Server A, Request 2 → Server B, Request 3 → Server A, etc.



**Active-Active vs Active-Passive**

1. Active-Active: Both app servers handle requests simultaneously.
   
2. Active-Passive: One server handles traffic while the other waits idle, only taking over if the active one fails.



Primary-Replica Database Cluster

Primary Node: Handles all writes and also reads if necessary. Keeps the authoritative dataset.



Replica Node: Receives a continuous copy of the Primary’s data via replication. Handles read-only queries to reduce load on the Primary.



From the application’s perspective:



All write queries → Primary.



Many read queries → Replica (for load distribution).



**Issues with This Infrastructure**

1. Single Points of Failure (SPOF): If the load balancer fails, the whole site is unreachable. If the Primary DB fails and Replica promotion isn’t automated, writes break.
   
2. Security Issues: no firewall, open to unnecessary ports and attacks. No HTTPS — data sent in plain text, vulnerable to eavesdropping.
   
3. No Monitoring: no health checks, alerts, or metrics — failures might go unnoticed.



