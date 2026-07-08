# How does UMA resolve prediction markets?

The flexibility of UMA makes it a natural fit for resolving prediction markets which serve a wide range of markets with different resolution requirements

The major prediction markets that UMA serves integrate UMA in the following way:

#### Market Lifecycle

An oracle request including the market rules is made at the time of market creation, the request proceeds through the oracle lifecycle as described in [how-does-umas-oracle-work.md](how-does-umas-oracle-work.md "mention"), and the market resolves as per that request unless one of the below cases occurs:

* if the first request for a given market is disputed, a 2nd request is made with the same rules. The settlement of the prediction market then ignores the 1st request and only resolves as per the 2nd request. If the 2nd request is disputed, the market will resolve as per the 2nd request. This acts as a form of escalation and prevents minor one-time proposer or disputer errors from slowing down the market resolution process with dispute resolution.
* if a market's 2nd or greater oracle request resolves as TOO EARLY (aka P4, learn more [here](https://blog.uma.xyz/articles/what-is-p4)), a subsequent oracle request is created and the market stays open and resolves as per the latest request. &#x20;

#### Rule Clarifications and Updates

The market rules specify how the integration can provide updates or clarifications to the rules in between market creation and settlement

#### Proposer Whitelist

Requests use the [managedoptimisticoraclev2.md](../developers/managedoptimisticoraclev2.md "mention") to specify a proposer whitelist of addresses that are able to propose the request. Learn more at [default-proposer-whitelist.md](../using-uma/default-proposer-whitelist.md "mention").

#### Challenge Period Extensions

Requests that use the [managedoptimisticoraclev2.md](../developers/managedoptimisticoraclev2.md "mention") specify a minimum challenge period. Risk Labs operates an automated proposal review system that uses LLMs and market data to flag proposals. Flagged proposals have an extended challenge period that allows additional time for all UMA partipicants to review and potentially dispute the proposal. The extended challenge period ends after the Risk Labs team has reviewed the proposal. This feature allows challenge periods to be right sized to the proposal. For example, an unflagged sports game proposals can safely resolve after a short minimum challenge period (e.g. 15 minutes) and complex or subjective geopolitics proposal can be extended past the default 2 hour minimum challenge period.
