This provides an nginx configuration that will allow for the *local* decryption of TLS 1.0, TLS 1.1 or SSLv3 traffic and forward that traffic to the beta and production FPDS web servers with TLS 1.2 connections.

Note: these will not be able to be used in your environment as-is. You will need to set up the certificates appropriately, load balance as necessary, and set up whatever configuration items may be affected by your own firewalls, DNS, etc. However, this provides a baseline for building a server that will forward FPDS requests to the updated servers. Please look at the comments in the configuration file.

The [nginx-fpds-beta.conf](nginx-fpds-beta.conf) file proxies requests to the FPDS Beta server. The [nginx-fpds-prod.conf](nginx-fpds-prod.conf) file proxies requests to the FPDS production server. 

To test, update the configuration and launch with `nginx -c <file>`. Then use cURL or wget to call the server. The proxy will send the entire path and query paramters as-is to the FPDS server. If you use self-signed certificates, you may need to provide the public key manually to cURL or wget (e.g., by using the `--cacert` option to cURL). For cURL, the following will forward a query on the ATOM feed. Note that the request forces a TLS 1.0 request to the server. Replace `<proxy_host>` with the location you deploy your proxy.

### Retrieving the FPDS Front Page

```
	curl --tlsv1.0 "https://<proxy_host>/fpdsng_cms/index.php?pageSource=loginPage"
```

### ATOM Feed Call

```
	curl --tlsv1.0 "https://<proxy_host>/ezsearch/FEEDS/ATOM?FEEDNAME=PUBLIC&q=company"
```

Web service calls using `POST` work as expected as well.
