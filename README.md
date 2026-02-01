# helm-grafana-operator

XRD that installs the Grafana Operator. Use this when you want Grafana custom resources (Grafana, Dashboard, Datasource) managed by the operator instead of raw Helm releases.

## Why Grafana Operator?

**Without Grafana Operator:**
- Grafana instances are managed ad hoc per namespace
- Dashboards and datasources require manual provisioning
- Multi-team Grafana becomes inconsistent

**With Grafana Operator:**
- Declarative Grafana instances and dashboards
- Consistent datasource management across clusters
- Fits GitOps workflows cleanly

## The Journey

### Stage 1: Getting Started (Individual)

Install the operator with defaults.

```yaml
apiVersion: helm.hops.ops.com.ai/v1alpha1
kind: GrafanaOperator
metadata:
  name: grafana-operator
  namespace: example-env
spec:
  clusterName: example-cluster
```

### Stage 2: Growing (Small Org)

Tune resources and enable namespaces you want the operator to watch.

```yaml
apiVersion: helm.hops.ops.com.ai/v1alpha1
kind: GrafanaOperator
metadata:
  name: grafana-operator
  namespace: example-env
spec:
  clusterName: example-cluster
  values:
    resources:
      requests:
        cpu: 50m
        memory: 128Mi
```

### Stage 3: Enterprise Scale

Harden the operator by setting explicit resource limits and policies.

```yaml
apiVersion: helm.hops.ops.com.ai/v1alpha1
kind: GrafanaOperator
metadata:
  name: grafana-operator
  namespace: example-env
spec:
  clusterName: example-cluster
  values:
    resources:
      limits:
        cpu: 200m
        memory: 256Mi
```

### Stage 4: Import Existing (Optional)

This configuration does not currently support importing existing Helm releases. If you need adoption support,
we can add external-name handling after validating provider-helm import behavior.

## Using GrafanaOperator

Define Grafana custom resources (Grafana, GrafanaDashboard, GrafanaDatasource) in namespaces watched by the operator.

## Status

- `status.ready`: overall readiness
- `status.release`: name/namespace/ready for the Helm release

## Composed Resources

- `helm.m.crossplane.io/v1beta1 Release` for grafana-operator

## Development

```bash
make render
make validate
make test
```
