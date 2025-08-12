**Flow of Events – User to Website**



1. User types www.foobar.com in their browser.



2\. Browser asks the DNS system: “What’s the IP for www.foobar.com?”



3\. DNS finds the A record for www pointing to 8.8.8.8.



4\. Browser sends an HTTP request to 8.8.8.8 (the server’s IP).



5\. The web server (Nginx) on the server receives the request.



6\. Nginx passes the request to the application server (e.g., Gunicorn for Python, PHP-FPM for PHP, or Passenger for Ruby).



7\. The application server runs application files (code base), which may query the MySQL database.



8\. The application server returns the generated HTML to Nginx.



9\. Nginx sends the response back to the user’s browser via HTTP.





**Components \& Roles**

1\. Server: a physical or virtual machine with an IP address (here: 8.8.8.8) that runs all components.

It stores and serves files, runs code, and processes requests.



2\. Domain Name: human-readable address (foobar.com) that maps to an IP (8.8.8.8).Makes it easy for people to access a website without remembering numbers.



3\. DNS Record Type: www in www.foobar.com is typically an A record mapping the subdomain www to the server’s IP.



4\. Web Server (Nginx): handles incoming HTTP requests from users. Serves static files directly (e.g., images, CSS, JS). Passes dynamic requests to the application server.



5\. Application Server: runs the actual web application logic (the code). Processes business logic, interacts with the database, and formats responses.



6\. Application Files (Code Base): the source code for the website or web app (e.g., Python, PHP, Ruby, etc.).



7\. Database (MySQL): stores and retrieves persistent data for the application (users, content, transactions).



8\. Communication Protocol: the server and the user’s computer communicate using HTTP/HTTPS over TCP/IP.





**Issues with This Infrastructure**

1. Single Point of Failure (SPOF): if the server crashes, everything goes offline since the web server, app, and database are all on one machine.



2\. Downtime During Maintenance: deploying new code or restarting the server means the whole site is temporarily unavailable.



3\. Scalability Limits: all components share CPU, RAM, and storage. A surge in traffic can overwhelm the server. Cannot horizontally scale without major restructuring.

