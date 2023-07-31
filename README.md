# 5gasp-helm

Helm charts for 5GASP Netapp-7

**Purpose:** These helm charts will deploy and instantiate MEC Handover prediction application


Note
====

After the first (probably failed) implementation as a KNF (from the OSM machine):

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


It has two APIs:

`GET <IP_address>:30101/` This API is to check the liveliness of the application-

*Expected Output*: `<200 OK> EMHO NetApp`

  

`POST <IP_address>:30101/mondata` This API consumes the radio monitoring data and return the mobile handover prediction probabilities
*Expected Output*: `Handover Probabilities and Time To Handover`

E.g.



    $ curl --location --request POST '10.68.107.102:30101/mondata' \
    
    --header 'Content-Type: application/json' \
    
    --data-raw '{
    
    "Time": {
    
    "0": 1735868776,
    
    "1": 1735868777
    
    },
    
    "RSRP-241": {
    
    "0": -83,
    
    "1": -81
    
    },
    
    "RSRP-341": {
    
    "0": -130,
    
    "1": -130
    
    },
    
    "RSRP-119": {
    
    "0": -120,
    
    "1": -120
    
    },
    
    "lat": {
    
    "0": 51.449481,
    
    "1": 51.4495
    
    },
    
    "lng": {
    
    "0": -2.601033,
    
    "1": -2.601006
    
    },
    
    "S-PCI": {
    
    "0": 241,
    
    "1": 241
    
    }
    
    }'
Expected Output:

    {'PCI-241': 0.9753108, 'PCI-341': 0.01637554, 'PCI-119': 0.008313667, 'TTH': 25.5}
