# helm-chart-skel

The Helm chart skeleton I start every new service with.

Not a library, not a framework. It is the chart you copy, delete the parts you
do not need, and ship. The Deployment and Service templates are written to be
read at 03:00 by someone who is tired and needs to find the probe timeout
quickly.

## What is in here

- `Chart.yaml` — metadata. Version it when you change the chart, not just the
  app.
- `values.yaml` — defaults that are sane, not generous. Resource limits are set,
  not commented out. Security context drops all capabilities and runs as
  non-root.
- `templates/deployment.yaml` — one Deployment, one Service. Probes point at
  `/healthz` and `/ready` because that is what they should point at. Change
  them if your service has something better, but have them.

## What is not in here

- No HPA template. Add one when you know your scaling thresholds, not before.
- No PodDisruptionBudget. Add one when you have more than two replicas and
  care about voluntary disruptions.
- No cert-manager, no service mesh, no sidecar injection. Add those when the
  service needs them, and not before.

## Usage

```bash
# Render the chart locally to see what you get
helm template my-release ./helm-chart-skel

# Install it
helm install my-release ./helm-chart-skel \
  --set image.repository=ghcr.io/yourorg/your-service \
  --set image.tag=v1.2.3
```

## Why

I have seen too many charts that try to do everything and end up doing nothing
well. This one does one thing: run a stateless container with probes, limits,
and a Service in front of it. Everything else is your problem, which is honest.

## License

MIT. See [LICENSE](LICENSE).
