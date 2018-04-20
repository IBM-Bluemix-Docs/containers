  Create a Kubernetes service for the app that you want to expose. Your app must be exposed by a Kubernetes service to be included by the cluster ALB in the Ingress load balancing.
      1.  Open your preferred editor and create a service configuration file that is named, for example, `myalbservice.yaml`.
      2.  Define a service for the app that the ALB will expose to the private network.

          ```
          apiVersion: v1
          kind: Service
          metadata:
            name: myalbservice
          spec:
            selector:
              <selector_key>: <selector_value>
            ports:
             - protocol: TCP
               port: 8080
          ```
          {: codeblock}

          <table>
          <caption>Understanding the ALB service file components</caption>
          <thead>
          <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
          </thead>
          <tbody>
          <tr>
          <td><code>selector</code></td>
          <td>Enter the label key (<em>&lt;selector_key&gt;</em>) and value (<em>&lt;selector_value&gt;</em>) pair that you want to use to target the pods where your app runs. To target your pods and include them in the service load balancing, make sure that the <em>&lt;selector_key&gt;</em> and <em>&lt;selector_value&gt;</em> are the same as the key/value pair that you used in the <code>spec.template.metadata.labels</code> section of your deployment yaml.</td>
           </tr>
           <tr>
           <td><code>port</code></td>
           <td>The port that the service listens on.</td>
           </tr>
           </tbody></table>
      3.  Save your changes.
      4.  Create the service in your cluster.

          ```
          kubectl apply -f myalbservice.yaml
          ```
          {: pre}
      5.  Repeat these steps for every app that you want to expose to the private network.
