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
* 