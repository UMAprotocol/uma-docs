# How does UMA work?

UMA is an optimistic oracle that resolves wide-ranging requests for information in a scalable and reliable way. The system has two components: the optimistic oracle and a dispute resolution mechanism called the DVM. The optimistic oracle resolves the vast majority of requests, currently 99.8%, quickly and cheaply, and escalates the others to be resolved through dispute resolution.

### Optimistic Oracle

The optimistic oracle resolves discrete requests for information using proposers and disputers who are economically incentivized to participate honestly.

#### Requests

Discrete data requests are posted to the oracle and include:

* Detailed instructions on how the request should be resolved at some time in the future. For example: “This request will resolve YES if John Doe wins the election, or NO if John Doe does not win.”
* A proposer reward for correctly proposing the request
* A bond amount that proposers must risk to propose the request
* A minimum challenge period duration during which pending proposals can be disputed

#### Proposals

Third-party proposers review open requests and propose resolutions when their resolution criteria are satisfied. Each request can only be proposed once. A proposal includes:

* A proposed resolution to the request, such as YES
* A proposer bond that is refundable if the proposal is deemed correct

The proposer reward incentivizes proposers to participate. The proposer bond incentivizes proposers to post only correct proposals.

Proposals are considered pending for a challenge period during which anyone can dispute the proposal. If no dispute is sent during the challenge period, the proposal is settled as correct, and the proposer receives the reward and the refunded proposer bond.

#### Disputes

Disputers review proposals during the challenge period for correctness. If they find an incorrect proposal, they submit a dispute that includes:

* An assertion that the proposal is incorrect
* A dispute bond that is refundable if the proposal is deemed incorrect

Each proposal can only be disputed once. Correct disputes are rewarded with a portion of the forfeited proposer bond. This incentivizes disputers to review proposals. The dispute bond incentivizes disputers to make only correct disputes.

Disputes are forwarded to the DVM for dispute resolution. After the DVM has resolved a dispute, the proposal and dispute bonds are settled.

### Dispute Resolution via DVM

The Data Verification Mechanism, or DVM, resolves disputes sent to it by UMA’s optimistic oracle. The DVM is a Schelling point mechanism where UMA stakers resolve disputes in return for staking rewards. Stakers commit secret votes during a 24-hour commit period and reveal them in the following 24-hour reveal period.

Disputes resolve when a minimum 65% majority of staked UMA is cast in favor of a single outcome. Stakers who did not vote, or who voted against the majority, are slashed, with the slashed amount redistributed to the majority voters. Slashing and rewards incentivize stakers to vote for the most correct answer. Casting votes in secret prevents lazy voters from copying other voters and dishonest voters from coordinating on an incorrect result.

Unstaking UMA requires waiting a one-week period before the tokens are released. If the DVM were successfully corrupted by 65% of stake voting against reality, the UMA token would depreciate significantly in the following week, resulting in a loss for the attackers. This creates a cost of corruption that disincentivizes corrupting the oracle.
