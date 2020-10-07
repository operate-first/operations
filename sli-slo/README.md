This folder contains all the SLIs (Service Level Indicators) and SLOs (Service Level Objectives) defined for each of the OperateFirst services/applications.

## SLI
An SLI is a service level **indicator**—a carefully defined quantitative measure of some aspect of the level of service that is provided.

Most services consider **request latency**—how long it takes to return a response to a request—as a key SLI. Other common SLIs include the **error rate**, often expressed as a fraction of all requests received, and **system throughput**, typically measured in requests per second. The measurements are often aggregated: i.e., raw data is collected over a measurement window and then turned into a rate, average, or percentile.

Ideally, the SLI directly measures a service level of interest, but sometimes only a proxy is available because the desired measure may be hard to obtain or interpret. For example, client-side latency is often the more user-relevant metric, but it might only be possible to measure latency at the server.

## SLO
An SLO is a **service level objective**: a target value or range of values for a service level that is measured by an SLI. A natural structure for SLOs is thus **SLI ≤ target**, or **lower bound ≤ SLI ≤ upper bound**. For example, we might decide that we will return Shakespeare search results "quickly," adopting an SLO that our average search request latency should be less than 100 milliseconds.

Choosing and publishing SLOs sets expectations to users about how a service will perform.

For more information please refer to the [Google SRE book](https://landing.google.com/sre/sre-book/chapters/service-level-objectives/).
