# Moving Features Extension Specification

- **Title:** Moving Features
- **Identifier:** <https://iaocea.github.io/moving-features/v1.0.0/schema.json>
- **Field Name Prefix:** mf
- **Scope:** Item
- **Extension [Maturity Classification](https://github.com/radiantearth/stac-spec/tree/master/extensions/README.md#extension-maturity):** Proposal
- **Owner**: @keewis

This document explains the Moving Features extension to the [SpatioTemporal Asset
Catalog](https://github.com/radiantearth/stac-spec) (STAC) specification.

The intention of the first version of the specification is to encode a **Trajectory** object as
defined by the [OGC Moving Features Encoding Extension –
JSON](https://docs.ogc.org/is/19-045r3/19-045r3.html) standard. Future versions will aspire to also
encode **Prism** objects.

- Examples:
  - [Item example](examples/item.json): Shows the basic usage of the extension in a STAC Item
- [JSON Schema](json-schema/schema.json)
- [Changelog](./CHANGELOG.md)

## Fields

The fields in the table below can be used in these parts of STAC documents:

- [ ] Catalogs
- [ ] Collections
- [x] Item Properties (incl. Summaries in Collections)
- [ ] Assets (for both Collections and Items, incl. Item Asset Definitions in Collections)
- [ ] Links

| Field Name | Type               | Description                                                       |
|------------|--------------------|-------------------------------------------------------------------|
| datetimes  | [string \| number] | **REQUIRED** (trajectory): one timestamp per node in a linestring |

### Additional Field Information

#### datetimes

To properly represent trajectories, the `geometry` field of a item **must** have a type of
`"LineString"` and the coordinates **must** describe *at least* 2 positions.

Once that is the case, the `datetimes` property has to be an array with the same number of elements
as the `coordinates` property of the geometry. Its values **must** describe time instants in
monotonic increasing order (without duplicated values) and may be:
- numeric values of milliseconds since 1970-01-01 00:00:00 UTC (unix timestamps)
- strings describing IETF RFC 3339 encoded timestamps
- strings describing ISO8601 encoded timestamps following the Gregorian calendar

Mixing different kinds of timestamp encodings is not allowed.

The `datetime` property from the base metadata should be `null`, and the `start_datetime` and
`end_datetime` properties should have the same value as the first and last values from `datetimes`.

## Contributing

All contributions are subject to the
[STAC Specification Code of Conduct](https://github.com/radiantearth/stac-spec/blob/master/CODE_OF_CONDUCT.md).
For contributions, please follow the
[STAC specification contributing guide](https://github.com/radiantearth/stac-spec/blob/master/CONTRIBUTING.md) Instructions
for running tests are copied here for convenience.

### Running tests

The same checks that run as checks on PR's are part of the repository and can be run locally to verify that changes are valid. 
To run tests locally, you'll need `npm`, which is a standard part of any [node.js installation](https://nodejs.org/en/download/).

First you'll need to install everything with npm once. Just navigate to the root of this repository and on 
your command line run:
```bash
npm install
```

Then to check markdown formatting and test the examples against the JSON schema, you can run:
```bash
npm test
```

This will spit out the same texts that you see online, and you can then go and fix your markdown or examples.

If the tests reveal formatting problems with the examples, you can fix them with:
```bash
npm run format-examples
```
