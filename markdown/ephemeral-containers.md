<img src="./images/ephemeral-containers-target-container-2000-opt.png" alt="credit: Ivan Velichko" style="width: 80%;"/>

Note:
An ephemeral container runs within the Pod's existing resource allocation and shares common container namespaces.
It is important to note that process namespace sharing has to be enabled at both the cluster level as well as in the debugged pod for ephemeral containers to work. It has been enabled by default at the cluster level since Kubernetes 1.17


#### DEMO