---
layout: post
title: Database Sharding
subtitle: Part 1 - Introduction
cover-img: /assets/img/path.jpg
thumbnail-img: /assets/img/thumb.png
share-img: /assets/img/path.jpg
tags: [database, work]
---

## Pramble
Eventually, most applications grow. Handling growth is usually relatively easy: every modern cloud provider has tools to do just that. Whether its simply adding more application servers (horizontal scaling) or increasing the CPU or memory of existing servers (vertical scaling), there is probably a nice, well designed UI for dealing with the growth of your application. 

While application scaling is a given using modern tools and frameworks, what happens to the databases that hold your application data? Most applications use a relational database system in one way or another, and many applications use relational databases exclusively or almost exclusively for data storage, perhaps with caches and file storage handled separately. Unfortunately, you will only find a nice UI for scaling your database vertically, by adding compute or memory resources to your existing database host.

Horizontally scaling a relational database is not something a cloud service provider can do (yet). To add servers to your database (which will now be called a database cluster), you have to do some leg work and design and implement a strategy for sharing the workload between your database hosts.

This is an introduction to a horizontal scaling strategy for data systems called sharding.

## What is sharding
A sharded database architecture, at its core, is essentially an extension of horizontal partitioning, a feature that exists is most modern relational database software. Imagine, if you will, a regular old table that represents a blog post. Lets call that table Posts. Lets also add a partitioned table, called Comments, that store the comments for posts. The Comments are partitioned by a post_id, and new partitions are added by a DBA or by some automated process every month, where a new parition is added and configured to house comments for any post_id greater than all the post_id's from the previous month. The previous month's partition is updated with a new configuration parameter that says it can only house comments where the post_id is less than or equal to the last post of that month.

This is relatively common, even for this contrived example. I will not go over the benefits all of partitioning, but some of them include: table sizes are smaller, searches can be parallelized, and corruption is isolated to subsets of a table rather than to an entire table.

This is great! In fact, in conjunction with partitioning, vertical scaling can improve database performance and longevity far beyond what many people would expect, and with modern relational databases, your application may not even need to be updated to take advantage of partitioning. Eventually, however, a ceiling will be hit: there is only so much compute power, disk access, network bandwidth, and memory that can be assigned to a single server. Even long before you hit those limits, adding those vertical scaling options starts to get very expensive. Doubling the throughput of a server is usually not linear, and it is definitely not linear for a tranditional relational database. To double your performance, you will have to more than double your spend for a database host.

Sharding takes the concept of horizontal partitioning and spreads those partitions around to other database hosts rather than simply to other files on the same database host. Instead of comments existing as another table on your Post database, you can create a dozen comment databases, and assign comments to each of them the same way you would a partition: check the post its assigned to, and put the comment on the database host assigned for that post.

## Upsides

## Downsides
This is not as easy as I make it sound. Most relational databases to not have out-of-the-box sharding, and even the ones that do like Oracle or MySQL, the configuration is not trivial and your application still needs to be aware of where the data it wants to access is actually located.

## Next

