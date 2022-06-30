#### Running Kubernetes in 

#### IoT/Edge environments
with

<img src="./images/k0s-logo-full-color-dark.png" alt="k0s-logo" width="300"/>


You can follow along here:
https://trawler.github.io/kcd-berlin-2022<!-- .element: class="r-fit-text"-->


#### About Me
- Devops → Dev 
- Open Source enthusiast
- Automation Freak
- Loves everything to do with the sea

<img src="./images/trawler.png" style="width: 20%;">

https://github.com/trawler<!-- .element: style="width: 10%;"-->


#### The Challenges of IoT Devices
<img src="./images/iot.gif" alt="credit: vccircle" style="width: 60%;"/>


- Security is a core concern  <!-- .element id="fragment-list" -->
- Wide range of devices  <!-- .element id="fragment-list" class="fragment fade-in"-->
- Computation and resource constraints <!-- .element id="fragment-list" class="fragment fade-in" -->

Note: 
- Security: It is necessary to keep the IoT edge devices away from unauthorized access. Discovery, authentication, and trust establishment in IoT edge and anonymity and traceability of devices are challenging in high-scale environments. An additional security layer is necessary to ensure that different IoT applications execute isolated from each other in the device.
- Wide Range: IoT devices can comprise of multiple architectures and operating systems. Sometime there will be multiple types of devices in the same cluster.


#### Control-plane Isolation


<table style="width: 100%">
    <colgroup>
       <col span="1" style="width: 40%;">
       <col span="1" style="width: 60%;">
    </colgroup>
<tbody>
  <tr>
    <td>
        <ul style=text-align:left;font-size:25px;list-style-type:square;line-height:2.0;>
            <li>Control-plane does not run any workloads</li>
            <li>Cluster management and workload fully separated</li>
            <li>Cluster networking not “stretched"</li>
        </ul>
    </td>
    <td style=vertical-align:middle;border:none;><img src="./images/isolated_controlplane.png" class="r-stretch"/></td> 
 </tr>
</table>


#### Why not the usual “master” nodes?

- Who can deploy apps into controller nodes? <!-- .element id="fragment-list" class="fragment fade-in"-->
- CNI needs to work → assumes flat-ish network <!-- .element id="fragment-list" class="fragment fade-in"-->
- Direct API → Kubelet connectivity <!-- .element id="fragment-list" class="fragment fade-in"-->
- Limited deployment architectures <!-- .element id="fragment-list" class="fragment fade-in"-->


#### Konnectivity
(kubernetes-sigs/apiserver-network-proxy)<!-- .element style=font-size:20px;-->
<table style="width: 100%">
    <colgroup>
       <col span="1" style="width: 50%;">
       <col span="1" style="width: 50%;">
    </colgroup>
<tbody>
  <tr>
    <td>
        <ul style=text-align:left;font-size:25px;list-style-type:square>
          <li>We want to be able to run workers in isolated networks from the control-plane</li>
          <li>Control-plane and Worker network space can even have overlapping IP addresses</li>
          <li>K8s API Server has a secure channel to the worker network (using gRPC)</li>
          <li>Advanced features: Load balancing, Monitoring, Auditing</li>
        </ul>
    </td>
    <td style=vertical-align:middle;border:none;><img src="./images/konnectivity.png" class="r-stretch"/></td> 
 </tr>
</table>


#### Konnectivity: Additional Sources

https://github.com/kubernetes-sigs/apiserver-network-proxy<!-- .element: class="r-fit-text"-->
https://github.com/kubernetes/enhancements/tree/master/keps/sig-api-machinery/1281-network-proxy#proposal<!-- .element: class="r-fit-text"-->
[KAS Proxy Service Presentation: Kubecon China 2019](https://static.sched.com/hosted_files/kccncosschn19eng/e8/Kubecon%202019%20Shanghai%20Network%20Proxy.pdf)<!-- .element: class="r-fit-text"-->


#### Packaging


#### One single binary
<table style="width: 100%">
    <colgroup>
       <col span="1" style="width: 50%;">
       <col span="1" style="width: 50%;">
    </colgroup>
<tbody>
  <tr>
    <td>
        <ul style=text-align:left;font-size:25px;line-height:2.0;list-style-type:square>
            <li>Can be installed on any Linux without additional SW packages*</li>
            <li>Security vulnerabilities can be fixed directly in k0s</li>
            <li>OS and k0s upgrades are independent from each other</li>
            <li>Easy installation process</li>
        </ul>
    </td>
    <td style=vertical-align:middle;border:none;><img src="./images/k0s_binary_diagram.png" class="r-stretch"/></td> 
 </tr>
</table>


#### Batteries Included, 
#### but swappable

<table style="width: 100%">
    <colgroup>
       <col span="1" style="width: 50%;">
       <col span="1" style="width: 50%;">
    </colgroup>
<tbody>
  <tr>
    <td style=border:none;font-size:35px;vertical-align:bottom>By default, k0s includes:</td>
    <td rowspan=2 style=vertical-align:middle;border:none;><img src="./images/k0s_components.png" class="r-stretch"/></td>
  </tr>
  <tr>
    <td style=border:none;>
        <ul style=text-align:left;font-size:25px;line-height:2.0;list-style-type:square>
            <li>Containerd as runtime</li>
            <li>Kube-Router and Calico for networking</li>
            <li>Etcd and Kine/SQLite for datastore</li>
        </ul>
    </td>
  </tr>
</tbody>
</table>


#### Airgapped Setup

- Made simple due to “packaging”
- Setup needs two files:
  - k0s binary
  - Image bundle (tarball) for “system images”
- k0s imports image bundles on startup
- Easy to ship own images


#### Minimum System Requirements

<style type="text/css">
.tg  {border-collapse:collapse;border-color:#ccc;border-spacing:0;}
.tg td{background-color:#fff;border-color:#ccc;border-style:solid;border-width:0px;color:#333;
  font-size:20px;overflow:hidden;padding:10px 5px;word-break:normal;}
.tg th{background-color:#f0f0f0;border-color:#ccc;border-style:solid;border-width:0px;color:#333;
  font-size:20px;font-weight:normal;overflow:hidden;padding:10px 5px;word-break:normal;}
.tg .tg-c3ow{border-color:inherit;text-align:center;vertical-align:top}
</style>
<table class="tg">
<thead>
  <tr>
    <th class="tg-c3ow">Role</th>
    <th class="tg-c3ow">Virtual CPU (vCPU)</th>
    <th class="tg-c3ow">Memory (RAM)</th>
    <th class="tg-c3ow">Storage</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td class="tg-c3ow">Controller node</td>
    <td class="tg-c3ow">1 vCPU (2 recommended)</td>
    <td class="tg-c3ow">1 GB (2 recommended)</td>
    <td class="tg-c3ow">~0.5 GB</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Worker node</td>
    <td class="tg-c3ow">1 vCPU (2 recommended)</td>
    <td class="tg-c3ow">0.5 GB (1 recommended)</td>
    <td class="tg-c3ow">~1.3 GB</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Controller + Worker</td>
    <td class="tg-c3ow">~1.3 GB</td>
    <td class="tg-c3ow">1 GB (2 recommended)</td>
    <td class="tg-c3ow">~1.7 GB</td>
  </tr>
</tbody>
</table>

Architecture: x86-64, ARM64, ARMv7


#### Demo

