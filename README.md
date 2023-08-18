# 5gasp-helm

Helm chart for 5GASP Netapp-7's MiniAPI

**Purpose:** This helm chart will deploy and instantiate the MiniAPI pod connected to the MEC Handover prediction application, for testing purposes.


Note
====

After the first (possibly failed) implementation as a KNF (from the OSM machine):

1. Login to your private docker registry, on your K8s cluster master node, at [harbor.patras5g.eu/5gasp_private/]

2. On the same machine, make sure you create a secret:

    ```bash
    sudo kubectl create secret docker-registry 5gaspsecret --docker-server=harbor.patras5g.eu/5gasp_private/ --docker-username=<TESTBED_HARBOR_USERNAME> --docker-password=<TESTBED_HARBOR_PASSWORD> --docker-email=<TESTBED_PROFILE_EMAIL_ADDRESS> -n <osm-deployed-namespace-from-failed-pod>
    ```
3. Retry the KNF deployment from your OSM machine (via the OSM -> Harbor-public-helm-repository call -> helm instantiation -> Harbor-private-docker-registry call -> docker image instantiation descriptor-based instantiation chain)


## How to use
To install the helm chart on your K8s cluster:

    $ helm install -n 5gasp emho 5gasp-helm

The App will serve on 

    <kubernetes_node_IP_address> (and <instantiation node address>):30101


The APIs are accessible via port 31001, and they are as follows:

[Provide some descriptions on functionality]
  
