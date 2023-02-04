# prometheus-certified-associate

* Email is a poor choice for a label as there will be a large number of different emails for an application and this will lead to high cardinality.
* The double underscore **__** before a label name signifies the label is a reserved label.
* This configuration will make Prometheus doesn’t scrape targets with a label of **team: frontend**:
  ```yaml
    relabel_configs:
    - source_labels: [team]
        regex: frontend
        action: drop
  ```
* Alert labels can be used as metadata so alertmanager can match on them and performing routing policies, Annotations should be used for cosmetic descriptions of the alerts.
* Prometheus follows a pull-based model. The prometheus.yml file will have a list of all targets Prometheus will need to scrape, which involves sending an HTTP request to the target.
* Use **ignoring(some_label)** to ignore labels in metrics operations.
* Prometheus uses Time Series database.
* Hit the reload config button is not a valid method for reloading alertmanager configuration.
* Client libraries are to instrument applications so Prometheus can collect metrics from them. In addition, client libraries can also be used to push metrics to a Push Gateway.
* Console templates allow you to create custom dashboards to display metrics and graphs.
* **scrape_interval** configs determine how often to scrape a target. If scrape_interval is set to 30s then each target will get scraped every 30s.
* A counter metric can be used to count the number of seconds a server has been running for. The uptime of a server can never go down, so a gauge metric shouldn’t be used.
* Inactive, pending and firing are the different states a Prometheus alert can be in.
* Example of annotation:
  * description: Instance {{ .Labels.instance }} has low disk space on filesystem {{ .Labels.mountpoint }}, current free space is at {{ .Value }}%
* 2 mo is not a valid time value to be used in a range selector.
* /metrics is the default path Prometheus will scrape to collect metrics.
* The cli utility that ships with Prometheus is called promtool.
* Complexity is not something that is tracked in a span within a trace.
* If a rule does not match with any of the parent routes, so it goes to the default route.
* 9090 is the default web port of Prometheus.
* Service discovert should be used to automatically discover all nodes in a Kubernetes cluster.
* The purpose of the **honor_labels: true** allows metrics to specify the instance and job labels instead of pulling it from scrape_configs.
* Exporters are responsible for collecting metrics from an instance and exposing them in a format Prometheus expects.
* Alerts and recording rules should be defined in a separate rules file. This file then needs to be referenced in the prometheus.yml file as per: prometheus.yml rule_files: - rules.yml.
* Since temperature readings can go up or down, a gauge metric should be used in this case.
* For histogram metrics, to calculate a quantile use the `histogram_quantile` function. The function takes two arguments, the desired percentile, and the histogram metric, make sure to pass in the _bucket sub metric.
* For histograms, quantiles must be calculated server side, thus they are less taxing on client libraries. whereas summary metrics are the opposite, as everything is calculated client side.
* Silences allow you to temporarily pause the generation of notifications for a specific alert(s).
* When naming metrics, the first word should be the application/library, which in this case is `redis`. The name second part of the metric name should be the metric name. The last part of the name should be the unit which should be unprefixed, so that means we prefer bytes, seconds over kilobytes, and milliseconds.
* The repeat interval determines how long alertmanager will wait before sending another notification.
* promtool is the cli utility for performing config validation. The `check config` subcommands perform validation of the passed in the configuration.
* Traces allow you to follow operations as they traverse through various systems & services, allowing you to follow a request hop by hop through a system.
* The range vector selector `[5m]` will return all values for a metric over the past 5 minutes, so it returns a range vector.
* To match on any mountpoint starting with /run a regular expression /run.* must be used. 
* Since Prometheus follows a pull-based model, this makes gathering metrics from short-lived jobs difficult. The push gateway allows short-lived jobs to push metrics to the push gateway, and the Prometheus server can scrape the push gateway.
* rate() calculates average rate over entire interval, irate() calculates the rate only between the last two datapoints in an interval.
* for attribute in a Prometheus alert rule determines how long a rule must be true before firing an alarm.
* To get all time series for both jobs `node`, and `web`, we will need to use a regular expression matcher `=~`. The regular expression to get both of those jobs is `web|node`.
* The 3 main components of the Prometheus: instance or retrieval node, tsdb, and the HTTP server. The retrieval node is responsible for scraping targets and collecting metrics. After the metrics are scraped, they will be stored in a time series database. To retrieve scraped metrics a query can be sent to the HTTP server.
* Histogram metrics have 3 submetrics: count: total number of observations sum: sum of all observations bucket: number of observations for a specific bucket.
* Instance and job are two labels assigned to every metric by default.
* For useful SLIs, you want to find metrics that accurately measure a user’s experience. So things like high CPU, memory, and disk utilization would make for poor SLIs, as a user may not experience any degradation of service during these events.
* Logging, metrics and tracing are the 3 components of observability.
* The `metrics_path` property can be updated with a path different from the default.
* The **group_interval** property determines how long alertmanager will wait after sending a notification, before it sends a new notification for a group.
* When an alert arrives on alertmanager, alertmanager will wait the amount of time specified on `group_wait`, to wait for other alerts to arrive before firing off a notification.
* The default delimiter is `;`.
* Prometheus provides a functional query language called PromQL.
* The time unit for hours is just `h` not `hr`.
* Alert rules are defined on the prometheus server in a separate rules file.
* 64 bits float is the data type do Prometheus metric values use.
* The `up` query will return a `1` if a target is able to be successfully scraped and a `0` if it is not.
* Groups are run in parallel and rules within a group are run in sequentially.
* Prometheus client libraries allow you to instrument applications.
* The two types of attributes metrics can have are: help - description of what the metric is type - specifies what type of metric(counter, gauge, histogram, summary).
* scheme field will alllow prometheus using http or https.
* group_by: ['...'] is used to effectively disable grouping of alerts for a particular Alertmanager route.
* avg_over_time(up[5m]) contains temporal aggregations are time-based.
* Include a version label on all exported metrics is not a best practice as it is recommend to use a designated build_info metric with a version label and dynamically attach it via PromQL grouping as-needed.
* A numeric measuremen is the definition of a metric.
* **relabel_configs** with action keep will drop all targets that do not match the regular expression and keep the ones that do.
* While Prometheus exposes the synthetic ALERTS  time series based on the alerts it evaluates, the only way to get the currently firing alerts in Alertmanager is via the Alertmanager API, not PromQL.
* Prometheus label values may contain any Unicode characters, including numbers and underscores. Additionally, unlike label names, there is no restriction that label values cannot start with a number.
* For each instance scrape, Prometheus stores a sample in the following time series:
  * up{job="<job-name>", instance="<instance-id>"}: 1 if the instance is healthy, i.e. reachable, or 0 if the scrape failed.
  * scrape_duration_seconds{job="<job-name>", instance="<instance-id>"}: duration of the scrape.
  * scrape_samples_post_metric_relabeling{job="<job-name>", instance="<instance-id>"}: the number of samples remaining after metric relabeling was applied.
  * scrape_samples_scraped{job="<job-name>", instance="<instance-id>"}: the number of samples the target exposed.
  * scrape_series_added{job="<job-name>", instance="<instance-id>"}: the approximate number of new series in this scrape. New in v2.10
* span is a call or logical section within a larger transaction detailing response time, status code and other metadata about the call
* Log is a textual representation of an event, commonly structured with key-value-based metadata.
* While Alertmanager integrates with some notification providers that can send text notifications, Alertmanager itself cannot directly generate text message notifications.
* The rate function should only be used with counters, not gauges.
* Sending a SIGHUP to the Prometheus process to trigger a Prometheus configuration reload,
* The offset modifier must be directly applied to a selector, not to the result of a function or aggregation.
* Metrics registry is a set of metrics in an instrumented application that will be returned when scraped.
* ec2_sd_configs can be used in AWS.
* SLA > SLO > SLI.
* kubernetes_sd_configs is BEST for identifying Kubernetes pods to scrape.
* Prometheus recording rules evaluates a PromQL expression and save the result back to the TSDB as a new metric.
* Inhibiting is a policy that when one type of alert fires, notifications should not be sent for a certain different type of alert.
* Globally via command line flags is Prometheus data retention configured.
* target and module must be specified as part of a Blackbox Exporter probe.
* Summary metrics generally cannot be aggregated.
* SNMP Exporter is ideal for monitoring network devices.
* increase operates on counter metrics while delta operates on gauge metrics.
* Error budget is the maximum time a system can be down without violating an SLO or SLA.
* Prometheus remote writing is the ability for Prometheus to stream samples to an endpoint.
* The sum and count aggregations drop the metric name from the result.
* Addition (+) can be used with either counters or gauges.
* JMX Exporter is BEST for monitoring Java Virtual Machine (JVM) metrics.
* static_configs * requires manually setting each scrape target.
* Unlike other monitoring software like statsd, Prometheus does not "namespace" metrics using dots.
* http is the networking scheme Prometheus use by default for scrapes.
* Status is a user interface tab allows viewing the configured service discovery as well as discovered targets.
* Heamap is the MOST appropriate for displaying the bucket values of a histogram over time in Grafana.
* Grafana only queries the data that already exists so scrape timeout would have no meaning in that context.
* relabel_configs can be used to apply metadata exposed via the various Prometheus service discovery methods as labels on scraped targets.
* Prometheus does not recommend or support Network File System (NFS) usage for Prometheus storage.
* An exemplar is a reference to some external data associated with a time series, commonly a trace.
* scrape_timeout defines how long to wait for a response before giving up on a scrape.
* Aler rules are defined in designated files, referenced by the Prometheus configuration file.
* Set the --web.enable-lifecycle command line flag to be able to reload Prometheus via its /-/reload endpoint.
* Summary is samples numerical observations and calculates configurable quantiles over a sliding time window.
* SMTP is the Simple Mail Transfer Protocol and has no role in Prometheus scrapes.
* As a best practice, alerting should be done on symptoms, not causes.
* promtool test rules can be used for unit testing Prometheus rules.
* deriv operates on gauge metrics while rate operates on counter metrics” is the correct answer.
* Error budget policy is a set of actions to be taken when an SLO or SLA has been breached
* Prometheus data model consist of metrics names, metric labels and samples.
* Months are not allowed for use in range selectors as they vary in the number of days they contain.
* predict_linear can be used to forecast the future value of a time series.
* a vector matching must contain the keywords on or ignoring.
* Blackbox Exporter is BEST for monitoring an HTTP web server endpoint to ensure that it returns a 200 status code.
* The idelta function is only appropriate for use with gauge metrics.
* Prometheus  is responsible for evaluating alerting rules.
* The Agent mode of Prometheus disables querying, alerting and local storage whereas the normal mode of Prometheus has each of these enabled.
* Metric type, help text, one or more samples are the three principal components of the Prometheus exposition format.
* Status is the user interface tab allows for viewing the current configuration of Prometheus.
* absent returns a result of 1 if the specified time series does not exist.
* A scrape instance is a single target within a scrape job.
* 9093 is the default web port of Alertmanager.
* Prometheus instrumentation is writing application code to expose desired aspects of the app as Prometheus metrics.
* The delta function considers the entire range interval while the idelta function only considers the last two samples in the interval.
* histogram_quantile yields the φ-quantile of a histogram.
* Module  is the name of an individual Blackbox Exporter probe configuration.
* Left and right single or double quotes are not valid in PromQL.
* promtool check config can be used to check the validity of the Prometheus configuration file.
* for specify how long the expression must be true before an alert is fired.
* **X-Prometheus-Scrape-Timeout-Seconds** is an HTTP header set by Prometheus on each scrape.
* You want to ensure that the API is served using HTTPS and that the returned status code is in the range 200-299:
  ```yaml
  prober: http
  http:
    valid_status_codes: 2xx
    fail_if_not_ssl: true
  ```
* **ALERTS{alertstate="firing"}** will return the currently firing alerts in Prometheus.
* file is service discovery method, the MOST generic.