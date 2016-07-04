#How the hell do I run my microservices in production, and will it scale?

From a cloud vendor, talked a lot about Dockerised environment with microservices. 

##Quality Containers
> "You put shit in a docker container, it's just a containerised shit"

> The right image should be the same in all environments"

He suggested the 5 **S**s of docker quality:

- **Slim**: Start with the smallest minimal, remove compile time dependencies, remove packages you don't need. Habitus.io = easily remove runtime components. 
- **Secure**
Remove all the secrets, patch to the latest sec updates, run the image with the right uid. Test the image: github.com/docker/docker-bench-security
- **Speedy**
Optimise code, mem + cpu usage, 1 process, load testing
- **Stable**
Lock the image version, lock the runtime version, tag your image, proper logging
- **Set**
Use volumes wisely, use external services for persistency, don't abuse host system. Remove things that are hard to maintain in production. 


He said 70% of his clients are still just containerising monolithic apps. 

> "Docker won't solve the problem of deploying microservices in production"
> [the next slide was]
![alt text](nowwhat.gif "Now what")

##Q&A

- For stateful services, do it on normal Linux, not docker. 
- Use 'telegraph' to collect stats from containers
