  View the IBM-provided domain and TLS certificate. Replace _&lt;cluster_name_or_ID&gt;_ with the name of the cluster where the app is deployed.

      ```
      {[bxcs]} cluster-get <cluster_name_or_ID>
      ```
      {: pre}

      Example output:

      ```
      Name:                   mycluster
      ID:                     18a61a63c6a94b658596ca93d087aad9
      State:                  normal
      Created:                2018-01-12T18:33:35+0000
      Location:               dal10
      Master URL:             https://{[public_IP]}:26268
      Ingress Subdomain:      mycluster-12345.us-south.containers.mybluemix.net
      Ingress Secret:         <ibm_tls_secret>
      Workers:                3
      Version:                {[kubeversions]}
      Owner Email:            owner@email.com
      Monitoring Dashboard:   <dashboard_URL>
      ```
      {: screen}

      You can see the IBM-provided domain in the **Ingress subdomain** and the IBM-provided certificate in the **Ingress secret** fields.
