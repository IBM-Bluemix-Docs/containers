---

copyright:
  years: 2014, 2018
lastupdated: "2018-01-16"

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

To add capabilities to your application load balancer, you can specify annotations as metadata in an Ingress resource.
{: shortdesc}

For general information about Ingress services and how to get started using them, see [Configuring public access to an app by using Ingress](cs_ingress.html#config).

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
 <td><a href="#alb-id">Private application load balancer routing</a></td>
 <td><code>ALB-ID</code></td>
 <td>Route incoming requests to your apps with a private application load balancer.</td>
 </tr>
 <tr>
 <td><a href="#rewrite-path">Rewrite paths</a></td>
 <td><code>rewrite-path</code></td>
 <td>Route incoming network traffic to a different path that your back-end app listens on.</td>
 </tr>
 <tr>
 <td><a href="#sticky-cookie-services">Session-affinity with cookies</a></td>
 <td><code>sticky-cookie-services</code></td>
 <td>Always route incoming network traffic to the same upstream server by using a sticky cookie.</td>
 </tr>
 <tr>
 <td><a href="#tcp-ports">TCP ports</a></td>
 <td><code>tcp-ports</code></td>
 <td>Access an app via a non-standard TCP port.</td>
 </tr>
 </tbody></table>


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
  <td><code>proxy-connect-timeout</code></td>
  <td>Adjust the time that the application load balancer waits to connect to and read from the back-end app before the back-end app is considered unavailable.</td>
  </tr>
  <tr>
  <td><a href="#keepalive-requests">Keepalive requests</a></td>
  <td><code>keepalive-requests</code></td>
  <td>Configure the maximum number of requests that can be served through one keepalive connection.</td>
  </tr>
  <tr>
  <td><a href="#keepalive-timeout">Keepalive timeout</a></td>
  <td><code>keepalive-timeout</code></td>
  <td>Configure the time that a keepalive connection stays open on the server.</td>
  </tr>
  <tr>
  <td><a href="#upstream-keepalive">Upstream keepalive</a></td>
  <td><code>upstream-keepalive</code></td>
  <td>Configure the maximum number of idle keepalive connections for an upstream server.</td>
  </tr>
  </tbody></table>


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
 <td>Disable the buffering of a client response on the application load balancer while sending the response to the client.</td>
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
<td><code>proxy-add-headers</code></td>
<td>Add header information to a client request before forwarding the request to your back-end app or to a client response before sending the response to the client.</td>
</tr>
<tr>
<td><a href="#response-remove-headers">Client response header removal</a></td>
<td><code>response-remove-headers</code></td>
<td>Remove header information from a client response before forwarding the response to the client.</td>
</tr>
<tr>
<td><a href="#client-max-body-size">Custom maximum client request body size</a></td>
<td><code>client-max-body-size</code></td>
<td>Adjust the size of the client request body that is allowed to be sent to the application load balancer.</td>
</tr>
</tbody></table>

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
<td><a href="#mutual-auth">Mutual authentication</a></td>
<td><code>mutual-auth</code></td>
<td>Configure mutual authentication for the application load balancer.</td>
</tr>
<tr>
<td><a href="#ssl-services">SSL services support</a></td>
<td><code>ssl-services</code></td>
<td>Allow SSL services support for load balancing.</td>
</tr>
</tbody></table>



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



### Private application load balancer routing (ALB-ID)
{: #alb-id}

Route incoming requests to your apps with a private application load balancer.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>
Choose a private application load balancer to route incoming requests with instead of the public application load balancer.</dd>


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
<td>The ID for your private application load balancer. Run <code>bx cs albs --cluster <my_cluster></code> to find the private application load balancer ID.
</td>
</tr>
</tbody></table>
</dd>
</dl>

<br />



### Rewrite paths (rewrite-path)
{: #rewrite-path}

Route incoming network traffic on an application load balancer domain path to a different path that your back-end application listens on.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>Your Ingress application load balancer domain routes incoming network traffic on <code>mykubecluster.us-south.containers.mybluemix.net/beans</code> to your app. Your app listens on <code>/coffee</code>, instead of <code>/beans</code>. To forward incoming network traffic to your app, add the rewrite annotation to your Ingress resource configuration file. The rewrite annotation ensures that incoming network traffic on <code>/beans</code> is forwarded to your app by using the <code>/coffee</code> path. When including multiple services, use only a semi-colon (;) to separate them.</dd>
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
<td>Replace <code>&lt;<em>target_path</em>&gt;</code> with the path that your app listens on. Incoming network traffic on the application load balancer domain is forwarded to the Kubernetes service by using this path. Most apps do not listen on a specific path, but use the root path and a specific port. In the example above, the rewrite path was defined as <code>/coffee</code>.</td>
</tr>
</tbody></table>

</dd></dl>

<br />


### Session-affinity with cookies (sticky-cookie-services)
{: #sticky-cookie-services}

Use the sticky cookie annotation to add session affinity to your application load balancer and always route incoming network traffic to the same upstream server.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>For high availability, some app setups require you to deploy multiple upstream servers that handle incoming client requests. When a client connects to you back-end app, you can use session-affinity so that a client is served by the same upstream server for the duration of a session or for the time it takes to complete a task. You can configure your application load balancer to ensure session-affinity by always routing incoming network traffic to the same upstream server.

</br></br>
Every client that connects to your back-end app is assigned to one of the available upstream servers by the application load balancer. The application load balancer creates a session cookie that is stored in the client's app, which is included in the header information of every request between the application load balancer and the client. The information in the cookie ensures that all requests are handled by the same upstream server throughout the session.

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
  <td>Replace <code>&lt;<em>expiration_time</em>&gt;</code> with the time in seconds (s), minutes (m), or hours (h) before the sticky cookie expires. This time is independent of the user activity. After the cookie is expired, the cookie is deleted by the client web browser and no longer sent to the application load balancer. For example, to set an expiration time of 1 second, 1 minute, or 1 hour, enter <code>1s</code>, <code>1m</code>, or <code>1h</code>.</td>
  </tr>
  <tr>
  <td><code>path</code></td>
  <td>Replace <code>&lt;<em>cookie_path</em>&gt;</code> with the path that is appended to the Ingress subdomain and that indicates for which domains and subdomains the cookie is sent to the application load balancer. For example, if your Ingress domain is <code>www.myingress.com</code> and you want to send the cookie in every client request, you must set <code>path=/</code>. If you want to send the cookie only for <code>www.myingress.com/myapp</code> and all its subdomains, then you must set <code>path=/myapp</code>.</td>
  </tr>
  <tr>
  <td><code>hash</code></td>
  <td>Replace <code>&lt;<em>hash_algorithm</em>&gt;</code> with the hash algorithm that protects the information in the cookie. Only <code>sha1</code> is supported. SHA1 creates a hash sum based on the information in the cookie and appends this hash sum to the cookie. The server can decrypt the information in the cookie and verify data integrity.</td>
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

<p>**Note**: The application load balancer operates in pass-through mode and forwards traffic to backend apps. SSL termination is not supported in this case.</p>
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
  <td>Replace <code>&lt;<em>service_port</em>&gt;</code> with this parameter is optional. When provided, the port is substituted to this value before traffic is sent to the backend app. Otherwise, the port remains same as the Ingress port.</td>
  </tr>
  </tbody></table>
  </dd>
  </dl>

<br />



## Connection annotations
{: #connection}

### Custom connect-timeouts and read-timeouts (proxy-connect-timeout, proxy-read-timeout)
{: #proxy-connect-timeout}

Set a custom connect-timeout and read-timeout for the application load balancer. Adjust the time that the application load balancer waits to connect to and read from the back-end app before the back-end app is considered unavailable.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>When a client request is sent to the Ingress application load balancer, a connection to the back-end app is opened by the application load balancer. By default, the application load balancer waits 60 seconds to receive a reply from the back-end app. If the back-end app does not reply within 60 seconds, then the connection request is aborted and the back-end app is considered to be unavailable.

</br></br>
After the application load balancer is connected to the back-end app, response data is read from the back-end app by the application load balancer. During this read operation, the application load balancer waits a maximum of 60 seconds between two read operations to receive data from the back-end app. If the back-end app does not send data within 60 seconds, the connection to the back-end app is closed and the app is considered to be not available.
</br></br>
A 60 second connect-timeout and read-timeout is the default timeout on a proxy and usually should not be changed.
</br></br>
If the availability of your app is not steady or your app is slow to respond because of high workloads, you might want to increase the connect-timeout or read-timeout. Keep in mind that increasing the timeout impacts the performance of the application load balancer as the connection to the back-end app must stay open until the timeout is reached.
</br></br>
On the other hand, you can decrease the timeout to gain performance on the application load balancer. Ensure that your back-end app is able to handle requests within the specified timeout, even during higher workloads.</dd>
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
 <td>The number of seconds to wait before the back-end app is read, for example <code>65s</code>.</td>
 </tr>
 </tbody></table>

 </dd></dl>

<br />



### Keepalive requests (keepalive-requests)
{: #keepalive-requests}

Configure the maximum number of requests that can be served through one keepalive connection.
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

Configure the time that a keepalive connection stays open on the server side.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>
Sets the time that a keepalive connection stays open on the server.
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



### Upstream keepalive (upstream-keepalive)
{: #upstream-keepalive}

Configure the maximum number of idle keepalive connections for an upstream server.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>
Change the maximum number of idle keepalive connections to the upstream server of a given service. The upstream server has 64 idle keepalive connections by default.
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



## Proxy buffer annotations
{: #proxy-buffer}


### Client response data buffering (proxy-buffering)
{: #proxy-buffering}

Use the buffer annotation to disable the storage of response data on the application load balancer while the data is sent to the client.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>The Ingress application load balancer acts as a proxy between your back-end app and the client web browser. When a response is sent from the back-end app to the client, the response data is buffered on the application load balancer by default. The application load balancer proxies the client response and starts sending the response to the client at the client's pace. After all data from the back-end app is received by the application load balancer, the connection to the back-end app is closed. The connection from the application load balancer to the client remains open until the client receives all data.

</br></br>
If buffering of response data on the application load balancer is disabled, data is immediately sent from the application load balancer to the client. The client must be able to handle incoming data at the pace of the application load balancer. If the client is too slow, data might get lost.
</br></br>
Response data buffering on the application load balancer is enabled by default.</dd>
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

Configure the number and size of proxy buffers for the application load balancer.
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


### Additional client request or response header (proxy-add-headers)
{: #proxy-add-headers}

Add extra header information to a client request before sending the request to the back-end app or to a client response before sending the response to the client.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>The Ingress application load balancer acts as a proxy between the client app and your back-end app. Client requests that are sent to the application load balancer are processed (proxied) and put into a new request that is then sent from the application load balancer to your back-end app. Proxying a request removes http header information, such as the user name, that was initially sent from the client. If your back-end app requires this information, you can use the <strong>ingress.bluemix.net/proxy-add-headers</strong> annotation to add header information to the client request before the request is forwarded from the application load balancer to your back-end app.

</br></br>
When a back-end app sends a response to the client, the response is proxied by the application load balancer and http headers are removed from the response. The client web app might require this header information to successfully process the response. You can use the <strong>ingress.bluemix.net/response-add-headers</strong> annotation to add header information to the client response before the response is forwarded from the application load balancer to client web app.</dd>
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
      "&lt;header1&gt;: &lt;value1&gt;";
      "&lt;header2&gt;: &lt;value2&gt;";
      }
      serviceName=&lt;myservice2&gt; {
      "&lt;header3&gt;: &lt;value3&gt;";
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
  <td>The key of the header information to add to the client request or client response.</td
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
 <dd>The Ingress application load balancer acts as a proxy between your back-end app and the client web browser. Client responses from the back-end app that are sent to the application load balancer are processed (proxied), and put into a new response that is then sent from the application load balancer to the client web browser. Although proxying a response removes http header information that was initially sent from the back-end app, this process might not remove all back-end app specific headers. Remove header information from a client reponse before the response is forwarded from the application load balancer to the client web browser.</dd>
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


### Custom maximum client request body size (client-max-body-size)
{: #client-max-body-size}

Adjust the maximum size of the body that the client can send as part of a request.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>To maintain the expected performance, the maximum client request body size is set to 1 megabyte. When a client request with a body size over the limit is sent to the Ingress application load balancer, and the client does not allow data to be divided, the application load balancer returns a 413 (Request Entity Too Large) HTTP response to the client. A connection between the client and the application load balancer is not possible until the size of the request body is reduced. When the client allows data to be split up into multiple chunks, data is divided into packages of 1 megabyte and sent to the application load balancer.

</br></br>
You might want to increase the maximum body size because you expect client requests with a body size that is greater than 1 megabyte. For example, you want your client to be able to upload large files. Increasing the maximum request body size might impact the performance of your application load balancer because the connection to the client must stay open until the request is received.
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



## HTTPS and TLS/SSL authentication annotations
{: #https-auth}


### Custom HTTP and HTTPS ports (custom-port)
{: #custom-port}

Change the default ports for HTTP (port 80) and HTTPS (port 443) network traffic.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>By default, the Ingress application load balancer is configured to listen for incoming HTTP network traffic on port 80 and for incoming HTTPS network traffic on port 443. You can change the default ports to add security to your application load balancer domain, or to enable only an HTTPS port.
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
 <td>Enter <strong>http</strong> or <strong>https</strong> to change the default port for incoming HTTP or HTTPS network traffic.</td>
 </tr>
 <tr>
 <td><code>&lt;port&gt;</code></td>
 <td>Enter the port number that you want to use for incoming HTTP or HTTPS network traffic.  <p><strong>Note:</strong> When a custom port is specified for either HTTP or HTTPS, the default ports are no longer valid for both HTTP and HTTPS. For example, to change the default port for HTTPS to 8443, but use the default port for HTTP, you must set custom ports for both: <code>custom-port: "protocol=http port=80; protocol=https port=8443"</code>.</p></td>
 </tr>
 </tbody></table>

 </dd>
 <dt>Usage</dt>
 <dd><ol><li>Review open ports for your application load balancer. 
<pre class="pre">
<code>kubectl get service -n kube-system</code></pre>
Your CLI output looks similar to the following:
<pre class="screen">
<code>NAME                     CLUSTER-IP     EXTERNAL-IP     PORT(S)                      AGE
public-ingress-ctl-svc   10.10.10.149   169.60.16.246   80:30776/TCP,443:30412/TCP   8d</code></pre></li>
<li>Open the Ingress controller config map.
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
 <li>Verify that your Ingress controller is re-configured with the non-default ports.
<pre class="pre">
<code>kubectl get service -n kube-system</code></pre>
Your CLI output looks similar to the following:
<pre class="screen">
<code>NAME                     CLUSTER-IP     EXTERNAL-IP     PORT(S)                      AGE
public-ingress-ctl-svc   10.10.10.149   169.60.16.246   &lt;port1&gt;:30776/TCP,&lt;port2&gt;:30412/TCP   8d</code></pre></li>
<li>Configure your Ingress to use the non-default ports when routing incoming network traffic to your services. Use the sample YAML file in this reference. </li>
<li>Update your Ingress controller configuration.
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
<dd>You set up your Ingress application load balancer to secure your domain with the IBM-provided TLS certificate or your custom TLS certificate. Some users might try to access your apps by using an insecure http request to your application load balancer domain, for example <code>http://www.myingress.com</code>, instead of using <code>https</code>. You can use the redirect annotation to always convert insecure HTTP requests to HTTPS. If you do not use this annotation, insecure HTTP requests are not converted into HTTPS requests by default and might expose unencrypted confidential information to the public.

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



### Mutual authentication (mutual-auth)
{: #mutual-auth}

Configure mutual authentication for the application load balancer.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>
Configure mutual authentication for the Ingress application load balancer. The client authenticates the server and the server also authenticates the client by using certificates. Mutual authentication is also known as certificate-based authentication or two-way authentication.
</dd>

<dt>Pre-requisites</dt>
<dd>
<ul>
<li>[You must have a valid secret that contains the required certificate authority (CA)](cs_app.html#secrets). The <code>client.key</code> and <code>client.crt</code> are also needed to authenticate with mutual authentication.</li>
<li>To enable mutual authentication on a port other than 443, [configure the load balancer to open the valid port](cs_ingress.html#opening_ingress_ports).</li>
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
<td>The application load balancer port number.</td>
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
Encrypt traffic to your upstream apps that require HTTPS with the application load balancers.

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
  <td>Replace <code>&lt;<em>myservice</em>&gt;</code> with the name of the service that represents your app. Traffic is encrypted from application load balancer to this app.</td>
  </tr>
  <tr>
  <td><code>ssl-secret</code></td>
  <td>Replace <code>&lt;<em>service-ssl-secret</em>&gt;</code> with the secret for the service. This parameter is optional. If the parameter is provided, the value must contain the key and the certificate that your app is expecting from the client.</td>
  </tr>
  </tbody></table>

  </dd>
</dl>

<br />


#### SSL Services support with authentication
{: #ssl-services-auth}

Allow HTTPS requests and encrypt traffic to your upstream apps with one-way or mutual authentication for additional security.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>
Configure mutual authentication for load balancing apps that require HTTPS with the Ingress controllers.

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
  <td>Replace <code>&lt;<em>myservice</em>&gt;</code> with the name of the service that represents your app. Traffic is encrypted from application load balancer to this app.</td>
  </tr>
  <tr>
  <td><code>ssl-secret</code></td>
  <td>Replace <code>&lt;<em>service-ssl-secret</em>&gt;</code> with the secret for the service. This parameter is optional. If the parameter is provided, the value must contain the key and the certificate that your app is expecting from the client.</td>
  </tr>
  </tbody></table>

  </dd>


<br />



