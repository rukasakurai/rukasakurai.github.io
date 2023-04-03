# Monitoring
- Errors
  - Server-side errors: Errors caused by the server not processing requests as expected. These errors are clearly the responsibility of the service provider
    - Azure issues
    - Application code issues
  - Client-side errors: Errors that are cause by the client not request what the server expected. These errors might be a result of the service provider not clearly communicating expectations
- Response time: Slow response time may affect user satisfaction
- Suspicous activity
  - Suspicious requests
  - Suspicious changes to the system
- Also application/access logs help debug issues, so need to be able to access them when needed

## From experience
- Warnings that require no action
  - Higher than expected metrics (e.g., CPU utilization, system response time)
- Errors that require immediate recovery of service availability (automated or manual)
- Errors that require fixing records
- Errors that require bug fixing (e.g., performance not meeting SLA)
- Errors that require manual work (e.g., fix financial transactions)

# Keywords/buzzwords
Observability
ELK: Elasticsearch, Logstash, Kibana (with all product maintained by Elastic)
EFK: Elasticsearch, Fluentd, Kibana (with Fluentd maintained under Apache Licence v2.0)
Elasticsearch
Logstash
Fluentd
Kibana
Prometheus
Grafana
Datadog
Dynatrace
CloudWatch
Zabbix - Which I believe is strongest in network monitoring

Scheduled Events

ServiceNow