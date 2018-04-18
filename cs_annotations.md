---

copyright:
  years: 2014, 2018
lastupdated: "2018-04-18"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Ingress annotations
{: #ingress_annotation}

To add capabilities to your Ingress application load balancer (ALB), you can specify annotations as metadata in an Ingress resource.
{: shortdesc}

For general information about Ingress services and how to get started using them, see [Managing network traffic by using Ingress](cs_ingress.html#planning).

<table>
<col width="20%">
<col width="20%">
<col width="60%">
 <thead>
 <th>General annotations</th>
 <th>Name</th>
 <th>Description</th>
 </thead>
 <tbody>
 <tr>
 <td><a href="#proxy-external-service">External services</a></td>
 <td><code>proxy-external-service</code></td>
 <td>Add path definitions to external services, such as a service hosted in {{site.data.keyword.Bluemix_notm}}.</td>
 </tr>
 <tr>
 <td><a href="#location-modifier">Location modifier</a></td>
 <td><code>location-modifier</code></td>
 <td>Modify the way the ALB matches the request URI against the app path.</td>
 </tr>
 <tr>
 <td><a href="#alb-id">Private ALB routing</a></td>
 <td><code>ALB-ID</code></td>
 <td>Route incoming requests to your apps with a private ALB.</td>
 </tr>
 <tr>
 <td><a href="#rewrite-path">Rewrite paths</a></td>
 <td><code>rewrite-path</code></td>
 <td>Route incoming network traffic to a different path that your backend app listens on.</td>
 </tr>
 <tr>
 <td><a href="#tcp-ports">TCP ports</a></td>
 <td><code>tcp-ports</code></td>
 <td>Access an app via a non-standard TCP port.</td>
 </tr>
 </tbody></table>

<br>

<table>
<col width="20%">
<col width="20%">
<col width="60%">
 <thead>
 <th>Connection annotations</th>
 <th>Name</th>
 <th>Description</th>
  </thead>
  <tbody>
  <tr>
  <td><a href="#proxy-connect-timeout">Custom connect-timeouts and read-timeouts</a></td>
  <td><code>proxy-connect-timeout, proxy-read-timeout</code></td>
  <td>Set the time that the ALB waits to connect to and read from the back-end app before the back-end app is considered unavailable.</td>
  </tr>
  <tr>
  <td><a href="#keepalive-requests">Keepalive requests</a></td>
  <td><code>keepalive-requests</code></td>
  <td>Set the maximum number of requests that can be served through one keepalive connection.</td>
  </tr>
  <tr>
  <td><a href="#keepalive-timeout">Keepalive timeout</a></td>
  <td><code>keepalive-timeout</code></td>
  <td>Set the maximum time that a keepalive connection stays open on the server.</td>
  </tr>
  <tr>
  <td><a href="#proxy-next-upstream-config">Proxy next upstream</a></td>
  <td><code>proxy-next-upstream-config</code></td>
  <td>Set when the ALB can pass a request to the next upstream server.</td>
  </tr>
  <tr>
  <td><a href="#sticky-cookie-services">Session-affinity with cookies</a></td>
  <td><code>sticky-cookie-services</code></td>
  <td>Always route incoming network traffic to the same upstream server by using a sticky cookie.</td>
  </tr>
  <tr>
  <td><a href="#upstream-keepalive">Upstream keepalive</a></td>
  <td><code>upstream-keepalive</code></td>
  <td>Set the maximum number of idle keepalive connections for an upstream server.</td>
  </tr>
  </tbody></table>

<br>

  <table>
  <col width="20%">
  <col width="20%">
  <col width="60%">
  <thead>
  <th>HTTPS and TLS/SSL authentication annotations</th>
  <th>Name</th>
  <th>Description</th>
  </thead>
  <tbody>
  <tr>
  <td><a href="#appid-auth">{{site.data.keyword.appid_short}} Authentication</a></td>
  <td><code>appid-auth</code></td>
  <td>Use {{site.data.keyword.appid_full_notm}} to authenticate with your app.</td>
  </tr>
  <tr>
  <td><a href="#custom-port">Custom HTTP and HTTPS ports</a></td>
  <td><code>custom-port</code></td>
  <td>Change the default ports for HTTP (port 80) and HTTPS (port 443) network traffic.</td>
  </tr>
  <tr>
  <td><a href="#redirect-to-https">HTTP redirects to HTTPS</a></td>
  <td><code>redirect-to-https</code></td>
  <td>Redirect insecure HTTP requests on your domain to HTTPS.</td>
  </tr>
  <tr>
  <td><a href="#hsts">HTTP Strict Transport Security (HSTS)</a></td>
  <td><code>hsts</code></td>
  <td>Set the browser to only access the domain using HTTPS.</td>
  </tr>
  <tr>
  <td><a href="#mutual-auth">Mutual authentication</a></td>
  <td><code>mutual-auth</code></td>
  <td>Configure mutual authentication for the ALB.</td>
  </tr>
  <tr>
  <td><a href="#ssl-services">SSL services support</a></td>
  <td><code>ssl-services</code></td>
  <td>Allow SSL services support to encrypt traffic to your upstream apps that require HTTPS. </td>
  </tr>
  </tbody></table>

<br>

<table>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>Istio annotations</th>
<th>Name</th>
<th>Description</th>
</thead>
<tbody>
<tr>
<td><a href="#istio-services">Istio services</a></td>
<td><code>istio-services</code></td>
<td>Route traffic to Istio-managed services.</td>
</tr>
</tbody></table>

<br>

<table>
<col width="20%">
<col width="20%">
<col width="60%">
 <thead>
 <th>Proxy buffer annotations</th>
 <th>Name</th>
 <th>Description</th>
 </thead>
 <tbody>
 <tr>
 <td><a href="#proxy-buffering">Client response data buffering</a></td>
 <td><code>proxy-buffering</code></td>
 <td>Disable the buffering of a client response on the ALB while sending the response to the client.</td>
 </tr>
 <tr>
 <td><a href="#proxy-buffers">Proxy buffers</a></td>
 <td><code>proxy-buffers</code></td>
 <td>Set the number and size of the buffers that read a response for a single connection from the proxied server.</td>
 </tr>
 <tr>
 <td><a href="#proxy-buffer-size">Proxy buffer size</a></td>
 <td><code>proxy-buffer-size</code></td>
 <td>Set the size of the buffer that reads the first part of the response that is received from the proxied server.</td>
 </tr>
 <tr>
 <td><a href="#proxy-busy-buffers-size">Proxy busy buffers size</a></td>
 <td><code>proxy-busy-buffers-size</code></td>
 <td>Set the size of proxy buffers that can be busy.</td>
 </tr>
 </tbody></table>

<br>

<table>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>Request and response annotations</th>
<th>Name</th>
<th>Description</th>
</thead>
<tbody>
<tr>
<td><a href="#proxy-add-headers">Additional client request or response header</a></td>
<td><code>proxy-add-headers, response-add-headers</code></td>
<td>Add header information to a client request before forwarding the request to your back-end app or to a client response before sending the response to the client.</td>
</tr>
<tr>
<td><a href="#response-remove-headers">Client response header removal</a></td>
<td><code>response-remove-headers</code></td>
<td>Remove header information from a client response before forwarding the response to the client.</td>
</tr>
<tr>
<td><a href="#client-max-body-size">Client request body size</a></td>
<td><code>client-max-body-size</code></td>
<td>Set the maximum size of the body that the client can send as part of a request.</td>
</tr>
<tr>
<td><a href="#large-client-header-buffers">Large client header buffers</a></td>
<td><code>large-client-header-buffers</code></td>
<td>Set the maximum number and size of buffers that read large client request headers.</td>
</tr>
</tbody></table>

<br>

<table>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>Service limit annotations</th>
<th>Name</th>
<th>Description</th>
</thead>
<tbody>
<tr>
<td><a href="#global-rate-limit">Global rate limits</a></td>
<td><code>global-rate-limit</code></td>
<td>Limit the request processing rate and number of connections per a defined key for all services.</td>
</tr>
<tr>
<td><a href="#service-rate-limit">Service rate limits</a></td>
<td><code>service-rate-limit</code></td>
<td>Limit the request processing rate and the number of connections per a defined key for specific services.</td>
</tr>
</tbody></table>

<br>



## General annotations
{: #general}

### External services (proxy-external-service)
{: #proxy-external-service}

Add path definitions to external services, such as services hosted in {{site.data.keyword.Bluemix_notm}}.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>Add path definitions to external services. Use this annotation only when your app operates on an external service instead of a backend service. When you use this annotation to create an external service route, only `client-max-body-size`, `proxy-read-timeout`, `proxy-connect-timeout`, and `proxy-buffering` annotations are supported in conjunction. Any other annotations are not supported in conjunction with `proxy-external-service`.
</dd>
<dt>Sample Ingress resource YAML</dt>
<dd>

<pre class="codeblock">
<code>
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: cafe-ingress
  annotations:
    ingress.bluemix.net/proxy-external-service: "path=&lt;mypath&gt; external-svc=https:&lt;external_service&gt; host=&lt;mydomain&gt;"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 80
</code></pre>

<table>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
 </thead>
 <tbody>
 <tr>
 <td><code>path</code></td>
 <td>Replace <code>&lt;<em>mypath</em>&gt;</code> with the path that the external service listens on.</td>
 </tr>
 <tr>
 <td><code>external-svc</code></td>
 <td>Replace <code>&lt;<em>external_service</em>&gt;</code> with the external service to be called. For example, <code>https://&lt;myservice&gt;.&lt;region&gt;.mybluemix.net</code>.</td>
 </tr>
 <tr>
 <td><code>host</code></td>
 <td>Replace <code>&lt;<em>mydomain</em>&gt;</code> with the host domain for the external service.</td>
 </tr>
 </tbody></table>

 </dd></dl>

<br />


### Location modifier (location-modifier)
{: #location-modifier}

Modify the way the ALB matches the request URI against the app path.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>By default, ALBs process the paths that apps listen on as prefixes. When an ALB receives a request to an app, the ALB checks the Ingress resource for a path (as a prefix) that matches the beginning of the request URI. If a match is found, the request is forwarded to the IP address of the pod where the app is deployed.<br><br>The `location-modifier` annotation changes the way the ALB searches for matches by modifying the location block configuration. The location block determines how requests are handled for the app path.<br><br>**Note**: To handle regular expression (regex) paths, this annotation is required.</dd>

<dt>Supported modifiers</dt>
<dd>

<table>
 <col width="10%">
 <col width="90%">
 <thead>
 <th>Modifier</th>
 <th>Description</th>
 </thead>
 <tbody>
 <tr>
 <td><code>=</code></td>
 <td>The equal sign modifier causes the ALB to select exact matches only. When an exact match is found, the search stops and the matching path is selected.</td>
 </tr>
 <tr>
 <td><code>~</code></td>
 <td>The tilde modifier causes the ALB to process paths as case-sensitive regex paths during matching.</td>
 </tr>
 <tr>
 <td><code>~\*</code></td>
 <td>The tilde followed by an asterisk modifier causes the ALB to process paths as case-insensitive regex paths during matching.</td>
 </tr>
 <tr>
 <td><code>^~</code></td>
 <td>The carat followed by a tilde modifier causes the ALB to select the best non-regex match instead of a regex path.</td>
 </tr>
 </tbody>
</table>

</dd>

<dt>Sample Ingress resource YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
name: myingress
annotations:
  ingress.bluemix.net/location-modifier: "modifier='&lt;location_modifier&gt;' serviceName=&lt;myservice&gt;;modifier='&lt;location_modifier&gt;' serviceName=&lt;myservice2&gt;"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: &lt;myservice&gt;
          servicePort: 80</code></pre>

 <table>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
  </thead>
  <tbody>
  <tr>
  <td><code>modifier</code></td>
  <td>Replace <code>&lt;<em>location_modifier</em>&gt;</code> with the location modifier you want to use for the path. Supported modifiers are <code>'='</code>, <code>'~'</code>, <code>'~\*'</code>, and <code>'^~'</code>. You must surround the modifiers in single quotes.</td>
  </tr>
  <tr>
  <td><code>serviceName</code></td>
  <td>Replace <code>&lt;<em>myservice</em>&gt;</code> with the name of the Kubernetes service you created for your app.</td>
  </tr>
  </tbody></table>
  </dd>
  </dl>

<br />


### Private ALB routing (ALB-ID)
{: #alb-id}

Route incoming requests to your apps with a private ALB.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>
Choose a private ALB to route incoming requests with instead of the public ALB.</dd>


<dt>Sample Ingress resource YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
name: myingress
annotations:
  ingress.bluemix.net/ALB-ID: "&lt;private_ALB_ID&gt;"
spec:
tls:
- hosts:
  - mydomain
  secretName: mytlssecret
rules:
- host: mydomain
  http:
    paths:
    - path: /
      backend:
        serviceName: myservice
        servicePort: 8080</code></pre>

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
</thead>
<tbody>
<tr>
<td><code>&lt;private_ALB_ID&gt;</code></td>
<td>The ID for your private ALB. Run <code>bx cs albs --cluster <my_cluster></code> to find the private ALB ID.
</td>
</tr>
</tbody></table>
</dd>
</dl>

<br />


### Rewrite paths (rewrite-path)
{: #rewrite-path}

Route incoming network traffic on an ALB domain path to a different path that your back-end application listens on.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>Your Ingress ALB domain routes incoming network traffic on <code>mykubecluster.us-south.containers.mybluemix.net/beans</code> to your app. Your app listens on <code>/coffee</code>, instead of <code>/beans</code>. To forward incoming network traffic to your app, add the rewrite annotation to your Ingress resource configuration file. The rewrite annotation ensures that incoming network traffic on <code>/beans</code> is forwarded to your app by using the <code>/coffee</code> path. When including multiple services, use only a semi-colon (;) to separate them.</dd>
<dt>Sample Ingress resource YAML</dt>
<dd>
<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/rewrite-path: "serviceName=&lt;myservice1&gt; rewrite=&lt;target_path1&gt;;serviceName=&lt;myservice2&gt; rewrite=&lt;target_path2&gt;"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /beans
        backend:
          serviceName: myservice1
          servicePort: 80
</code></pre>

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Replace <code>&lt;<em>myservice</em>&gt;</code> with the name of the Kubernetes service that you created for your app.</td>
</tr>
<tr>
<td><code>rewrite</code></td>
<td>Replace <code>&lt;<em>target_path</em>&gt;</code> with the path that your app listens on. Incoming network traffic on the ALB domain is forwarded to the Kubernetes service by using this path. Most apps do not listen on a specific path, but use the root path and a specific port. In the example above, the rewrite path was defined as <code>/coffee</code>.</td>
</tr>
</tbody></table>

</dd></dl>

<br />


### TCP ports for application load balancers (tcp-ports)
{: #tcp-ports}

Access an app via a non-standard TCP port.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>
Use this annotation for an app that is running a TCP streams workload.

<p>**Note**: The ALB operates in pass-through mode and forwards traffic to backend apps. SSL termination is not supported in this case.</p>
</dd>


<dt>Sample Ingress resource YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
name: myingress
annotations:
  ingress.bluemix.net/tcp-ports: "serviceName=&lt;myservice&gt; ingressPort=&lt;ingress_port&gt; [servicePort=&lt;service_port&gt;]"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: &lt;myservice&gt;
          servicePort: 80</code></pre>

 <table>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
  </thead>
  <tbody>
  <tr>
  <td><code>serviceName</code></td>
  <td>Replace <code>&lt;<em>myservice</em>&gt;</code> with the name of the Kubernetes service to access over non-standard TCP port.</td>
  </tr>
  <tr>
  <td><code>ingressPort</code></td>
  <td>Replace <code>&lt;<em>ingress_port</em>&gt;</code> with the TCP port on which you want to access your app.</td>
  </tr>
  <tr>
  <td><code>servicePort</code></td>
  <td>This parameter is optional. When provided, the port is substituted to this value before traffic is sent to the backend app. Otherwise, the port remains same as the Ingress port.</td>
  </tr>
  </tbody></table>

 </dd>
 <dt>Usage</dt>
 <dd><ol><li>Review open ports for your ALB.
<pre class="pre">
<code>kubectl get service -n kube-system</code></pre>
Your CLI output looks similar to the following:
<pre class="screen">
<code>NAME                     CLUSTER-IP     EXTERNAL-IP     PORT(S)                      AGE
public-ingress-ctl-svc   10.10.10.149   169.60.16.246   80:30776/TCP,443:30412/TCP   8d</code></pre></li>
<li>Open the ALB config map.
<pre class="pre">
<code>kubectl edit configmap ibm-cloud-provider-ingress-cm -n kube-system</code></pre></li>
<li>Add the TCP ports to the config map. Replace &lt;port&gt; with the TCP ports that you want to open.
<pre class="codeblock">
<code>apiVersion: v1
kind: ConfigMap
data:
 public-ports: &lt;port1&gt;;&lt;port2&gt;
metadata:
 creationTimestamp: 2017-08-22T19:06:51Z
 name: ibm-cloud-provider-ingress-cm
 namespace: kube-system
 resourceVersion: "1320"
 selfLink: /api/v1/namespaces/kube-system/configmaps/ibm-cloud-provider-ingress-cm
 uid: &lt;uid&gt;</code></pre></li>
 <li>Verify that your ALB is re-configured with the TCP ports.
<pre class="pre">
<code>kubectl get service -n kube-system</code></pre>
Your CLI output looks similar to the following:
<pre class="screen">
<code>NAME                     CLUSTER-IP     EXTERNAL-IP     PORT(S)                      AGE
public-ingress-ctl-svc   10.10.10.149   169.60.16.246   &lt;port1&gt;:30776/TCP,&lt;port2&gt;:30412/TCP   8d</code></pre></li>
<li>Configure your Ingress to access your app via a non-standard TCP port. Use the sample YAML file in this reference. </li>
<li>Update your ALB configuration.
<pre class="pre">
<code>kubectl apply -f &lt;yaml_file&gt;</code></pre>
</li>
<li>Open your preferred web browser to access your app. Example: <code>https://&lt;ibmdomain&gt;:&lt;ingressPort&gt;/</code></li></ol></dd></dl>

<br />


## Connection annotations
{: #connection}

### Custom connect-timeouts and read-timeouts (proxy-connect-timeout, proxy-read-timeout)
{: #proxy-connect-timeout}

Set the time that the ALB waits to connect to and read from the back-end app before the back-end app is considered unavailable.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>When a client request is sent to the Ingress ALB, a connection to the back-end app is opened by the ALB. By default, the ALB waits 60 seconds to receive a reply from the back-end app. If the back-end app does not reply within 60 seconds, then the connection request is aborted and the back-end app is considered to be unavailable.

</br></br>
After the ALB is connected to the back-end app, response data is read from the back-end app by the ALB. During this read operation, the ALB waits a maximum of 60 seconds between two read operations to receive data from the back-end app. If the back-end app does not send data within 60 seconds, the connection to the back-end app is closed and the app is considered to be not available.
</br></br>
A 60 second connect-timeout and read-timeout is the default timeout on a proxy and usually should not be changed.
</br></br>
If the availability of your app is not steady or your app is slow to respond because of high workloads, you might want to increase the connect-timeout or read-timeout. Keep in mind that increasing the timeout impacts the performance of the ALB as the connection to the back-end app must stay open until the timeout is reached.
</br></br>
On the other hand, you can decrease the timeout to gain performance on the ALB. Ensure that your back-end app is able to handle requests within the specified timeout, even during higher workloads.</dd>
<dt>Sample Ingress resource YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/proxy-connect-timeout: "&lt;connect_timeout&gt;s"
   ingress.bluemix.net/proxy-read-timeout: "&lt;read_timeout&gt;s"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080</code></pre>

<table>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
 </thead>
 <tbody>
 <tr>
 <td><code>&lt;connect_timeout&gt;</code></td>
 <td>The number of seconds to wait to connect to the back-end app, for example <code>65s</code>. <strong>Note:</strong> A connect-timeout cannot exceed 75 seconds.</td>
 </tr>
 <tr>
 <td><code>&lt;read_timeout&gt;</code></td>
 <td>The number of seconds to wait before the back-end app is read, for example <code>65s</code>. <strong>Note:</strong> A read-timeout cannot exceed 120 seconds.</td>
 </tr>
 </tbody></table>

 </dd></dl>

<br />



### Keepalive requests (keepalive-requests)
{: #keepalive-requests}

Set the maximum number of requests that can be served through one keepalive connection.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>
Sets the maximum number of requests that can be served through one keepalive connection.
</dd>


<dt>Sample Ingress resource YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
name: myingress
annotations:
  ingress.bluemix.net/keepalive-requests: "serviceName=&lt;myservice&gt; requests=&lt;max_requests&gt;"
spec:
tls:
- hosts:
  - mydomain
  secretName: mytlssecret
rules:
- host: mydomain
  http:
    paths:
    - path: /
      backend:
        serviceName: &lt;myservice&gt;
        servicePort: 8080</code></pre>

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Replace <code>&lt;<em>myservice</em>&gt;</code> with the name of the Kubernetes service that you created for your app. This parameter is optional. The configuration is applied to all of the services in the Ingress host unless a service is specified. If the parameter is provided, the keepalive requests are set for the given service. If the parameter is not provided, the keepalive requests are set at the server level of the <code>nginx.conf</code> for all the services that do not have the keepalive requests configured.</td>
</tr>
<tr>
<td><code>requests</code></td>
<td>Replace <code>&lt;<em>max_requests</em>&gt;</code> with the maximum number of requests that can be served through one keepalive connection.</td>
</tr>
</tbody></table>

</dd>
</dl>

<br />



### Keepalive timeout (keepalive-timeout)
{: #keepalive-timeout}

Set the maximum time that a keepalive connection stays open on the server side.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>
Sets the maximum time that a keepalive connection stays open on the server.
</dd>


<dt>Sample Ingress resource YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/keepalive-timeout: "serviceName=&lt;myservice&gt; timeout=&lt;time&gt;s"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080</code></pre>

<table>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
 </thead>
 <tbody>
 <tr>
 <td><code>serviceName</code></td>
 <td>Replace <code>&lt;<em>myservice</em>&gt;</code> with the name of the Kubernetes service that you created for your app. This parameter is optional. If the parameter is provided, the keepalive timeout is set for the given service. If the parameter is not provided, the keepalive timeout is set at the server level of the <code>nginx.conf</code> for all the services that do not have the keepalive timeout configured.</td>
 </tr>
 <tr>
 <td><code>timeout</code></td>
 <td>Replace <code>&lt;<em>time</em>&gt;</code> with an amount of time in seconds. Example: <code>timeout=20s</code>. A zero value disables the keepalive client connections.</td>
 </tr>
 </tbody></table>

 </dd>
 </dl>

<br />


### Proxy next upstream (proxy-next-upstream-config)
{: #proxy-next-upstream-config}

Set when the ALB can pass a request to the next upstream server.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>
The Ingress ALB acts as a proxy between the client app and your app. Some app setups require multiple upstream servers that handle incoming client requests from the ALB. Sometimes the proxy server that the ALB uses cannot establish a connection with an upstream server that the app uses. The ALB can then try to establish a connection with the next upstream server to pass the request to it instead. You can use the `proxy-next-upstream-config` annotation to set in which cases, how long, and how many times the ALB can try to pass a request to the next upstream server.<br><br><strong>Note</strong>: Timeout is always configured when you use `proxy-next-upstream-config`, so don't add `timeout=true` to this annotation.
</dd>
<dt>Sample Ingress resource YAML</dt>
<dd>
<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/proxy-next-upstream-config: "serviceName=&lt;myservice1&gt; retries=&lt;tries&gt; timeout=&lt;time&gt; error=true http_502=true; serviceName=&lt;myservice2&gt; http_403=true non_idempotent=true"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice1
          servicePort: 80
</code></pre>

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Replace <code>&lt;<em>myservice</em>&gt;</code> with the name of the Kubernetes service that you created for your app.</td>
</tr>
<tr>
<td><code>retries</code></td>
<td>Replace <code>&lt;<em>tries</em>&gt;</code> with the maximum amount of times that the ALB tries to pass a request to the next upstream server. This number includes the original request. To turn off this limitation, use <code>0</code>. If you do not specify a value, the default value <code>0</code> is used.
</td>
</tr>
<tr>
<td><code>timeout</code></td>
<td>Replace <code>&lt;<em>time</em>&gt;</code> with the maximum amount of time, in seconds, that the ALB tries to pass a request to the next upstream server. For example, to set a time of 30 seconds, enter <code>30s</code>. To turn off this limitation, use <code>0</code>. If you do not specify a value, the default value <code>0</code> is used.
</td>
</tr>
<tr>
<td><code>error</code></td>
<td>If set to <code>true</code>, the ALB passes a request to the next upstream server when an error occurred while establishing a connection with the first upstream server, passing a request to it, or reading the response header.
</td>
</tr>
<tr>
<td><code>invalid_header</code></td>
<td>If set to <code>true</code>, the ALB passes a request to the next upstream server when the first upstream server returns an empty or invalid response.
</td>
</tr>
<tr>
<td><code>http_502</code></td>
<td>If set to <code>true</code>, the ALB passes a request to the next upstream server when the first upstream server returns a response with the code 502. You can designate the following HTTP response codes: <code>500</code>, <code>502</code>, <code>503</code>, <code>504</code>, <code>403</code>, <code>404</code>, <code>429</code>.
</td>
</tr>
<tr>
<td><code>non_idempotent</code></td>
<td>If set to <code>true</code>, the ALB can pass requests with a non-idempotent method to the next upstream server. By default, the ALB does not pass these requests to the next upstream server.
</td>
</tr>
<tr>
<td><code>off</code></td>
<td>To prevent the ALB from passing requests to the next upstream server, set to <code>true</code>.
</td>
</tr>
</tbody></table>
</dd>
</dl>

<br />


### Session-affinity with cookies (sticky-cookie-services)
{: #sticky-cookie-services}

Use the sticky cookie annotation to add session affinity to your ALB and always route incoming network traffic to the same upstream server.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>For high availability, some app setups require you to deploy multiple upstream servers that handle incoming client requests. When a client connects to you back-end app, you can use session-affinity so that a client is served by the same upstream server for the duration of a session or for the time it takes to complete a task. You can configure your ALB to ensure session-affinity by always routing incoming network traffic to the same upstream server.

</br></br>
Every client that connects to your back-end app is assigned to one of the available upstream servers by the ALB. The ALB creates a session cookie that is stored in the client's app, which is included in the header information of every request between the ALB and the client. The information in the cookie ensures that all requests are handled by the same upstream server throughout the session.

</br></br>
When you include multiple services, use a semi-colon (;) to separate them.</dd>
<dt>Sample Ingress resource YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/sticky-cookie-services: "serviceName=&lt;myservice1&gt; name=&lt;cookie_name1&gt; expires=&lt;expiration_time1&gt; path=&lt;cookie_path1&gt; hash=&lt;hash_algorithm1&gt;;serviceName=&lt;myservice2&gt; name=&lt;cookie_name2&gt; expires=&lt;expiration_time2&gt; path=&lt;cookie_path2&gt; hash=&lt;hash_algorithm2&gt;"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: &lt;myservice1&gt;
          servicePort: 8080
      - path: /myapp
        backend:
          serviceName: &lt;myservice2&gt;
          servicePort: 80</code></pre>

  <table>
  <caption>Understanding the YAML file components</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
  </thead>
  <tbody>
  <tr>
  <td><code>serviceName</code></td>
  <td>Replace <code>&lt;<em>myservice</em>&gt;</code> with the name of the Kubernetes service that you created for your app.</td>
  </tr>
  <tr>
  <td><code>name</code></td>
  <td>Replace <code>&lt;<em>cookie_name</em>&gt;</code> with the name of a sticky cookie that is created during a session.</td>
  </tr>
  <tr>
  <td><code>expires</code></td>
  <td>Replace <code>&lt;<em>expiration_time</em>&gt;</code> with the time in seconds (s), minutes (m), or hours (h) before the sticky cookie expires. This time is independent of the user activity. After the cookie is expired, the cookie is deleted by the client web browser and no longer sent to the ALB. For example, to set an expiration time of 1 second, 1 minute, or 1 hour, enter <code>1s</code>, <code>1m</code>, or <code>1h</code>.</td>
  </tr>
  <tr>
  <td><code>path</code></td>
  <td>Replace <code>&lt;<em>cookie_path</em>&gt;</code> with the path that is appended to the Ingress subdomain and that indicates for which domains and subdomains the cookie is sent to the ALB. For example, if your Ingress domain is <code>www.myingress.com</code> and you want to send the cookie in every client request, you must set <code>path=/</code>. If you want to send the cookie only for <code>www.myingress.com/myapp</code> and all its subdomains, then you must set <code>path=/myapp</code>.</td>
  </tr>
  <tr>
  <td><code>hash</code></td>
  <td>Replace <code>&lt;<em>hash_algorithm</em>&gt;</code> with the hash algorithm that protects the information in the cookie. Only <code>sha1</code> is supported. SHA1 creates a hash sum based on the information in the cookie and appends this hash sum to the cookie. The server can decrypt the information in the cookie and verify data integrity.</td>
  </tr>
  </tbody></table>

 </dd></dl>

<br />


### Upstream keepalive (upstream-keepalive)
{: #upstream-keepalive}

Set the maximum number of idle keepalive connections for an upstream server.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>
Set the maximum number of idle keepalive connections to the upstream server of a given service. The upstream server has 64 idle keepalive connections by default.
</dd>


 <dt>Sample Ingress resource YAML</dt>
 <dd>

 <pre class="codeblock">
 <code>apiVersion: extensions/v1beta1
 kind: Ingress
 metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/upstream-keepalive: "serviceName=&lt;myservice&gt; keepalive=&lt;max_connections&gt;"
 spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

 <table>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
  </thead>
  <tbody>
  <tr>
  <td><code>serviceName</code></td>
  <td>Replace <code>&lt;<em>myservice</em>&gt;</code> with the name of the Kubernetes service that you created for your app.</td>
  </tr>
  <tr>
  <td><code>keepalive</code></td>
  <td>Replace <code>&lt;<em>max_connections</em>&gt;</code> with the maximum number of idle keepalive connections to the upstream server. The default is <code>64</code>. A <code>0</code> value disables upstream keepalive connections for the given service.</td>
  </tr>
  </tbody></table>
  </dd>
  </dl>

<br />


## HTTPS and TLS/SSL authentication annotations
{: #https-auth}

### {{site.data.keyword.appid_short_notm}} Authentication (appid-auth)
{: #appid-auth}

  Use {{site.data.keyword.appid_full_notm}} to authenticate with your application.
  {:shortdesc}

  <dl>
  <dt>Description</dt>
  <dd>
  Authenticate web or API HTTP/HTTPS requests with {{site.data.keyword.appid_short_notm}}.

  <p>If you set the request type to <code>web</code>, a web request that contains an {{site.data.keyword.appid_short_notm}} access token is validated. If token validation fails, the web request is rejected. If the request does not contain an access token, then the request is redirected to the {{site.data.keyword.appid_short_notm}} login page. **Note**: For {{site.data.keyword.appid_short_notm}} web authentication to work, cookies must be enabled in the user's browser.</p>

  <p>If you set the request type to <code>api</code>, an API request that contains an {{site.data.keyword.appid_short_notm}} access token is validated. If the request does not contain an access token, a <code>401: Unauthorized</code> error message is returned to the user.</p>

  <p>**Note**: For security reasons, {{site.data.keyword.appid_short_notm}} authentication only supports backends with TLS/SSL enabled.</p>
  </dd>
   <dt>Sample Ingress resource YAML</dt>
   <dd>

   <pre class="codeblock">
   <code>apiVersion: extensions/v1beta1
   kind: Ingress
   metadata:
    name: myingress
    annotations:
      ingress.bluemix.net/appid-auth: "bindSecret=&lt;bind_secret&gt; namespace=&lt;namespace&gt; requestType=&lt;request_type&gt; serviceName=&lt;myservice&gt;"
   spec:
    tls:
    - hosts:
      - mydomain
      secretName: mytlssecret
    rules:
    - host: mydomain
      http:
        paths:
        - path: /
          backend:
            serviceName: myservice
            servicePort: 8080</code></pre>

   <table>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
    </thead>
    <tbody>
    <tr>
    <td><code>bindSecret</code></td>
    <td>Replace <em><code>&lt;bind_secret&gt;</code></em> with the Kubernetes secret which stores the bind secret.</td>
    </tr>
    <tr>
    <td><code>namespace</code></td>
    <td>Replace <em><code>&lt;namespace&gt;</code></em> with the namespace of the bind secret. This field defaults to the `default` namespace.</td>
    </tr>
    <tr>
    <td><code>requestType</code></td>
    <td>Replace <code><em>&lt;request_type&gt;</em></code> with the type of request you want to send to {{site.data.keyword.appid_short_notm}}. Accepted values are `web` or `api`. The default is `api`.</td>
    </tr>
    <tr>
    <td><code>serviceName</code></td>
    <td>Replace <code><em>&lt;myservice&gt;</em></code> with the name of the Kubernetes service that you created for your app. This field is optional. If a service name is not included, then the annotation is enabled for all services.  If a service name is included, then the annotation is enabled only for that service. Separate multiple services with a semi-colon (;).</td>
    </tr>
    </tbody></table>
    </dd>
    <dt>Usage</dt>
    <dd>Because the application uses {{site.data.keyword.appid_short_notm}} for authenication, you must provision an {{site.data.keyword.appid_short_notm}} instance, configure the instance with valid redirect URIs, and generate a bind secret.
    <ol>
    <li>Provision an [{{site.data.keyword.appid_short_notm}} instance](https://console.bluemix.net/catalog/services/app-id).</li>
    <li>In the {{site.data.keyword.appid_short_notm}} management console, add redirectURIs for your app.</li>
    <li>Create a bind secret.
    <pre class="pre"><code>bx cs cluster-service-bind &lt;my_cluster&gt; &lt;my_namespace&gt; &lt;my_service_instance_GUID&gt;</code></pre> </li>
    <li>Configure the <code>appid-auth</code> annotation.</li>
    </ol></dd>
    </dl>

<br />



### Custom HTTP and HTTPS ports (custom-port)
{: #custom-port}

Change the default ports for HTTP (port 80) and HTTPS (port 443) network traffic.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>By default, the Ingress ALB is configured to listen for incoming HTTP network traffic on port 80 and for incoming HTTPS network traffic on port 443. You can change the default ports to add security to your ALB domain, or to enable only an HTTPS port.
</dd>


<dt>Sample Ingress resource YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/custom-port: "protocol=&lt;protocol1&gt; port=&lt;port1&gt;;protocol=&lt;protocol2&gt; port=&lt;port2&gt;"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080</code></pre>

<table>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
 </thead>
 <tbody>
 <tr>
 <td><code>&lt;protocol&gt;</code></td>
 <td>Enter <code>http</code> or <code>https</code> to change the default port for incoming HTTP or HTTPS network traffic.</td>
 </tr>
 <tr>
 <td><code>&lt;port&gt;</code></td>
 <td>Enter the port number that you want to use for incoming HTTP or HTTPS network traffic.  <p><strong>Note:</strong> When a custom port is specified for either HTTP or HTTPS, the default ports are no longer valid for both HTTP and HTTPS. For example, to change the default port for HTTPS to 8443, but use the default port for HTTP, you must set custom ports for both: <code>custom-port: "protocol=http port=80; protocol=https port=8443"</code>.</p></td>
 </tr>
 </tbody></table>

 </dd>
 <dt>Usage</dt>
 <dd><ol><li>Review open ports for your ALB.
<pre class="pre">
<code>kubectl get service -n kube-system</code></pre>
Your CLI output looks similar to the following:
<pre class="screen">
<code>NAME                     CLUSTER-IP     EXTERNAL-IP     PORT(S)                      AGE
public-ingress-ctl-svc   10.10.10.149   169.60.16.246   80:30776/TCP,443:30412/TCP   8d</code></pre></li>
<li>Open the ALB config map.
<pre class="pre">
<code>kubectl edit configmap ibm-cloud-provider-ingress-cm -n kube-system</code></pre></li>
<li>Add the non-default HTTP and HTTPS ports to the config map. Replace &lt;port&gt; with the HTTP or HTTPS port that you want to open.
<pre class="codeblock">
<code>apiVersion: v1
kind: ConfigMap
data:
 public-ports: &lt;port1&gt;;&lt;port2&gt;
metadata:
 creationTimestamp: 2017-08-22T19:06:51Z
 name: ibm-cloud-provider-ingress-cm
 namespace: kube-system
 resourceVersion: "1320"
 selfLink: /api/v1/namespaces/kube-system/configmaps/ibm-cloud-provider-ingress-cm
 uid: &lt;uid&gt;</code></pre></li>
 <li>Verify that your ALB is re-configured with the non-default ports.
<pre class="pre">
<code>kubectl get service -n kube-system</code></pre>
Your CLI output looks similar to the following:
<pre class="screen">
<code>NAME                     CLUSTER-IP     EXTERNAL-IP     PORT(S)                      AGE
public-ingress-ctl-svc   10.10.10.149   169.60.16.246   &lt;port1&gt;:30776/TCP,&lt;port2&gt;:30412/TCP   8d</code></pre></li>
<li>Configure your Ingress to use the non-default ports when routing incoming network traffic to your services. Use the sample YAML file in this reference. </li>
<li>Update your ALB configuration.
<pre class="pre">
<code>kubectl apply -f &lt;yaml_file&gt;</code></pre>
</li>
<li>Open your preferred web browser to access your app. Example: <code>https://&lt;ibmdomain&gt;:&lt;port&gt;/&lt;service_path&gt;/</code></li></ol></dd></dl>

<br />



### HTTP redirects to HTTPS (redirect-to-https)
{: #redirect-to-https}

Convert insecure HTTP client requests to HTTPS.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>You set up your Ingress ALB to secure your domain with the IBM-provided TLS certificate or your custom TLS certificate. Some users might try to access your apps by using an insecure <code>http</code> request to your ALB domain, for example <code>http://www.myingress.com</code>, instead of using <code>https</code>. You can use the redirect annotation to always convert insecure HTTP requests to HTTPS. If you do not use this annotation, insecure HTTP requests are not converted into HTTPS requests by default and might expose unencrypted confidential information to the public.

</br></br>
Redirecting HTTP requests to HTTPS is disabled by default.</dd>

<dt>Sample Ingress resource YAML</dt>
<dd>
<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/redirect-to-https: "True"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080</code></pre>
</dd></dl>

<br />


### HTTP Strict Transport Security (hsts)
{: #hsts}

<dl>
<dt>Description</dt>
<dd>
HSTS instructs the browser to only access a domain by using HTTPS. Even if the user enters or follows a plain HTTP link, the browser strictly upgrades the connection to HTTPS.
</dd>


<dt>Sample Ingress resource YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/hsts: enabled=&lt;true&gt; maxAge=&lt;31536000&gt; includeSubdomains=&lt;true&gt;
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice1
          servicePort: 8443
      - path: /
        backend:
          serviceName: myservice2
          servicePort: 8444
          </code></pre>

<table>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
  </thead>
  <tbody>
  <tr>
  <td><code>enabled</code></td>
  <td>Use <code>true</code> to enable HSTS.</td>
  </tr>
    <tr>
  <td><code>maxAge</code></td>
  <td>Replace <code>&lt;<em>31536000</em>&gt;</code> with an integer representing how many seconds a browser will cache sending requests straight to HTTPS. The default is <code>31536000</code>, which is equal to 1 year.</td>
  </tr>
  <tr>
  <td><code>includeSubdomains</code></td>
  <td>Use <code>true</code> to tell the browser that the HSTS policy also applies to all subdomains of the current domain. The default is <code>true</code>. </td>
  </tr>
  </tbody></table>

  </dd>
  </dl>


<br />


### Mutual authentication (mutual-auth)
{: #mutual-auth}

Configure mutual authentication for the ALB.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>
Configure mutual authentication for the Ingress ALB. The client authenticates the server and the server also authenticates the client by using certificates. Mutual authentication is also known as certificate-based authentication or two-way authentication.
</dd>

<dt>Pre-requisites</dt>
<dd>
<ul>
<li>[You must have a valid secret that contains the required certificate authority (CA)](cs_app.html#secrets). The <code>client.key</code> and <code>client.crt</code> are also needed to authenticate with mutual authentication.</li>
<li>To enable mutual authentication on a port other than 443, [configure the ALB to open the valid port](cs_ingress.html#opening_ingress_ports).</li>
</ul>
</dd>


<dt>Sample Ingress resource YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
name: myingress
annotations:
  ingress.bluemix.net/mutual-auth: "secretName=&lt;mysecret&gt; port=&lt;port&gt; serviceName=&lt;servicename1&gt;,&lt;servicename2&gt;"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080
          </code></pre>

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
</thead>
<tbody>
<tr>
<td><code>secretName</code></td>
<td>Replace <code>&lt;<em>mysecret</em>&gt;</code> with a name for the secret resource.</td>
</tr>
<tr>
<td><code>&lt;port&gt;</code></td>
<td>The ALB port number.</td>
</tr>
<tr>
<td><code>&lt;serviceName&gt;</code></td>
<td>The name of one or more Ingress resources. This parameter is optional.</td>
</tr>
</tbody></table>

</dd>
</dl>

<br />



### SSL services support (ssl-services)
{: #ssl-services}

Allow HTTPS requests and encrypt traffic to your upstream apps.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>
Encrypt traffic to your upstream apps that require HTTPS.

**Optional**: You can add [one-way authentication or mutual authentication](#ssl-services-auth) to this annotation.
</dd>


<dt>Sample Ingress resource YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: &lt;myingressname&gt;
  annotations:
    ingress.bluemix.net/ssl-services: "ssl-service=&lt;myservice1&gt; [ssl-secret=&lt;service1-ssl-secret&gt;];ssl-service=&lt;myservice2&gt; [ssl-secret=&lt;service2-ssl-secret&gt;]
spec:
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice1
          servicePort: 8443
      - path: /
        backend:
          serviceName: myservice2
          servicePort: 8444</code></pre>

<table>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
  </thead>
  <tbody>
  <tr>
  <td><code>ssl-service</code></td>
  <td>Replace <code>&lt;<em>myservice</em>&gt;</code> with the name of the service that requires HTTPS. Traffic is encrypted from the ALB to this app's service.</td>
  </tr>
  <tr>
  <td><code>ssl-secret</code></td>
  <td>Replace <code>&lt;<em>service-ssl-secret</em>&gt;</code> with the secret for the service. This parameter is optional. If the parameter is provided, the value must contain the key and the certificate that your app is expecting from the client. To create a TLS secret, see [Creating secrets](cs_app.html#secrets).</td>
  </tr>
  </tbody></table>

  </dd>
</dl>

<br />


#### SSL Services support with authentication
{: #ssl-services-auth}

<dl>
<dt>Description</dt>
<dd>
Allow HTTPS requests and encrypt traffic to your upstream apps with one-way or mutual authentication for additional security.

**Note**: Before you begin, [convert the cert and key into base-64 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.base64encode.org/).

</dd>


<dt>Sample Ingress resource YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: &lt;myingressname&gt;
  annotations:
    ingress.bluemix.net/ssl-services: |
      ssl-service=&lt;myservice1&gt; ssl-secret=&lt;service1-ssl-secret&gt;;
      ssl-service=&lt;myservice2&gt; ssl-secret=&lt;service2-ssl-secret&gt;
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice1
          servicePort: 8443
      - path: /
        backend:
          serviceName: myservice2
          servicePort: 8444
          </code></pre>

<table>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
  </thead>
  <tbody>
  <tr>
  <td><code>ssl-service</code></td>
  <td>Replace <code>&lt;<em>myservice</em>&gt;</code> with the name of the service that requires HTTPS. Traffic is encrypted from the ALB to this app's service.</td>
  </tr>
  <tr>
  <td><code>ssl-secret</code></td>
  <td>Replace <code>&lt;<em>service-ssl-secret</em>&gt;</code> with the secret for the service. This parameter is optional. If the parameter is provided, the value must contain the key and the certificate that your app is expecting from the client. To create a mutual authentication secret, see [Creating secrets](cs_app.html#secrets).</td>
  </tr>
  </tbody></table>

  </dd>
  </dl>

<br />


## Istio annotations
{: #istio-annotations}

### Istio services (istio-services)
{: #istio-services}

  Route traffic to Istio-managed services.
  {:shortdesc}

  <dl>
  <dt>Description</dt>
  <dd>
  If you have Istio-managed services, you can use a cluster ALB to route HTTP/HTTPS requests to the Istio Ingress controller. The Istio Ingress controller then routes the requests to the app services. In order to route traffic, you must make changes to the Ingress resources for both the cluster ALB and the Istio Ingress controller.
    <br><br>In the Ingress resource for the cluster ALB, you must:
      <ul>
        <li>specify the `istio-services` annotation</li>
        <li>define the service path as the actual path the app listens on</li>
        <li>define the service port as the port of the Istio Ingress controller</li>
      </ul>
    <br>In the Ingress resource for the Istio Ingress controller, you must:
      <ul>
        <li>define the service path as the actual path the app listens on</li>
        <li>define the service port as the HTTP/HTTPS port of the app service that is exposed by the Istio Ingress controller</li>
  </dd>

   <dt>Sample Ingress resource YAML for the cluster ALB</dt>
   <dd>

   <pre class="codeblock">
   <code>apiVersion: extensions/v1beta1
   kind: Ingress
   metadata:
    name: myingress
    annotations:
      ingress.bluemix.net/istio-services: "enable=True serviceName=&lt;myservice1&gt; istioServiceNamespace=&lt;istio-namespace&gt; istioServiceName=&lt;istio-ingress-service&gt;"
   spec:
    tls:
    - hosts:
      - mydomain
      secretName: mytlssecret
    rules:
    - host: mydomain
      http:
        paths:
        - path: &lt;/myapp1&gt;
          backend:
            serviceName: &lt;myservice1&gt;
            servicePort: &lt;istio_ingress_port&gt;
        - path: &lt;/myapp2&gt;
          backend:
            serviceName: &lt;myservice2&gt;
            servicePort: &lt;istio_ingress_port&gt;</pre>

   <table>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
    </thead>
    <tbody>
    <tr>
    <td><code>enable</code></td>
      <td>To enable traffic routing to Istio-manages services, set to <code>True</code>.</td>
    </tr>
    <tr>
    <td><code>serviceName</code></td>
    <td>Replace <code><em>&lt;myservice1&gt;</em></code> with the name of the Kubernetes service that you created for your Istio-managed app. Separate multiple services with a semi-colon (;). This field is optional. If you do not specify a service name, then all Istio-managed services are enabled for traffic routing.</td>
    </tr>
    <tr>
    <td><code>istioServiceNamespace</code></td>
    <td>Replace <code><em>&lt;istio-namespace&gt;</em></code> with the Kubernetes namespace where Istio is installed. This field is optional. If you do not specify a namespace, then the <code>istio-system</code> namespace is used.</td>
    </tr>
    <tr>
    <td><code>istioServiceName</code></td>
    <td>Replace <code><em>&lt;istio-ingress-service&gt;</em></code> with the name of the Istio Ingress service. This field is optional. If you do not specify the Istio Ingress service name, then service name <code>istio-ingress</code> is used.</td>
    </tr>
    <tr>
    <td><code>path</code></td>
      <td>For each Istio-managed service that you want to route traffic to, replace <code><em>&lt;/myapp1&gt;</em></code> with the backend path that the Istio-managed service listens on. The path must correspond to the path that you defined in the Istio Ingress resource.</td>
    </tr>
    <tr>
    <td><code>servicePort</code></td>
    <td>For each Istio-managed service that you want to route traffic to, replace <code><em>&lt;istio_ingress_port&gt;</em></code> with port of the Istio Ingress controller.</td>
    </tr>
    </tbody></table>
    </dd>
    </dl>

## Proxy buffer annotations
{: #proxy-buffer}


### Client response data buffering (proxy-buffering)
{: #proxy-buffering}

Use the buffer annotation to disable the storage of response data on the ALB while the data is sent to the client.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>The Ingress ALB acts as a proxy between your back-end app and the client web browser. When a response is sent from the back-end app to the client, the response data is buffered on the ALB by default. The ALB proxies the client response and starts sending the response to the client at the client's pace. After all data from the back-end app is received by the ALB, the connection to the back-end app is closed. The connection from the ALB to the client remains open until the client receives all data.

</br></br>
If buffering of response data on the ALB is disabled, data is immediately sent from the ALB to the client. The client must be able to handle incoming data at the pace of the ALB. If the client is too slow, data might get lost.
</br></br>
Response data buffering on the ALB is enabled by default.</dd>
<dt>Sample Ingress resource YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/proxy-buffering: "False"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080</code></pre>
</dd></dl>

<br />



### Proxy buffers (proxy-buffers)
{: #proxy-buffers}

Configure the number and size of proxy buffers for the ALB.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>
Set the number and size of the buffers that read a response for a single connection from the proxied server. The configuration is applied to all of the services in the Ingress host unless a service is specified. For example, if a configuration such as <code>serviceName=SERVICE number=2 size=1k</code> is specified, 1k is applied to the service. If a configuration such as <code>number=2 size=1k</code> is specified, 1k is applied to all of the services in the Ingress host.
</dd>
<dt>Sample Ingress resource YAML</dt>
<dd>
<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: proxy-ingress
 annotations:
   ingress.bluemix.net/proxy-buffers: "serviceName=&lt;myservice&gt; number=&lt;number_of_buffers&gt; size=&lt;size&gt;"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080</code></pre>

<table>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
 </thead>
 <tbody>
 <tr>
 <td><code>serviceName</code></td>
 <td>Replace <code>&lt;<em>myservice</em>&gt;</code> with the name for a service to apply proxy-buffers.</td>
 </tr>
 <tr>
 <td><code>number_of_buffers</code></td>
 <td>Replace <code>&lt;<em>number_of_buffers</em>&gt;</code> with a number, such as <em>2</em>.</td>
 </tr>
 <tr>
 <td><code>size</code></td>
 <td>Replace <code>&lt;<em>size</em>&gt;</code> with the size of each buffer in kilobytes (k or K), such as <em>1K</em>.</td>
 </tr>
 </tbody>
 </table>
 </dd></dl>

<br />


### Proxy buffer size (proxy-buffer-size)
{: #proxy-buffer-size}

Configure the size of the proxy buffer that reads the first part of the response.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>
Set the size of the buffer that reads the first part of the response that is received from the proxied server. This part of the response usually contains a small response header. The configuration is applied to all of the services in the Ingress host unless a service is specified. For example, if a configuration such as <code>serviceName=SERVICE size=1k</code> is specified, 1k is applied to the service. If a configuration such as <code>size=1k</code> is specified, 1k is applied to all of the services in the Ingress host.
</dd>


<dt>Sample Ingress resource YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: proxy-ingress
 annotations:
   ingress.bluemix.net/proxy-buffer-size: "serviceName=&lt;myservice&gt; size=&lt;size&gt;"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
</code></pre>

<table>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
 </thead>
 <tbody>
 <tr>
 <td><code>serviceName</code></td>
 <td>Replace <code>&lt;<em>myservice</em>&gt;</code> with the name of a service to apply proxy-buffers-size.</td>
 </tr>
 <tr>
 <td><code>size</code></td>
 <td>Replace <code>&lt;<em>size</em>&gt;</code> with the size of each buffer in kilobytes (k or K), such as <em>1K</em>.</td>
 </tr>
 </tbody></table>

 </dd>
 </dl>

<br />



### Proxy busy buffers size (proxy-busy-buffers-size)
{: #proxy-busy-buffers-size}

Configure the size of proxy buffers that can be busy.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>
Limit the size of any buffers that are sending a response to the client while the response is not yet fully read. In the meantime, the rest of the buffers can read the response and, if needed, buffer part of the response to a temporary file. The configuration is applied to all of the services in the Ingress host unless a service is specified. For example, if a configuration such as <code>serviceName=SERVICE size=1k</code> is specified, 1k is applied to the service. If a configuration such as <code>size=1k</code> is specified, 1k is applied to all of the services in the Ingress host.
</dd>


<dt>Sample Ingress resource YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: proxy-ingress
 annotations:
   ingress.bluemix.net/proxy-busy-buffers-size: "serviceName=&lt;serviceName&gt; size=&lt;size&gt;"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
         </code></pre>

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Replace <code>&lt;<em>myservice</em>&gt;</code> with the name of a service to apply proxy-busy-buffers-size.</td>
</tr>
<tr>
<td><code>size</code></td>
<td>Replace <code>&lt;<em>size</em>&gt;</code> with the size of each buffer in kilobytes (k or K), such as <em>1K</em>.</td>
</tr>
</tbody></table>

 </dd>
 </dl>

<br />



## Request and response annotations
{: #request-response}


### Additional client request or response header (proxy-add-headers, response-add-headers)
{: #proxy-add-headers}

Add extra header information to a client request before sending the request to the back-end app or to a client response before sending the response to the client.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>The Ingress ALB acts as a proxy between the client app and your back-end app. Client requests that are sent to the ALB are processed (proxied) and put into a new request that is then sent to your back-end app. Similarly, backend app responses that are sent to the ALB are processed (proxied) and put into a new response that is then sent to the client. Proxying a request or response removes HTTP header information, such as the user name, that was initially sent from the client or back-end app.

<br><br>
If your back-end app requires HTTP header information, you can use the <code>proxy-add-headers</code> annotation to add header information to the client request before the request is forwarded by the ALB to the back-end app.

<br>
<ul><li>For example, you might need to add the following X-Forward header information to the request before it is forwarded to your app:

<pre class="screen">
<code>proxy_set_header Host $host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-Proto $scheme;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;</code></pre>

</li>

<li>To add the X-Forward header information to the request sent to your app, use the `proxy-add-headers` annotation in the following way:

<pre class="screen">
<code>ingress.bluemix.net/proxy-add-headers: |
  serviceName=<myservice1> {
  Host $host;
  X-Real-IP $remote_addr;
  X-Forwarded-Proto $scheme;
  X-Forwarded-For $proxy_add_x_forwarded_for;
  }</code></pre>

</li></ul><br>

If the client web app requires HTTP header information, you can use the <code>response-add-headers</code> annotation to add header information to the response before the response is forwarded by the ALB to the client web app.</dd>

<dt>Sample Ingress resource YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/proxy-add-headers: |
      serviceName=&lt;myservice1&gt; {
      &lt;header1&gt; &lt;value1&gt;;
      &lt;header2&gt; &lt;value2&gt;;
      }
      serviceName=&lt;myservice2&gt; {
      &lt;header3&gt; &lt;value3&gt;;
      }
    ingress.bluemix.net/response-add-headers: |
      serviceName=&lt;myservice1&gt; {
      &lt;header1&gt;: &lt;value1&gt;;
      &lt;header2&gt;: &lt;value2&gt;;
      }
      serviceName=&lt;myservice2&gt; {
      &lt;header3&gt;: &lt;value3&gt;;
      }
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: &lt;myservice1&gt;
          servicePort: 8080
      - path: /myapp
        backend:
          serviceName: &lt;myservice2&gt;
          servicePort: 80</code></pre>

 <table>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
  </thead>
  <tbody>
  <tr>
  <td><code>service_name</code></td>
  <td>Replace <code>&lt;<em>myservice</em>&gt;</code> with the name of the Kubernetes service that you created for your app.</td>
  </tr>
  <tr>
  <td><code>&lt;header&gt;</code></td>
  <td>The key of the header information to add to the client request or client response.</td>
  </tr>
  <tr>
  <td><code>&lt;value&gt;</code></td>
  <td>The value of the header information to add to the client request or client response.</td>
  </tr>
  </tbody></table>
 </dd></dl>

<br />



### Client response header removal (response-remove-headers)
{: #response-remove-headers}

Remove header information that is included in the client response from the back-end end app before the response is sent to the client.
 {:shortdesc}

 <dl>
 <dt>Description</dt>
 <dd>The Ingress ALB acts as a proxy between your back-end app and the client web browser. Client responses from the back-end app that are sent to the ALB are processed (proxied), and put into a new response that is then sent from the ALB to the client web browser. Although proxying a response removes http header information that was initially sent from the back-end app, this process might not remove all back-end app specific headers. Remove header information from a client reponse before the response is forwarded from the ALB to the client web browser.</dd>
 <dt>Sample Infress resource YAML</dt>
 <dd>
 <pre class="codeblock">
 <code>apiVersion: extensions/v1beta1
 kind: Ingress
 metadata:
   name: myingress
   annotations:
     ingress.bluemix.net/response-remove-headers: |
       serviceName=&lt;myservice1&gt; {
       "&lt;header1&gt;";
       "&lt;header2&gt;";
       }
       serviceName=&lt;myservice2&gt; {
       "&lt;header3&gt;";
       }
 spec:
   tls:
   - hosts:
     - mydomain
     secretName: mytlssecret
   rules:
   - host: mydomain
     http:
       paths:
       - path: /
         backend:
           serviceName: &lt;myservice1&gt;
           servicePort: 8080
       - path: /myapp
         backend:
           serviceName: &lt;myservice2&gt;
           servicePort: 80</code></pre>

  <table>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
   </thead>
   <tbody>
   <tr>
   <td><code>service_name</code></td>
   <td>Replace <code>&lt;<em>myservice</em>&gt;</code> with the name of the Kubernetes service that you created for your app.</td>
   </tr>
   <tr>
   <td><code>&lt;header&gt;</code></td>
   <td>The key of the header to remove from the client response.</td>
   </tr>
   </tbody></table>
   </dd></dl>

<br />


### Client request body size (client-max-body-size)
{: #client-max-body-size}

Set the maximum size of the body that the client can send as part of a request.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>To maintain the expected performance, the maximum client request body size is set to 1 megabyte. When a client request with a body size over the limit is sent to the Ingress ALB, and the client does not allow data to be divided, the ALB returns a 413 (Request Entity Too Large) HTTP response to the client. A connection between the client and the ALB is not possible until the size of the request body is reduced. When the client allows data to be split up into multiple chunks, data is divided into packages of 1 megabyte and sent to the ALB.

</br></br>
You might want to increase the maximum body size because you expect client requests with a body size that is greater than 1 megabyte. For example, you want your client to be able to upload large files. Increasing the maximum request body size might impact the performance of your ALB because the connection to the client must stay open until the request is received.
</br></br>
<strong>Note:</strong> Some client web browsers cannot display the 413 HTTP response message properly.</dd>
<dt>Sample Ingress resource YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/client-max-body-size: "size=&lt;size&gt;"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080</code></pre>

<table>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
 </thead>
 <tbody>
 <tr>
 <td><code>&lt;size&gt;</code></td>
 <td>The maximum size of the client response body. For example, to set it to 200 megabyte, define <code>200m</code>.  <strong>Note:</strong> You can set the size to 0 to disable the check of the client request body size.</td>
 </tr>
 </tbody></table>

 </dd></dl>

<br />


### Large client header buffers (large-client-header-buffers)
{: #large-client-header-buffers}

Set the maximum number and size of buffers that read large client request headers.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>Buffers that read large client request headers are allocated only by demand: If a connection is transitioned into the keepalive state after the end-of-request processing, these buffers are released. By default, the buffer size is equal to <code>8K</code> bytes. If a request line exceeds the set maximum size of one buffer, the <code>414 Request-URI Too Large</code> error is returned to the client. Additionally, if a request header field exceeds the set maximum size of one buffer, the <code>400 Bad Request</code> error is returned to the client. You can adjust the maximum number and size of buffers that are used for reading large client request headers.

<dt>Sample Ingress resource YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/large-client-header-buffers: "number=&lt;number&gt; size=&lt;size&gt;"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080</code></pre>

<table>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
 </thead>
 <tbody>
 <tr>
 <td><code>&lt;number&gt;</code></td>
 <td>The maximum number of buffers that should be allocated to read large client request header. For example, to set it to 4, define <code>4</code>.</td>
 </tr>
 <tr>
 <td><code>&lt;size&gt;</code></td>
 <td>The maximum size of buffers that read large client request header. For example, to set it to 16 kilobytes, define <code>16k</code>.
   <strong>Note:</strong> The size must end with a <code>k</code> for kilobyte or <code>m</code> for megabyte.</td>
 </tr>
</tbody></table>
</dd>
</dl>

<br />


## Service limit annotations
{: #service-limit}


### Global rate limits (global-rate-limit)
{: #global-rate-limit}

Limit the request processing rate and number of connections per a defined key for all services.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>
For all services, limit the request processing rate and the number of connections per a defined key that are coming from a single IP address for all paths of the selected backends.
</dd>


 <dt>Sample Ingress resource YAML</dt>
 <dd>

 <pre class="codeblock">
 <code>apiVersion: extensions/v1beta1
 kind: Ingress
 metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/global-rate-limit: "key=&lt;key&gt; rate=&lt;rate&gt; conn=&lt;number_of_connections&gt;"
 spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

 <table>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
  </thead>
  <tbody>
  <tr>
  <td><code>key</code></td>
  <td>To set a global limit for incoming requests based on the location or service, use `key=location`. To set a global limit for incoming requests based on the header, use `X-USER-ID key==$http_x_user_id`.</td>
  </tr>
  <tr>
  <td><code>rate</code></td>
  <td>Replace <code>&lt;<em>rate</em>&gt;</code> with the processing rate. Enter a value as a rate per second (r/s) or rate per minute (r/m). Example: <code>50r/m</code>.</td>
  </tr>
  <tr>
  <td><code>number-of_connections</code></td>
  <td>Replace <code>&lt;<em>conn</em>&gt;</code> with the number of connections.</td>
  </tr>
  </tbody></table>

  </dd>
  </dl>

<br />



### Service rate limits (service-rate-limit)
{: #service-rate-limit}

Limit the request processing rate and the number of connections for specific services.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>For specific services, limit the request processing rate and the number of connections per a defined key that are coming from a single IP address for all paths of the selected backends.
</dd>


 <dt>Sample Ingress resource YAML</dt>
 <dd>

 <pre class="codeblock">
 <code>apiVersion: extensions/v1beta1
 kind: Ingress
 metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/service-rate-limit: "serviceName=&lt;myservice&gt; key=&lt;key&gt; rate=&lt;rate&gt; conn=&lt;number_of_connections&gt;"
 spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

 <table>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
  </thead>
  <tbody>
  <tr>
  <td><code>serviceName</code></td>
  <td>Replace <code>&lt;<em>myservice</em>&gt;</code> with the name of the service for which you want to limit the processing rate.</li>
  </tr>
  <tr>
  <td><code>key</code></td>
  <td>To set a global limit for incoming requests based on the location or service, use `key=location`. To set a global limit for incoming requests based on the header, use `X-USER-ID key==$http_x_user_id`.</td>
  </tr>
  <tr>
  <td><code>rate</code></td>
  <td>Replace <code>&lt;<em>rate</em>&gt;</code> with the processing rate. To define a rate per second, use r/s: <code>10r/s</code>. To define a rate per minute, use r/m: <code>50r/m</code>.</td>
  </tr>
  <tr>
  <td><code>number-of_connections</code></td>
  <td>Replace <code>&lt;<em>conn</em>&gt;</code> with the number of connections.</td>
  </tr>
  </tbody></table>
  </dd>
  </dl>

  <br />



