# Training Service Helm repository

```bash
helm repo add training-service https://raw.githubusercontent.com/opendatahub-io/training-service/helm-charts/
helm repo update training-service
helm install training-service training-service/training-service \
  --namespace training-service --create-namespace --wait --timeout 15m
```

Prepared OpenShift AI, Ray/Kueue and installer RBAC permissions are required.
The chart supplies its image, Route, runtime discovery and log gateway/CA defaults.
API reference: open `/docs` at the Route URL printed by Helm.

Version 0.1.0 is the developer-preview chart prepared for
https://github.com/opendatahub-io/training-service/pull/14.
Its service image was built from commit 994e168e919e01bfc3d215529a53493cba762cc9.
