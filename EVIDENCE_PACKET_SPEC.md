# Evidence Packet Specification

Source task: AI-Stack issue #52, second foundation item.

Create the packet at step start and update it incrementally as work occurs.

Required contents:

- Contract
- RCA / trace
- Diff
- Unit test results
- Integration test results
- E2E test results
- Acceptance evidence
- Limits / unverified items
- Review result

## Completeness preflight

Do not send the packet to review unless the required sections are present and current.

At minimum, verify that:

- the contract is present
- the diff is present
- full test results are present, including failures
- integration and E2E results are present when required by the work
- acceptance evidence is mapped to the minimum required evidence in the contract
- limits or unverified items are stated
- current status is recorded as Pass, Fail, or Blocked

If a required item is missing, complete the packet before review.
