---
title: "Issue with Kafka batch processing"
date: 2023-08-09
---

Some time ago I got involved in a tricky case with commiting of Kafka batch.

Issue was that sometimes application could not commit Kafka batch and fail all records in it - but it was non-deterministic in nature, had some spikes & times of stability - so it seemed very tricky to pin-point the root cause.

To proceed with explanation what was actually going on - let's have some info on Kafka properties & batch processing:

1. Kafka consumer configuration has such properties as:
    - `max.poll.interval.ms` - time to process a batch
    - `max.poll.records` - number of records in a batch
2. Events in kafka batch are processed in sync way, one by one
3. If in scope of 1 batch application is not able to process all records within allowed time - it won't be able to do a kafka commit, which results in infinite retry of this batch - on this machine or on any other machine

However, after some time, we as a team could find out the root cause.

Root cause was actually quite complex, as it was a combination of next things:
1. Numbers of records in a batch was quite high (few hundreds)
  <br> It was expected to be actually utilized by one only one consumer in application, not all of the consumers - but was actually applied to all consumers as it was quicker to implement
2. Kafka retries of processing single message were quite high (~2 minutes)
  <br> Network issues (some external service unavailability) were not usual - and most of them would trigger L3 support team at night.
  <br> This was mitigated by updating kafka retries from constant back-off (~30 seconds) to exponential back-off (~2 minutes). This actually helped.
  <br> Side note here: I think that main issue was that some external services had downtimes during deployments - and it was evening for main team (US timezone) but night/early morning for support team (Europe timezone)
3. Time to process a batch was relatively high, but not extreme (~18 minutes)
4. In consumer flow we had an interesting situation - some event producers did not follow the contract of event.
  <br> Meaning, for example, if we have object `User` with field `employeeIdentifier` (let's assume that this is some universal id of user in a system - and user could be looked up by this id easily).
  <br> However, some producers would utilize this field to provide `name`. Others - would use this field to provide `socialNumber`. And some even would send `phoneNumber`. P.S. all field names are changed from actual ones, but you get the idea.
  <br> So, field `id` was populated with different sort of data (but let's be honest, all of them were unique, you just didn't knew the type of data).
  <br> Main point - it was decided by higher management for consumer application to handle it and find out type of data on our own.
5. Based on field data ambiguity - consumer app had a logic to try validate given value against external system #1 as `employeeIdentifier`, then as `name` against external system #2 and so on. If none of checks results with success - fail consuming of event.
6. Such fail was done via exception that was kafka-retryable - as sometimes some of checks (for example against external system #1) had some data consistency lags (as they also had consuming of similar events) - and flow was made retryable by Kafka to allow external system #1 to catch up (it actually worked in 90% of such failures earlier).
7. So application ended in a situation with long duration of retry, increased probability of causing retries and not so much duration for a kafka batch processing

And when application had 1 batch that had multiple events with improper `employeeIdentifier` value - and all them were processed one-by-one with retries up to 2 minutes - and total processing time exceeded defined time - application entered a kafka commit failure infinite loop.

Potential fixes that we discussed later:
1. Update config to reduce max poll records for a batch (was actually done on prod, fixed situation)
2. Review if exception could be ignored by kafka retry
3. Review consumer configuration to be applied only to specific consumers

Resulted in implementing all of 3 fixes over time.
