# Authentification with OAuth2 using Envoy, IdentityServer and Apache Httpd on Minikube

This example shows how to build a simple OAuth2 roundtrip with [Envoy](https://www.envoyproxy.io/) as proxy, [IdentityServer 4](http://docs.identityserver.io/en/latest/) as OpenIDConnect and OAuth2 implementation and [Apache Httpd](https://httpd.apache.org/) as a simple web server containing a hello world app on minikube.

## Prerequisites
* basic understanding of [OAuth2](https://oauth.net/2/)
*  [Minikube](https://kubernetes.io/docs/setup/learning-environment/minikube/)
*  [curl](https://curl.haxx.se/)

## build environment


## OAuth2-Roundtrip

### first request to the service
* in bash call 'minikube ip' to get your minikube-ip (= minikube_ip)
* in bash call 'kubectl get service front-service'; under the port(s) entry take the right port (= entry_port) (this is the entry-port generated by kubernetes)
* If you enter the following command on your bash 'curl -v minikube_ip:entry_port', you will get an error '401 unauthorized'.
  This is because the envoy-proxy cannot validate the request. The client has not a valid JWT-token yet.

### Get the access token
In bash call 'curl -v -X POST -H 'Content-Type: application/x-www-form-urlencoded;charset=UTF-8' -k -d "grant_type=password&username=test&password=test&scope=openid&client_id=local.client&client_secret=secret" 'http://localhost:5000/connect/token'
The response will be a JSON-String containing the access-token.
Copy the access token to your clipboard.

### Use the access token to send second request to service
in bash call 'curl -v -H 'Accept: application/json' -H "Authorization: Bearer **your_accesstoken**" http://localhost:1234''
You will get a response of http '200 ok' with html-title 'Hello World', because now your client is authenticated.