# Installation

The Developer Preview is distributed as a Helm chart. It is not installed
through an operator.

## Install from a checkout

    helm upgrade --install training-service ./charts/training-service \
      --namespace training-service \
      --create-namespace

To use a private image or a release tag:

    helm upgrade --install training-service ./charts/training-service \
      --namespace training-service \
      --set image.repository=quay.io/opendatahub/training-service \
      --set image.tag=<tag>

Check the deployment:

    kubectl -n training-service rollout status deployment/training-service
    kubectl -n training-service get service training-service

The chart exposes /healthz and /readyz on port 8080. The service must receive
an OpenShift bearer token with the request and use the caller's existing
identity and project permissions. The chart must not grant broad cluster
permissions or elevate caller access.
