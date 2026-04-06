---
title: Original Prompt
---

# Original Prompt

This page preserves the original prompt that motivated the CloudMessages bootstrap effort. It is provided as background context rather than normative specification text.

The canonical preserved copy lives in the `cloudmessages/governance` repository.

- [View the governance copy on GitHub](https://github.com/cloudmessages/governance/blob/main/ORIGINAL_PROMPT.md)

## Prompt text

I believe that there can be more done to standardise messaging. cloudevents offers a standard for events, however, when I have worked with Event Modelling, CQRS, and DDD, it was also important to standardise Commands and Queries.

I'd like to create a plan that includes a github organisation, a spec for Commands and Queries, a github pages website stating the motivation and including clear instructions about when to use each message type.

In the motivation, what we want to be able to achieve is to deliver a clear message that with cloudevents, cloudcommands and cloudqueries, we can normalise all message types within a high velocity, fault tolerant, scalable, message-oriented organisation.

Via the specifications, and the SDKs it should be easy to see tracing from Query, through to Command, and via the state change, all the Events that are emitted.

The website and READMEs should state the thanks and acknowledgements to cloudevents, event modelling, cqrs and ddd.

In the plan, list what you think might be criticisms of the introduction of cloudcommands and cloudqueries as siblings to the cloudevents specification. Also consider what other advantages there might be.

The documentation will state how in contrast to Events, which are single publisher, multiple subscribers, cloudcommands should must be point to point: single publisher, single consumer. We don't want several processes changing state. When the commander creates the command and requests state modification (create, update, delete), it should receive back a sub type of a cloudcommand, a cloudcommandresponse. Ideally that response includes a unique request ID that the client can use for command fulfilment. To the discretion of the implementer, they can return an ID that will be used to create the object should the request be a Create request. If so, the ID is guaranteed to be held by an ID generator, or it is a UUID where uniqueness is guaranteed.

More information includes how with a Command the requester is expecting a specific service to respond (even if, in implementation, the service is a facade). Whereas with an Event, the publisher has no knowledge of the subscribers. Similarly, with a Query, the requester is expecting a specific responder. Compare this to SOAP, CORBA and other service discovery style patterns/technologies.

In the cloudcommand spec, the payload is the state to change. What is worth considering is how we can increase the likelihood of idempotence. There are the Commands that are naturally idempotent, such as a delete request, or an assignment or unassignment request (assigning the same address to a customer twice has no further effect). For some commands, where idempotence is trickier, for example a credit or debit, then there are potentially two strategies: (1) the command to change state includes the whole of the prior state with the change of state, though this still could be problematic, and (2) commands that are not idempotent must come with a version (optimistic lock) that the command is expecting to target.

In the cloudquery spec, the payload is the query itself. Probably there are plenty of query structure standards. Perhaps we can use one for the inner payload or leave it up to the implementer?

Furthermore, with the overarching website page, we can describe which technologies are good for Events (pub/sub, e.g. SNS) and which are good for Commands (AMPQ, e.g. SQS). Some technologies can be useful for both under certain conditions/configuration. For Queries, the example technologies are a little less clear. There's just the straight API request, then there's the asychronous request for Data (run this query, poll for results), and then things like socket streaming results (e.g. FIX) and dumping a file to an FTP location.

Similarly cloudqueries are conceptually a single requester and single responder. The difference is that there must not be a change of state with a query (telemetry is ok).

We could also, as a sub-type to cloudqueries, add cloudqueryresults. A standard for the cloudquery response.

For the specifications, we will want to be clear what type of message we are dealing with. For Commands, we could also specify whether it is a "create", "update", "delete" or "upsert".
