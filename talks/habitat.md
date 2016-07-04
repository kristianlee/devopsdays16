#The Evolution of Automation

The talk was given by Adam Jacob, Chef's CTO. 

He wrote chef, and started talking in general about his process in writing their new product 'Habitat'.

> "If your people are miserable, you're probably writing a miserable product"

Working in silos makes automation difficult (i.e. you'll have a bunch of different automation products for each different silo). 

Automation challenges are different according to the scale of the company. 

At companies who run their own application delivery layer (i.e. Google, who run their own server management code to run their datacentres etc) they tend to have a good automation story:
> "The platform is the product"

Smaller companies struggle more. 

##Habitat
He talked about the production cliff, i.e. the growing difficulty of deploying apps the closer one gets to production (often as a result of incompatible environments between development and production). 

>"Adopting new technology is like climbing Everest. You love it because you tortured yourself and survived"

Application automation is traditionally built from the infrastructure up, however, Habitat is built with the application in mind instead.

Snipped from [here](http://siliconangle.com/blog/2016/06/14/chef-cooks-up-a-new-open-source-application-automation-project/)

When apps are wrapped in Habitat, the runtime environment is no longer the focus which means the app is no longer constrained.

Habitat is based on the same concept of how container systems already work. Application packages created using Habitat are repeatable (which ensures the same results from each build), immutable (which ensures the package contents don’t change during runtime) and language-and-runtime-agnostic (they can be deployed via containers, bare metal servers et al). Using that foundation, Habitat merges the app with its automation details, so the two stay together throughout its lifecycle. As such, any changes made to the app are discoverable and can be altered by editing the app’s configuration.

As for the automation data, this is detailed in a shell script and optional configuration files. Habitat uses what’s called a “supervisor” app to run on the target platform as a host for the app in question, which performs the actual management. It’s possible to lump multiple supervisor apps together into service groups, and these supervisors can also be packaged inside Docker containers together with their apps.