# Generating Secrets #

This section goes over how to generate secrets for use in Kubernetes for TLS or
Mutual Authentication.

## Generating Certificates ##

There are several ways to create certificates. In this guide we will use
`openssl`.

When generating the certificates make sure the CN is different for each certificate.
The value for the [CN](https://support.dnsimple.com/articles/what-is-common-name/) can
be entered while the certificate is being generated.

```
# Generate the CA Cert, and Key
$ openssl genrsa -out ca.key 4096
$ openssl req -new -x509 -days 365 -key ca.key -out ca.crt

# Generate the Server Key, and CSR
# The CN should be your Ingress-Subdomain example: <Cluster-Name>.us-south.containers.mybluemix.net
$ openssl genrsa -out server.key 1024
$ openssl req -new -key server.key -out server.csr

# Generate the Server Cert, Signing it with the CA
$ openssl x509 -req -days 365 -in server.csr -CA ca.crt -CAkey ca.key -set_serial 01 -out server.crt

# Generate the Client Key and CSR
$ openssl genrsa -out client.key 1024
$ openssl req -new -key client.key -out client.csr

# Generate the Client Cert, Signing it with the CA
$ openssl x509 -req -days 365 -in client.csr -CA ca.crt -CAkey ca.key -set_serial 01 -out client.crt
```

*Warning: In production you should obtain a certificate from a trusted source(Let's Encrypt, Digicert, etc.)*

The Client Cert and Client Key must be able to be verified up to the trusted root certificate
which in this case is the CA cert. An example of a valid certificate chain would be:

Client Certificate: issued by Intermediate Certificate
Intermediate Certificate: issued by Root Certificate
Root Certificate: issued by itself

## Adding TLS Certificate and Key as a Kubernetes Secret ##

In order to create a secret to be used for a TLS connection we require the
`server.crt` and `server.key` generated above.

`$ kubectl create secret generic <secretName> --from-file=tls.crt=server.crt --from-file=tls.key=server.key`

Now we have a secret that can be used for TLS.

## Adding a Mutual Authentication CA as Kubernetes Secret ##

In order to create a secret that can be used for mutual authentication we require the 
`ca.crt` generated above.

`$ kubectl create secret generic <secretName> --from-file=ca.crt=ca.crt`

Now we have a secret that can be used for mutual authentication.

## Adding a TLS Certificate, TLS Key, Mutual Authentication CA as Kubernetes Secret ##

This will allow you to create a secret that can be used for both a TLS connection as well as
the mutual authentication annotation.

`$ kubectl create secret generic <secretName> --from-file=tls.crt=server.crt --from-file=tls.key=server.key --from-file=ca.crt=ca.crt`

Now we have a secret that can be used for TLS and mutual authentication.