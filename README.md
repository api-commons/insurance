# Insurance

The common insurance vocabulary -- the quotes a customer is given, the policies they buy, the coverages in them, the people insured and the claims they make -- distilled from the APIs that already exist. Health insurance belongs to the healthcare context. Part of [Schema Commons](https://github.com/api-commons/schema-commons),
published by the [API Commons](https://apicommons.org).

Built from the providers tagged Insurance, Insurtech, Property and Casualty, Life Insurance, Embedded Insurance, Claims, Underwriting, Auto Insurance in [APIs.io](https://apis.io) that
publish their own OpenAPI. Only harvested contracts count -- nothing API Evangelist
generated. Every number below is a count of providers.

## What the market already agrees on

<!-- commons:summary -->
| Concept | Expose it | Readable schema | Core properties (share) | Collection named |
| --- | ---: | ---: | --- | --- |
| policy | 30 | 23 | effective_date 83%, status 78%, expiration_date 70%, number 61%, id 56% | policies 24, policy 9 |
| quote | 34 | 31 | id 55%, premium 52% | quotes 27, quote 11 |
| claim | 16 | 11 | status 82%, policy_id 73%, id 64%, loss_date 64%, description 55%, number 55% | claims 15, claim 3, fnol 1 |
| insurance_product | 5 | 3 |  | products 4, insurance-products 1 |

Cohort: 143 providers with harvested OpenAPI, 47 crosswalks. Of the providers exposing a policy: 57% create, 60% list, 67% fetch, 30% update, 17% delete. Generated 2026-11-11 by build.py.
<!-- /commons:summary -->

Shares are of the providers with a readable resource schema for that concept. The
extended tier and the long tail of every concept are in [profile.yml](profile.yml).

## What the measurement shows

- A claim agrees on more than any other insurance resource: number, status, the policy, the loss date and a description are all core.
- A policy agrees on its number, status and effective and expiration dates; what it covers is modelled too differently to share.
- A quote agrees only on id and premium, with status just under (48%): embedded-insurance APIs quote by product and carriers quote by risk, and the request shapes diverge.
- Coverages live on the policy and the quote, not as resources (2 providers).

## Artifacts

- [openapi.yml](openapi.yml) -- OpenAPI 3.1 of the shared operations. No one serves it; it
  is the shape to map to. Lints clean against the
  [naming ruleset](https://github.com/api-commons/schema-commons/blob/main/ruleset/schema-commons-naming.yaml).
- [arazzo.yml](arazzo.yml) -- reference workflows composed only of the shared operations (compositions, not measurements).
- [schema/](schema) -- JSON Schema 2020-12 per concept; each property carries
  `x-commons-tier`, `x-commons-share`, `x-commons-providers` and the spellings seen.
- [crosswalk/](crosswalk) -- one per provider, common name -> their name, with the harvested
  spec and operations each mapping was read from.
- [adopters.yml](adopters.yml) -- who exposes which concept, linked to their APIs.io page.
- [concepts.yml](concepts.yml) -- the only hand-written input: concepts, path segments and
  their qualifiers, aliases, reviewed exclusions, merged duplicates, and the decisions below.
- [census/](census) -- the cohort and the raw census the profile is built from.

## Decisions

These are calls, not measurements, recorded so they can be revisited as the method matures.

- Health insurance is the healthcare context's: payers, benefits plans and medical claims are a different domain from property, casualty and life.
- Policyholders are not published as a concept: counting only `insureds` and `policyholders` (`members` and `customers` were workspace members and agency CRM records), 2 providers expose them.
- `plans` is not an insurance product here: reviewed, it was health, retirement and benefits plans.
- A claim needs no policy in the same API: claims and FNOL platforms expose claims alone.
- Duplicates merged: Audatex into Solera, Oper, Orbii, Thatch.

## Rebuild

```bash
../schema-commons/scripts/run.sh .
```

## License

Artifacts CC BY-NC-SA 4.0 ([LICENSE](LICENSE)); code Apache 2.0 ([LICENSE-CODE](LICENSE-CODE)).
