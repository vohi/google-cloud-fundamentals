# Google Cloud Fundamentals

Files and notes from the training day in Oslo, June 15th.

## Introduction

### Billing and organisational structure

GSuite is used at the organisational level, and GSuite admin is also administrator for the GCP project structure.

Projects are used for tracking billing, but projects can be grouped into folders to create billing groups.

Policies are inherited through the organisational and project hierarchy. Least restrictive policy applies.

### Identity and Access management

GSuite is used for user credentials (ie no separate user directory). Possible to get a free GSuite without the applications in order to provide user credentials, if GSuite is not used in organisation. Also possible to integrate authentication with Active Directory or similar.

Entities that can get access are individual users, services accounts (authenticate using keys and can be used by resources), groups, or the entire domain.

Role base access control, with 

* primitive
    * owner
    * editor
    * viewer
    * billing admin
    
* predefined
    * grants privileges
    * for specific resources types

and

* custom roles.


### API Access

Two sets of client libraries: Cloud Client Libraries are hand-crafted, idiomatic libraries for Google Cloud APIs.

Google API Client Libraries are generated from discovery documents and include end-user products (such as Google Calendar).


### Provisioning and deployment solutions

Google's Deployment Manager solution is comparable to Terraform, with templates available to deploy pre-paid services (such as Jenkins) via the Cloud Launcher.

## Compute

### Networking

Default VPC automatically provisioned, with subnets for each region globally. Subnets have IP address range that can be modified. Machines within the same VPC can (in general) talk to each other.

Regional traffic is free.

VPC network provides many internetworking features (policies, routes, firewalls, VPN, cloud router with BGP support).

Interconnect options include Carrier Interconnect, Direct Peering - allows hybrid setups with trusted connectivity between on-prem and GCP. CDN Interconnect gives better pricing for egress of static content.

Cloud Load Balancing allows globally distributed traffic routing.

Cloud CDN replicates static content at the edge of the network.



### Compute instances

Instances are deployed in a region and AZ.

Standard machines have balanced compute/memory. High-mem machines have comparatively high memory etc. Customising possible, within certain limits (minimum of RAM per vCPU)

Larger disks can deliver higher IOps.

Some attributes (esp disk sizes) can be changed without reboot.

Instance metadata can include startup scripts.

Preemptible instances die after 24 hours at the latest. Generally, designing around machines dying leads to more resilience.


## Storage

### Cloud Storage

Stores objects as BLOBs in buckets, with metadata for each object. Metadata can be used for e.g http caching.

Bucket name must be globally unique; can be a domain name, when validated easily serves static web site.

Default storage class for a bucket, can be overridden for each object. Storage class can be managed via lifecycles (move old data to cheaper storage class). Objects are versioned.

Multi-regionals have highest availability, but most expensive for storage. Near/Coldline have cheaper storage cost, but higher retrieval cost.

Durability is the same for each storage class (11 9's), access time is sub-second for all storage classes.

Cloud storage is a good staging area - upload and process raw data before pushing it into other products, from which to serve them.

### Cloud Bigtable

Managed NoSQL, based on HBase.

Good for data where key is known in advance (such as time series, senor data, gmail, financial data). Fast to do key-range scans, but not very flexible querying.

Access via APIs (such an OpenTSDB), streaming, or batch processing.

### Cloud SQL and Spanner

MySQL or PSQL as a service - managed RDBMS, replicated (with read-replicas) and backed up. Writes need to go through master (only vertical scaling).

Cloud spanner provides horizontally scalling, ACID compliant RDBMS for read and write. Significantly more expensive.

### Cloud Datastore

Adds ACID transactions to NoSQL.

Schemaless access through a RESTful interface.
