# DMN Workshop

This is a group of three guided exercises about Decision Model and Notation - DMN. In this workshop you will find three levels of exercise:

- Getting started - Policy Price use case
- Intermediate - Vacation Days use case
- Advanced - Call Center use case

Prerequisite

* RHPAM or RHDM 7.5+

The output of the guides can be found at: https://github.com/kmacedovarela/dmn-workshop-labs 

Last update: PAM 7.10




## Instructions

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
