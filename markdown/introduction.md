#### Debugging Kubernetes with kubectl debug

#### And Ephemeral Containers
<!-- with

<img src="./images/k0s-logo-full-color-dark.png" alt="k0s-logo" width="300"/> -->


You can follow along here:
https://trawler.github.io/cncf-meetup-dec-2022<!-- .element: class="r-fit-text"-->


##### About Me

- Engineering Manager @ Flink
- Software Engineer with Devops experience
- Open Source contributor
- Loves everything to do with the sea

<img src="./images/karen_circle.png" style="width: 20%;">

https://github.com/trawler<!-- .element: style="width: 50%;"-->


#### Debugging Use-Cases
- We want to debug an application in its "native" environment <!-- .element id="fragment-list" class="fragment fade-in" -->
- Slim, fast containers, so no debugging tools baked-in <!-- .element id="fragment-list" class="fragment fade-in" -->
- Full-blown Linux containers tend to have more security vulnerabilities  <!-- .element id="fragment-list" class="fragment fade-in"-->

Note:
We can't or we don't want to debug a pod in our dev environment, but in production.
We also want our pods to deploy quickly and for the containers to be as slim as possible, so we try to strip as much as possible from the container image.
A lot of linux-based container image have security vulnerability, since they are usually not updated quickly enough.


```
$ kubectl exec -ti go-webapp-7c65f7886c-dcwhs -- /bin/bash
OCI runtime exec failed: exec failed: unable to start container process: 
exec: "/bin/bash": stat /bin/bash: no such file or directory: unknown
command terminated with exit code 126
```
```
$ kubectl exec -ti go-webapp-7c65f7886c-dcwhs -- /bin/sh
OCI runtime exec failed: exec failed: unable to start container process: 
exec: "/bin/sh": stat /bin/sh: no such file or directory: unknown
command terminated with exit code 126
```


#### What are Ephemeral Containers?
<img src="./images/containers.webp" alt="credit: vccircle" style="width: 60%;"/>


- Short-lasting containers - part of Kubernetes core API <!-- .element id="fragment-list" -->
- Lack guarantees for resources or execution  <!-- .element id="fragment-list" class="fragment fade-in"-->
- Will never be automatically restarted <!-- .element id="fragment-list" class="fragment fade-in" -->
- Created through a special "ephemeralcontainers" API handler <!-- .element id="fragment-list" class="fragment fade-in" -->

Note:
Ephemeral containers are useful for interactive troubleshooting when kubectl exec is insufficient because a container has crashed or a container image doesn't include debugging utilities


#### Kubectl Debug

Note:
This option is available in k8s versions 1.23 and become Generally Available with 1.25


