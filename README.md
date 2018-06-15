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

## Containers and Orchestration with Kubernetes

Abstraction layer above a cluster of virtual machines - containers isolate workload, kubernetes assigns workload to capacity.

Cloud providers provide bindings to support Kubernetes (such as disk provisioning); running k8 on premise requires that such bindings are made available.

Setting up k8 is hard (setting up master, registering nodes etc); Kubernetes Engine provides managed kubernetes - managed nodes, managed master (which is transparent). Also includes logging, health monitoring, monitoring.

Container Builder and Container Registry are auxiliary and integrated services. Supports building and managing docker images based on triggers, such as git pushes.


## App Engine

Two flavours for building web services (ie HTTP/S): 

* App Engine Standard - easy, but restrictive
   * supports Java, Python, PHP, Go, Node.js
   * Services, f.ex memcache, lucene, scheduling, logging
   * No writes to local file system (perhaps a good thing)
   * Front-end requests have 60 seconds timeout
   * Limited availability of 3rd party software installations
   * free daily quota of 28 instance hours per day; instances not visible
   * SDKs for dev, test, deploy

* App Engine Flex - less restrictive
    * No sandbox constraints
    * Standard runtimes as for AppEngine Standard (but without sandbox constraints)
    * Custom runtime: any language that supports HTTP requests (package runtime as Dockerfile)
    * pay per instance, access to instance

Not mutually exclusive - can run multiple versions of same service, some with Standard, some with Flex, and use traffic splitting. Enables microservice architecture.

### Cloud Endpoints

Distributed API management. Access control, call validation (via JSON web token and API keys), metering.

Use swagger to declare endpoint APIs, generate client API libraries and server stubs.

For public endpoints, automatically translate from HTTP to gRPC.

Cloud Endpoints supports Kubernetes and Compute Engine through proxy.

### Apigee

Additional to Cloud Endpoints, includes analytics, monetization options for APIs, developer portal.

Target usecase is transformation and opening up legacy.

## Cloud Functions, Deployment Manager, Stackdriver

### Cloud Source Repository

Hosted git repository with collaboration functionality.

### Cloud Functions

Small code, linked to tiggers to process data. Currently only supports Node.js. Go support coming up.

Recommended Cloud Functions tutorial: google cloud ocr tutorial. Cloud 

### Deployment manager

Infrastructure as code, using a .yaml file.

Provides repeatable deployment, similar to terraform.

Hosted and fully declarative (ansible/puppet considered imperative?!)

### Monitoring with Stackdriver

Supports monitoring, logging, error reporting, debugging, tracing, profiling (new).

Aggregated logging, tracing into latency etc.

Debugger allows setting breakpoints directly into code of applications run in App Engine, Compute Engine, Kubernetes - code is not stopped, but takes snapshot of stack trace and variable values when breakpoint is hit.

Hosted outside Google Cloud console. Best to use a single stackdrive account for multiple projects, single-pane-of-glass for multiple projects.

