{{#if ingress}}
To get the certificate, run:

```bash
kubectl get secret ingress-cert -n metadata-store -o json | jq -r '.data."ca.crt"' | base64 -d > insight-ca.crt
```
{{else}}
Since you have deployed Supply Chain Security Tools - Store without using Ingress,
you must use the Certificate resource `app-tls-cert` for HTTPS communication.
To get the CA Certificate:

```bash
kubectl get secret app-tls-cert -n metadata-store -o json | jq -r '.data."ca.crt"' | base64 -d > insight-ca.crt
```
{{/if}}

Set the target by running:
{{#if ingress}}
```bash
tanzu insight config set-target https://$METADATA_STORE_DOMAIN --ca-cert insight-ca.crt
```
{{else}}
```bash
tanzu insight config set-target https://$METADATA_STORE_DOMAIN:$METADATA_STORE_PORT --ca-cert insight-ca.crt
```
{{/if}}

> **Important** The `tanzu insight config set-target` will not initiate a test connection.
> Use `tanzu insight health` to test connecting using the configured endpoint and CA certificate.
> Neither commands test whether the access token is correct.
> <!--For that you need to use the plug-in to [add](add-data.hbs.md) and [query](query-data.hbs.md) data.-->
