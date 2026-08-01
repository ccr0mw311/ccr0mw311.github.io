---
title: "Part 1: Building a SQL Server Observability Stack with Prometheus and Grafana"
date: 2026-08-01
categories: [Docker, Windows Server, Linux Server, Database, Prometheus, Grafana, Observability]
tags: [Docker, Windows Server, Linux Server, Database, Prometheus, Grafana, Observability]
---

As part of my observability learning journey, I built a monitoring stack for Microsoft SQL Server 2019 using Prometheus, Grafana, node_exporter, and sql_exporter. The objective was to monitor SQL Server performance metrics from a centralized dashboard while keeping the monitoring services isolated on a dedicated Ubuntu server.

## Environment

- Ubuntu Server (VM) – Docker host
- Windows Server 2019 (VM) – Microsoft SQL Server 2019 Developer Edition
- Database: BikeStores (Dummy Database)

## Architecture

![Monitoring Pipeline Architecture](/assets/img/posts/monitoring_pipeline_architecture.png)

## Monitoring Objectives

The observability stack was designed to collect the following SQL Server performance metrics:

* Database Time and Database CPU
* Average Active Sessions
* Wait Events
* Redo Log Wait Time
* Buffer Cache Hit Ratio
* Shared Pool Usage
* Library Cache Hit Ratio
* Top SQL Queries
* Blocking Sessions and Locks

---

## Install Docker

I installed Docker Engine and Docker Compose on the Ubuntu server and verified that the installation completed successfully.

![Docker Version](/assets/img/posts/dockercheck.png)

---

## Build the Base Monitoring Stack

Before integrating SQL Server monitoring, I first deployed a basic monitoring stack consisting of Prometheus and Grafana. I then verified that both web interfaces were accessible and confirmed that Grafana was successfully connected to Prometheus before proceeding with the SQL Server integration.

![Base Monitoring Portal](/assets/img/posts/basemonitoring1.png)

![Base Monitoring Portal](/assets/img/posts/basemonitoring2.png)

---

## Prepare the SQL Server Environment

To serve as the data source for the observability stack, I prepared a dedicated Windows Server 2019 virtual machine with Microsoft SQL Server 2019 Developer Edition.

The setup included the following tasks:

* Installed Windows Server 2019.

![Windows Server 2019](/assets/img/posts/wsvr1.png)

* Installed Microsoft SQL Server 2019 Developer Edition.

![Microsoft SQL Server 2019 Developer Edition](/assets/img/posts/sqlsvr.png)

* Installed SQL Server Management Studio (SSMS).

![SQL Server Management Studio](/assets/img/posts/ssms.png)

* Created the BikeStores dummy database.

![Dummy Database](/assets/img/posts/createdb.png)

* Created the required database objects, including schemas and tables.

![Database Objects, including Schemas and Tables](/assets/img/posts/createobj.png)

* Populated the database with dummy data.

![Dummy Data](/assets/img/posts/insertdata.png)

* Validated the database by executing SQL queries to verify that the database objects and data were created successfully.

![Validate Data](/assets/img/posts/validatedata.png)

After completing these tasks, the SQL Server environment was fully configured and ready for monitoring.

---

## Add SQL Exporter and Finalize the Monitoring Stack

With the monitoring platform and SQL Server environment validated, I integrated sql_exporter into the Docker Compose stack.

The following tasks were completed:

* Created the sql_exporter project directory.
* Prepared the SQL Server monitoring configuration.
* Updated .yml files
* Configured Prometheus to scrape metrics from sql_exporter.
* Connected sql_exporter to the SQL Server instance using the sql_monitor account.

---

## Apply and Verify the Configuration

After completing the configuration, I validated and started the monitoring stack.

All four containers were successfully deployed and running.

![Containers](/assets/img/posts/containers.png)

Next, I verified that sql_exporter was exposing SQL Server metrics.

![Server Metrics](/assets/img/posts/servermetrics.png)

The command returned SQL Server metrics, confirming successful communication between sql_exporter and the SQL Server instance.

Finally, I opened the Prometheus Targets page and verified that the following targets were in the UP state:

* Prometheus
* node_exporter
* sql_exporter

![Targets](/assets/img/posts/targets.png)

---

## Step 6 – Import the Grafana Dashboard

With metrics successfully flowing into Prometheus, I imported a Grafana dashboard to visualize SQL Server performance in real time.

![Dashboard 1](/assets/img/posts/dash1.png)

![Dashboard 2](/assets/img/posts/dash2.png)

![Dashboard 3](/assets/img/posts/dash3.png)

The dashboard provides visibility into key database metrics such as database CPU utilization, wait events, active sessions, buffer cache performance, blocking sessions, and the top SQL queries running on the server.

---

## Overall

This project provided valuable hands-on experience in building an end-to-end observability solution for Microsoft SQL Server using Prometheus, Grafana, node_exporter, and sql_exporter. By separating the monitoring platform from the database server, the environment closely resembles a real-world deployment while providing centralized visibility into both server and database performance.

This is Part 1 of my observability series, focusing on SQL Server monitoring. In Part 2, I'll extend the solution by monitoring the Windows Server virtual machine and the VMware virtualization layer, providing deeper visibility into operating system resources, virtualization performance, and infrastructure health alongside the SQL Server metrics collected in this project.