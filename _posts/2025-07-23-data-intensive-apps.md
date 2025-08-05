---
layout: post
title: Designing Data-Intensive Applications
date: 2025-07-23 00:00:00
description: System design & distributed systems
tags: system-design; distributed-systems
featured: false
related_posts: false
---

### <b>Storage and Retrieval</b>
<br>

##### <b>Append-only data storage</b>

A simple, high-performance key-value store design inspired by log-structured storage (used in systems like Apache Kafka, Bitcask, and LSM trees).

Use an indexing strategy and keep an in-memory hash map where every key is mapped to a byte offset in the data file which shows the location the value is found for that key. Update the hash map whenever you append a new key-value pair.

How to avoid eventually running out of disk space? Split the log into segment files of a certain size and close the segment file when it reaches that size and start a new one. Compaction = going through segments and removing duplicate keys in the log and keep only the most recent update. 

Each segment has its own in-memory hash map mapping key to file offsets. To find the value for a key, first check in the most recent segment's hash map, if not found then check second most recent segment's hash map, etc.

<br>
##### <b>Data Warehousing</b>

Online Transaction Processing (OLTP) systems are expected to be highly available and to be low latency, since they are important to the operations of the business. They are written to by user input.

Online Analytical Processing (OLAP) systems are designed for complex data analysis. They are written to by extracting data from OLTP via Extract-Transform-Load (ETL). OLAPs are read-only.

OLAP systems usually use star or snowflake schema. These involve using fact tables which connect to multiple dimension tables. Fact tables are very large and store quantitative data related to business needs. Dimension tables store descriptive context about the data in the fact table.

