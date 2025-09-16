Problem Statement : 

When running applications that rely heavily on MySQL, it’s important to know how the database is performing. Issues like slow queries, too many connections, or replication lag can easily go unnoticed until they impact users. Manual monitoring (logging in to MySQL and checking status) is inefficient and error-prone. What we really need is automated monitoring with dashboards and alerts, so we can quickly detect and troubleshoot problems.

Goal of the Project :
1. Automatically collect key metrics from MySQL.
2. Centralize those metrics in a time-series database (Prometheus).
3. Visualize trends in Grafana dashboards (queries, uptime, connections, slow queries, replication health, etc.).
4. Provide a setup that’s reproducible and easy to run anywhere using Docker.

How It Works :
This project ties together 3 main components:
mysqld_exporter – connects to MySQL and exposes metrics over HTTP.
Prometheus – scrapes metrics from the exporter at intervals (every 15s) and stores them.
Grafana – reads from Prometheus and visualizes the metrics in dashboards.
All of this runs in Docker containers, orchestrated with docker-compose.

Files in the repo :
1. docker-compose.yml → Defines and runs MySQL, Prometheus, Grafana, and mysqld_exporter containers.
2. prometheus/prometheus.yml → Configures Prometheus to scrape MySQL metrics from mysqld_exporter.
3. grafana-provisioning/datasources/prometheus.yml → Sets up Prometheus as the default Grafana data source.
4. grafana-provisioning/dashboards/dashboards.yml → Tells Grafana where to find and auto-load dashboards.
5. grafana-provisioning/dashboards/mysql-dashboard.json → configured Grafana dashboard for MySQL metrics ( Here code is for active connections)
6. .my.cnf (local only) → Stores MySQL credentials for mysqld_exporter (not pushed to Git).
7. .gitignore → Ensures sensitive and unnecessary files (like .my.cnf) are not committed.
8. README.md → Documentation describing the project setup, usage, and output.


Output :
Once everything is running, here’s what you’ll get:
1. Prometheus continuously scrapes metrics from MySQL.
2. The exporter confirms MySQL is up and serving metrics at http://localhost:9104/metrics
3. Grafana automatically provisions a MySQL Overview dashboard, where you can see in one place:
     - Database uptime (mysql_up)
     - Total queries executed (mysql_global_status_queries)
     - Active connections (mysql_global_status_threads_connected)
     - Number of client requests/questions (mysql_global_status_questions)
     -  Slow query count (mysql_global_status_slow_queries)

So at a glance, you’ll know if MySQL is up, how many queries are running, and how many connections are active, without logging into the database manually.
