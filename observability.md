# Observability at La Suite numérique

Here are a few conventions regarding observability in La Suite numérique applications. The goal is to make applications easier to operate by exposing the information needed to understand their health, load and resource usage.

## Prometheus metrics endpoints

Applications should expose operational metrics through a [Prometheus](https://prometheus.io/) endpoint when they run in an environment where those metrics can be scraped.

This convention is about **operational metrics**: counters, gauges and histograms that help teams operate the application. Product analytics and user behavior tracking should remain handled separately, for example through PostHog or a future OpenTelemetry-based analytics pipeline.

### Endpoint

Expose the metrics endpoint under the application API namespace, following this recommended pattern (see `messages` for a reference implementation):

```
/api/v1.0/prometheus/metrics
```

The endpoint should be disabled by default and enabled explicitly through configuration, for example with an environment variable such as `ENABLE_PROMETHEUS`.

### Authentication

The Prometheus endpoint should not be publicly accessible.

Use service-to-service authentication, for example a bearer token passed in the `Authorization` header and configured through an environment variable such as `PROMETHEUS_API_KEY`:

```http
Authorization: Bearer <prometheus-api-key>
```

This token is not user authentication. It is only intended to let the metrics scraper access the endpoint.

### What to expose

Metrics should describe the operational state of the application and should be useful for alerting, dashboards or capacity planning.

Good examples include:

* HTTP request count and latency;
* queue sizes and processing delays;
* number of objects in important operational states;
* storage usage;
* background job failures;
* external dependency errors.

Custom metrics should be global or instance-level. Do not expose per-user metrics, personal data, secrets or business information that is only useful for product analytics.

### Implementation notes

When adding a Prometheus endpoint:

* keep the feature behind an explicit configuration flag;
* protect the endpoint with a dedicated API key or equivalent service authentication;
* document the exposed custom metrics, their labels and their meaning;
* avoid labels with unbounded values such as user IDs, email addresses or arbitrary object IDs;
* make sure metrics collection does not perform expensive queries on every scrape;
* add tests for both authorized and unauthorized access to the endpoint.

The `messages` application provides a reference implementation and documents its endpoint in [`docs/prometheus.md`](https://github.com/suitenumerique/messages/blob/main/docs/prometheus.md).
