#Test driven Dockerized infrastructure

This talk explained what TDI was, and recommended a bunch of tools to facilitate it. 


TDI = test driven infrastructure

TDI is similar to test driven development, but applied instead to infrastructure. 

Why TDI:
- Better "Code"
- Design
- Consistency

The general workflow for TDI is:
1. Set up virtual env
2. Run provisioning code
3. Run test suite

##Serverspec
Amongst infra test specs, [serverspec](http://serverspec.org/) is most popular tools. 
Define resources and how they should behave for each server, you can test a particular host. 
Serverspec documentation has a bunch of resources. You'll need to know basic ruby. 

##Specing containerized infrastructure
[Containerspec](https://github.com/de-wiring/containerspec), specs docker images and container (build on serverspec and cucumber). It runs as a docker container itself. You can write human-readable specs because it uses cucumber. 

##Other Tools
[TestKitchen](kitchen.ci), write tests in ruby, there's a plugin for Docker
[Goss](Github.com/aelsabbahy/goss)
[TestInfra](https://github.com/philpep/testinfra) - the Python equivalent of serverspec

TDI is simple with these tools. 



