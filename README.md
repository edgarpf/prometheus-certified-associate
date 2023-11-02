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
* Service discovery should be used to automatically discover all nodes in a Kubernetes cluster.
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
* Heatmap is the MOST appropriate for displaying the bucket values of a histogram over time in Grafana.
* Grafana only queries the data that already exists so scrape timeout would have no meaning in that context.
* relabel_configs can be used to apply metadata exposed via the various Prometheus service discovery methods as labels on scraped targets.
* Prometheus does not recommend or support Network File System (NFS) usage for Prometheus storage.
* An exemplar is a reference to some external data associated with a time series, commonly a trace.
* scrape_timeout defines how long to wait for a response before giving up on a scrape.
* Alert rules are defined in designated files, referenced by the Prometheus configuration file.
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
* Do not use labels to store dimensions with high cardinality (many different label values), such as user IDs, email addresses, or other unbounded sets of values.
* A counter is a cumulative metric that represents a single monotonically increasing counter whose value can only increase or be reset to zero on restart. For example, you can use a counter to represent the number of requests served, tasks completed, or errors.
Do not use a counter to expose a value that can decrease. For example, do not use a counter for the number of currently running processes; instead use a gauge.
* A gauge is a metric that represents a single numerical value that can arbitrarily go up and down.
Gauges are typically used for measured values like temperatures or current memory usage, but also "counts" that can go up and down, like the number of concurrent requests.
* A histogram samples observations (usually things like request durations or response sizes) and counts them in configurable buckets. It also provides a sum of all observed values.
* Similar to a histogram, a summary samples observations (usually things like request durations and response sizes). While it also provides a total count of observations and a sum of all observed values, it calculates configurable quantiles over a sliding time window.
* In Prometheus terms, an endpoint you can scrape is called an instance, usually corresponding to a single process. A collection of instances with the same purpose, a process replicated for scalability or reliability for example, is called a job.
* When Prometheus scrapes a target, it attaches some labels automatically to the scraped time series which serve to identify the scraped target: job and instance.
* Prometheus supports two types of rules which may be configured and then evaluated at regular intervals: recording rules and alerting rules. To include rules in Prometheus, create a file containing the necessary rule statements and have Prometheus load the file via the rule_files field in the Prometheus configuration. Rule files use YAML.
* You can use promtool to test your rules.
  * ./promtool test rules test.yml
* Prometheus supports basic authentication and TLS. This is experimental and might change in the future.
* In Prometheus's expression language, an expression or sub-expression can evaluate to one of four types:
  * Instant vector - a set of time series containing a single sample for each time series, all sharing the same timestamp
  * Range vector - a set of time series containing a range of data points over time for each time series
  * Scalar - a simple numeric floating point value
  * String - a simple string value; currently unused
* Strings may be specified as literals in single quotes, double quotes or backticks.
* The offset modifier always needs to follow the selector immediately
* The @ modifier allows changing the evaluation time for individual instant and range vectors in a query. The time supplied to the @ modifier is a unix timestamp and described with a float literal.
* Prometheus supports the following built-in aggregation operators that can be used to aggregate the elements of a single instant vector, resulting in a new vector of fewer elements with aggregated values:
  * sum (calculate sum over dimensions)
  * min (select minimum over dimensions)
  * max (select maximum over dimensions)
  * avg (calculate the average over dimensions)
  * group (all values in the resulting vector are 1)
  * stddev (calculate population standard deviation over dimensions)
  * stdvar (calculate population standard variance over dimensions)
  * count (count number of elements in the vector)
  * count_values (count number of elements with the same value)
  * bottomk (smallest k elements by sample value)
  * topk (largest k elements by sample value)
  * quantile (calculate φ-quantile (0 ≤ φ ≤ 1) over dimensions)
* Metrics have a TYPE and HELP attribute.
  * TYPE– Specifies what type of metric(counter, gauge, histogram, summary).
  * HELP – description of what the metric is.
* Metric name is just another label.
* Labels surrounded by __ are considered internal to prometheus.
* Every metric is assigned 2 labels by default(instance and job).
* A PromQL expression can evaluate to one of four types:
  * String – a simple string value (currently unused) 
  * Scalar – a simple numeric floating point value
  * Instant Vector – set of time series containing a single sample for each time series, all sharing the same timestamp.
  * Range Vector – set of time series containing a data points over time for each time series.
* PromQL has 3 logical operators: or and unless.
* Ignoring keyword can be used to “ignore” labels to ensure there is a match between 2 vectors.
* Ignoring keyword is used to ignore a label when matching, the on keyword is to specify exact list of labels to match on.
* Resulting vector will have matching elements with all labels listed in “on” or all labels NOT listed in “ignoring”.
* group_left tells PromQL that elements from the right side are now matched with multiple elements from the left.
* The by clause allows you to choose which labels to aggregate along.
* The without keyword does the opposite of by and tells the query which labels not to include in the aggregation.
* rate
  * Looks at the first and last data points within a range.
  * Effectively an average rate over the range.
  * Best used for slow moving counter and alerting rules.
* irate
  * Looks at the last two data points
within a ranged
  * Instant rate
  * Should be used for graphing volatile,
fast-moving counters.
* Quantiles - determine how many values in a distribution are above or below a certain limit.
* Histogram quantiles can be used to accurately measure if a specific SLO is being met and can be used to generate alerts if a SLO is exceeded.
  * histogram_quantile(0.95, request_latency_seconds_bucket)
* Each histogram bucket is stored as a separate time series, so having too
many buckets will results:
  * High Cardinality
  * High Ram usage
  * High disk space
  * Slower performance for inserts in Prometheus database
* Histogram
  * Bucket sizes can be picked
  * Less taxing on client libraries
  * Any quantile can be selected
  * Prometheus server must calculate quantiles 
* Summary
  * Quantile must be defined ahead of time
  * More taxing on client libraries
  * Only quantiles predefined in client can be used
  * Very minimal server-side cost
* Recording Rules allow Prometheus to periodically evaluate PromQL expressions and store the resulting times series generated by them.
* Recording rules go in a sperate file called a rule files.
* Console templates allow you to create your own custom html pages using Go templating language.
* Metrics should follow snake_case – lowercase with words separated by _
* The first word of the metric should be the application/library the metric is used for.
* The name portion of the metric name should provide a description of what the metric is used for. More than one word can be used.
* The unit should always be included in the metric name, so there’s no second guessing which units its using.
* Make sure to use unprefixed base units like seconds and bytes, and meters. We don’t want to use microseconds or kilobytes.
* Re-labeling – allows you to classify/filter Prometheus targets and metrics by rewriting their label set.
* To change the delimiter between labels use the separator property.
* Target labels - are labels that are added to the labels of every time series returned from a scrape.
* Metrics can be pushed to the Pushgateway using one of the following methods:
  * Send HTTP Requests to Pushgateway
  * Prometheus Client Libraries
* The for clause tells Prometheus that an expression must evaluate to true for a specified period of time before firing alert.
* Alert states:
  * Inactive – Expression has not returned any results
  * Pending – expression returned results but it hasn’t been long enough to be considered firing(5m in this case)
  * Firing – Active for more than the defined for clause(5m in this case)
* Labels can be added to alerts to provide a mechanism to classify and match specific alerts in Alertmanager.
* Annotations can be used to provide additional information, however unlike labels they do no play a part in the alerts identity. So they cannot be use for routing in Alertmanager.
* Alertmanager is responsible for receiving alerts generated from Prometheus and converting them into notifications. These notifications can include, pages,
webhooks, email messages, and chat messages.
* Alerts can be silenced to prevent generating notifications for a period of time.
* The API response format is JSON. Every successful API request returns a 2xx status code.
* Invalid requests that reach the API handlers return a JSON error object and one of the following HTTP response codes:
  * 400 Bad Request when parameters are missing or incorrect.
  * 422 Unprocessable Entity when an expression can't be executed (RFC4918).
  * 503 Service Unavailable when queries time out or abort.
* Instant queries - URL query parameters:
  * query=<string>: Prometheus expression query string.
  * time=<rfc3339 | unix_timestamp>: Evaluation timestamp. Optional.
  * timeout=<duration>: Evaluation timeout. Optional. Defaults to and is capped by the value of the -query.timeout flag.
* Range Queries URL query parameters:
  * query=<string>: Prometheus expression query string.
  * start=<rfc3339 | unix_timestamp>: Start timestamp, inclusive.
  * end=<rfc3339 | unix_timestamp>: End timestamp, inclusive.
  * step=<duration | float>: Query resolution step width in duration format or float number of seconds.
  * timeout=<duration>: Evaluation timeout. Optional. Defaults to and is capped by the value of the -query.timeout flag.
* Finding series by label matchers URL query parameters:
  * match[]=<series_selector>: Repeated series selector argument that selects the series to return. At least one match[] argument must be provided.
  * start=<rfc3339 | unix_timestamp>: Start timestamp.
  * end=<rfc3339 | unix_timestamp>: End timestamp.
* We have found the following guidelines very effective:
  * Have no more than 5 graphs on a console.
  * Have no more than 5 plots (lines) on each graph. You can get away with more if it is a stacked/area graph.
  * When using the provided console template examples, avoid more than 20-30 entries in the right-hand-side table.
* Keep alerting simple, alert on symptoms, have good consoles to allow pinpointing causes, and avoid having pages where there is nothing to do. Aim to have as few alerts as possible, by alerting on symptoms that are associated with end-user pain rather than trying to catch every possible way that pain could be caused. Alerts should link to relevant consoles and make it easy to figure out which component is at fault.
* We only recommend using the Pushgateway in certain limited cases. There are several pitfalls when blindly using the Pushgateway instead of Prometheus's usual pull model for general metrics collection:
  * When monitoring multiple instances through a single Pushgateway, the Pushgateway becomes both a single point of failure and a potential bottleneck.
  * You lose Prometheus's automatic instance health monitoring via the up metric (generated on every scrape).
  * The Pushgateway never forgets series pushed to it and will expose them to Prometheus forever unless those series are manually deleted via the Pushgateway's API. 
* Usually, the only valid use case for the Pushgateway is for capturing the outcome of a service-level batch job. 
