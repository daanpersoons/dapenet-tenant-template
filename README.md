# dapenet tenant template

Deploy your app to your own namespace on dapenet from this repo. GitHub Actions logs in
with its own OIDC token; no secrets needed.

## Setup

1. *Use this template* to create your repo, and send the cluster admin the repo name,
   your environments and hostnames.
2. In `k8s/environments/production/kustomization.yaml`, set your `namespace:` and
   hostname. Put your app in `k8s/base/`.
3. Push to `main` (changes under `k8s/**` deploy; or run the workflow manually).

## Your own domain

| Name | Type | Value |
|---|---|---|
| `app.example.com` | CNAME | `customers.dape.pro` |
| `_acme-challenge.app.example.com` | CNAME | *provided by the cluster admin* |

If your domain has CAA records, add `0 issue "ssl.com"`.

## Rules

- Pods must pass Pod Security *restricted* (see `k8s/base/app.yaml`).
- Expose apps with an `HTTPRoute` on Gateway `public` (namespace `gateway`), using your
  assigned hostnames. Serve plain HTTP; Cloudflare handles HTTPS.
- No `Namespace` resources, LoadBalancers or NodePorts.

## kubectl

```bash
brew install kubelogin
KUBECONFIG=kubeconfig-dapenet.yaml kubectl -n <namespace> get pods
```
