# Percona Training

## Bundled Classes

There are two pre-bundled classes for 16 credits each: "Operations" and "Developers". The PXC training is full-day, so it too can be considered a standalone class.

### Operations for MySQL

This is a 2-day class covering the following modules:
* Basics and Tools
* Backup and Recovery
* Replication
* Server Troubleshooting
* Monitoring
* Security
* Hardware and OS
* InnoDB Basics
* InnoDB Internals, Locking and Diagnostics

### Developers for MySQL

This is a 2-day class covering the following modules:
* Query Optimization
* Architecture Mistakes
* Application Architecture
* Instrumentation
* Schema Design
* SQL Modes
* Partitioning
* SQL Injection
* MyMovies Lab

## Training Modules (alphabetical)

### Application Architecture

    Duration: 1hr
    Target: Developer, Operations

This module covers example architecture topologies. Hardware solutions, caching, replication and sharding are also discussed.

### Architecture Mistakes

    Duration: 45m
    Target: Developer, Operations

This module covers a series of general architecture mistakes seen in the field over the past several years. The scenario is discussed along with the mistakes made. Better solutions to the issues are presented and discussed as well.

### Backup and Recovery

    Duration: 2hrs
    Target: Operations

In this module, discussion topics include: backup strategies, logical vs physical backups, incremental backups, delayed slaves and backup validation.

### Basics and Tools

    Duration: 1hr
    Target: Operations

The different storage engines, the different MySQL versions, the supplied tools and the Percona Toolkit are touched on in this module.

### Hardware and Operating Systems

    Duration: 0.5hr
    Target: Operations

This short module discusses various concerns (OS, RAID, Virtualization, Hardware, etc) when building a new MySQL instances.

### High Availability

    Duration: 0.75hr
    Target: Operations

Various high availbility solutions are presented and discussed with their pros and cons.

### InnoDB Basics

    Duration: 1hr
    Target: Developer, Operations

This module covers the basics of InnoDB's operation, including the transaction log, undo operations and checkpointing.

### InnoDB Internals, Locking and Diagnostics

    Duration: 4hrs
    Target: Operations

This module deep-dives into many specific InnoDB internals, including: tablespace usage, the clustered index, the adaptive hash, the change and doublewrite buffers, multi-version concurrency control, purging old data, locking structures, and diagnostic information.

### Instrumentation
    
    Duration: 0.5hr
    Target: Developer

Application instrumentation is the focus of this module. Determining where the problem may lie, from the application's perspective. What, where, why and how are discussed.

### MyMovies Lab

    Duration: 3hrs
    Target: Developer
    Depends: Query Optimization, Schema Design, Basics and Tools

This hands-on lab presents each student with a poorly performing sample web application and database. The goal is to optimize the application, with a partner, by making database changes using the tools, tricks and tips discussed in earlier modules.

### MySQL for Oracle DBAs

    Duration: 8hrs
    Target: Developer, Operations

This full-day class introduces MySQL from an Oracle DBAs perspective. Topics include: high level overviews, key differences in schema/index management, the mysql security model, major differences in stored routines, triggers and views, differences in the SQL dialects, basic administration, replication, how to handle backups and various workarounds for missing features.

### MySQL 5.7 Overview

    Duration: 8hrs
    Target: Developer, Operations

This module presents all of the new MySQL 5.7 features. Additionally, upgrading to 5.7, various improvements and defaults changes are discussed. Various hands-on labs let the student experience these changes and features.

### Partitioning

    Duration: 1.5hrs
    Target: Developer, Operations

In this module, the different partitioning types, how to manage partitions, their benefits, tools and tips, and restrictions are discussed.

### Percona XtraDB Cluster Tutorial

    Duration: 8hrs
    Target: Developer, Operations

This is a full-day, hands-on session. Students will learn XtraDB Cluster by migrating an asynchronous Master/Slave setup to PXC. We discuss replication and behavior, online schema changes, application high availability, how to monitor, state transfers, node failures, arbitration, bootstrapping and how to tune replication.

### Query Optimization

    Duration: 2hrs
    Target: Developer

This module covers the basics of query optimization within MySQL. Students learn how to use, read and understand the _EXPLAIN_ syntax to improve query performance. Single, prefix and multi-column indexes are discusses as are JOIN optimizations and covering indexes.

### Replication

    Duration: 2.5hrs
    Target: Operations

This hands-on module takes the student through the process of setting up a slave replica. How replication works, various topologies, administration, maintenance, various problems and solutions along with ensuring replication itegrity are all discussed.

### Schema Design

    Duration: 3.5hrs
    Target: Developer, Operations

This module coveres database design, SQL data types, table design, normalization, SQL constraints, and extensible schema design methodology.

### Server Troubleshooting

    Duration: 1.5hrs
    Target: Operations

The student is taken through various server components to understand interworkings of all levels. Various diagnostic tools, and MySQL-supplied information is also discussed.

### Stored Procedures

    Duration: 1.5hrs
    Target: Developer

This module discusses all of the ins and outs of stored procedures within MySQL. The why, the basics, performance and other issues are discussed.

### SQL Modes

    Duration: 0.5hr
    Target: Developer, Operations

What is 'SQL Mode', the various settings and their impact on the data stored within the database is discussed in this module.

### SQL Injection

    Duration: 1hr
    Target: Developer

This module discusses how SQL injection is still a very serious threat. How it happens and what can be done to protect against it are topics of discussion.
