---
New Relic - get list of metrics available for app:

SELECT uniques(metricTimesliceName) 
FROM Metric WHERE appName='YOUR_APP_NAME' 
AND newrelic.timeslice.value IS NOT NULL

https://docs.newrelic.com/docs/data-apis/understand-data/metric-data/query-apm-metric-timeslice-data-nrql/#get-list

---
New Relic - implement grouping of requests by some request params (adding @Trace on controller to also get DB query)

Describe why was that needed at all

---
New Relic info:
1. FACET is really great thing, allows to see count of some grouping
2. New Relic metrics ARE Case-Sensitive

---
