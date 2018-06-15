# Google Cloud Fundamentals

Files and notes from the training day in Oslo, June 15th.

## Billing and organisational structure

GSuite is used at the organisational level, and GSuite admin is also administrator for the GCP project structure.

Projects are used for tracking billing, but projects can be grouped into folders to create billing groups.

Policies are inherited through the organisational and project hierarchy. Least restrictive policy applies.

## Identity and Access management

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


## API Access

Two sets of client libraries: Cloud Client Libraries are hand-crafted, idiomatic libraries for Google Cloud APIs.

Google API Client Libraries are generated from discovery documents and include end-user products (such as Google Calendar).


## Provisioning and deployment solutions

Google's Deployment Manager solution is comparable to Terraform, with templates available to deploy pre-paid services (such as Jenkins) via the Cloud Launcher.

