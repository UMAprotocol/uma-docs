# ManagedOptimisticOracleV2

The [ManagedOptimisticOracleV2](https://github.com/UMAprotocol/managed-oracle/blob/master/src/optimistic-oracle-v2/implementation/ManagedOptimisticOracleV2.sol) contract is modified version of the OptimisticOracleV2 intended to be deployed for a single integration. It allows the integration to designate a request manager address that can set a default proposer whitelist and easily manage an existing request's bond size, liveness and proposer whitelist before the request has been proposed.
