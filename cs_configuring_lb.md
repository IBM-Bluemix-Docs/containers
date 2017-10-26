# Load Balancer Configuration #

This section contains information on configuring the Load Balancer.

## Opening Ports ##

By default the load-balancer only exposes ports 80 and 443. In order to expose different
ports some action must be taken.

The **public-ports** section with the desired port you wish to open must be added to the
**ibm-cloud-provider-ingress-cm** config map. In this example we will use port 9443.

Perform: `$ kubectl edit cm ibm-cloud-provider-ingress-cm -n kube-system`
and the **data** section as seen below.

```
Config Map Example:
apiVersion: v1
data:
  public-ports: "80;443;9443"
kind: ConfigMap
metadata:
  name: ibm-cloud-provider-ingress-cm
  namespace: kube-system
```

*Note: 80, 443 are included to keep those ports open, any port not specified will be closed.*