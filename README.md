This workshop is aimed at providing hands on experience creating Decision and Process Assets. This lab will implement a Loan Approval workflow.

Goals

* Define and create the process domain model using the platform’s Data Modeller.
* Create an Age Pre-Qualification Decision using Guided Rules
* Create an Interest Calculation decision using XLS Decision table.
* Create a Loan Pre-Qualification Decision using Guided Decision tables.
* Create a Loan Approval workflow.
* Create Task forms for human tasks.
* Deploying and Management of the Loan Approval Process

Prerequisite

* Successful completion of the Environment Setup Lab or
* An existing, accessible, DM/PAM 7.3+ environment.

You'll find the instructions at this [detailed instructions](Loan_Provision.adoc).



## Instructions

You can find the labs in a single doc within the ".adoc" file, or you can check them at: https://red.ht/workshop-pam-fuse

### Running locally

Have docker or podman running locally:

```````
docker run -it --rm -p 8080:8080 -v $(pwd):/app-data -e CONTENT_URL_PREFIX="file:///app-data" -e WORKSHOPS_URLS="file:///app-data/_dmn_workshop.yml" -e LOG_TO_STDOUT=true quay.io/osevg/workshopper
```````

### Running on ocp

Login on ocp and:

````
oc create -f support/ocp-provisioning.yml
````
