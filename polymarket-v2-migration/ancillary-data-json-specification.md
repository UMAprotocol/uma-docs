# Ancillary Data JSON Specification

### JSON fields

Encode requests as UTF-8 JSON objects. All fields below are required except integration.

* `type`: integer - 1 for binary markets; 2 for atomic neg-risk.
* `title`: string - market or event title.
* `description`: string - resolution rules, evidence sources, timing and exceptional cases.
* `market_id`: string - external market identifier, treated as an opaque string.
* `res_data`: array of value (string) and outcome (nonempty string). Array position has no meaning; never infer a value or code from ordering.
* `product_spec`: string - supplementary product specification or reference; use "" when absent.
* `integration`: - "polymarket" or other

Both arrays use entries with Numeric value strings encode unscaled decimal values. Convert exactly to the signed on-chain integer by multiplying by 10^18, without floating-point arithmetic. The Type 1 sentinel "minInt256" encodes −2^255 directly and is never scaled.

### Type 1: binary market

```
{
  "type": 1, // new json schemas for this type will increment to 1.1 
  "title": "Did it rain in Central Park?",
  "description": "full market rules or description optionally with a provided product_spec",
  "integration_id": "4805182", // marketId for Polymarket
  "res_data": [
    {"value": "0", "outcome": "No"}, // or Lakers, Under
    {"value": "1", "outcome": "Yes"}, // or Celtics, Over
    {"value": "0.5", "outcome": "Cancelled"} // do not include for incremental neg-risk events
  ],
  "product_spec": "", // url rule template
  "integration": “polymarket”
}
```

Notes: “Res\_data” must include "0" and "1" values exactly once. Value “0.5” is optional. Other values may not be used. Outcome labels describe the market-specific meaning of those values. A "0.5" settlement is valid only when permitted by the resolution rules and supported by the settlement contract; its presence in res\_data does not authorize that result. Too early is an oracle voting action and is not included in res\_data. Use market\_id to tie requests to the integrator’s APIs.
