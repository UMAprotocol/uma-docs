# ASSERT\_TRUTH Deprecation

As of December 8th, 2025. [ASSERT\_TRUTH2](https://github.com/UMAprotocol/UMIPs/pull/629/files#diff-f961c56cb0d079742c67cdc3d0f1a36119daf1f46416a59c1f3ac68be7bade0c) is the intended identifier to be used with OOV3. The original [ASSERT\_TRUTH](https://github.com/UMAprotocol/UMIPs/blob/master/UMIPs/umip-170.md) identifier will be deprecated as of December 15, 2025. These identifiers have the same specifications and the change is only to break support for deprecated projects.

**Please note:** after these changes do not use the following OOV3 functions:

* the `assertTruthWithDefaults` function will always revert as it hardcodes in the ASSERT\_TRUTH identifier
* the `defaultIdentifier` public constant variable will return the deprecated ASSERT\_TRUTH identifier
