# Unit Testing & TDD Quality Checklist

Choose checks based on the behavior and risk of the change. Unit, contract, database integration, and workflow tests exercise different guarantees.

---

## 1. Test Architecture & Structure (Vladimir Khorikov)
- [ ] **Arrange-Act-Assert (AAA):** Is the test clearly partitioned into Arrange (setup), Act (execution), and Assert (verification) sections?
- [ ] **Observable Behavior:** Does the test verify what the system achieves for the user/client, rather than asserting on internal private methods?
- [ ] **Single concept per test.** Does each test describe a coherent scenario? Assert the outcomes needed to show that scenario works.
- [ ] **Meaningful names.** Describe the scenario and observable outcome in domain language. Khorikov discusses the drawbacks of rigid templates. [Manning, chapter 3 §3.4 listing](https://livebook.manning.com/learn/unit-testing) · [Khorikov's explanation](https://khorikov.org/posts/2021-02-25-test-naming-conventions/)

---

## 2. Test Doubles Discipline (Mocks vs. Stubs)
- [ ] **Stub Invariant:** Are Stubs used strictly to feed incoming data into the SUT?
- [ ] **Zero Stub Assertions:** Is the test free of interaction assertions on stubs (`expect(stubRepo.find).toHaveBeenCalled()`)?
- [ ] **Mock Discipline:** Are Mocks used exclusively to verify outgoing external side effects (e.g., dispatching an email or sending a webhook)?
- [ ] **No In-Memory Mocking:** Are domain objects, value objects, and internal calculation classes tested as real objects rather than mocked?

---

## 3. The Four Pillars Evaluation
- [ ] **Protection Against Regression:** Does the test exercise core domain complexity and edge cases where bugs could actually occur?
- [ ] **Resistance to Refactoring:** Can the internal implementation of the class be completely rewritten without this test breaking?
- [ ] **Fast Feedback:** Does this unit test run quickly (milliseconds) without disk or network I/O?
- [ ] **Maintainability.** Can a reader see the inputs and expected outcomes? Prefer parameterized cases or property runners when they keep failures easy to reproduce.

---

## 4. Unit Tests & Property Tests (Derived)

- [ ] **Exact outcomes.** Cover zero, positive and negative values where supported, rounding ties, and representation limits. Assert exact amounts and currency or unit. For example, under a first-remainder policy, splitting 10,000 minor units among three equal shares in stable order yields `[3334, 3333, 3333]`.
- [ ] **Invariants.** Generate valid inputs to check properties such as preservation of the total, repeatability of rounding and allocation under the same policy, and correct rejection of invalid inputs. Record the failing example or seed so it can be replayed.
- [ ] **Time and identity.** Inject a clock or ID generator where time or identity affects behavior. Advance a controlled clock across expiry boundaries. Model retries with the same saved operation ID and independent operations with different IDs.
- [ ] **Test-first when useful.** For a new calculation rule or a regression that needs coverage, prefer expressing the next behavior with a failing test. Confirm it fails for the intended reason before implementing. Reuse existing tests that already cover a behavior-preserving change.

## 5. Shared Vectors & Old-Client Contracts (Derived)

- [ ] **Shared vectors.** Store versioned JSON fixtures with exact inputs and expected outputs for money, rounding and allocation, units, pricing revisions, or retry hashes shared across implementations. Use the contract's exact encoding and record its rounding or tie-break policy. Run the same fixtures in each language that implements the rule.
- [ ] **Compatibility.** Replay representative request, response, and stored queue fixtures from each supported contract version through current decoders, handlers, and upgrade paths. Check accepted or rejected results, preserved operation identity, and the effect of uploading an older client's pending queue. Use captured or version-pinned synthetic fixtures so current serializers cannot silently rewrite the older format.

## 6. Real Database Integration (Derived)

- [ ] **Atomic commit and rollback.** Run production migrations on isolated instances of the deployed engines. Inject a failure between writes and verify complete rollback. On success, verify that the durable effect and its recorded operation result commit together.
- [ ] **Concurrency and replay.** Use independent connections and an explicit barrier to submit the same operation concurrently. Verify that exactly one effect is committed and that subsequent exact replays return the stored outcome. Reuse the same key with a different payload and check the contract's conflict outcome.
- [ ] **Isolation.** Use the runtime database roles, including worker roles, and two organization fixtures. Verify allowed access within the correct scope and denied reads and writes across organizations. Exercise RLS in PostgreSQL where applicable.

## 7. Workflow Recovery (Derived)

- [ ] **Lost response.** Drop the response after commit, then retry via the normal queue or command path. Verify one recorded effect, the same durable identity and result, and the correct operator-visible status.
- [ ] **Restart or restore.** Restart or restore a device with pending operations, then synchronize. Verify preserved business identity and one effect per business operation, including when reconciliation gives the device a new epoch.
- [ ] **Retry policy.** Exercise transient failures, invalid requests, and ambiguous external outcomes where applicable. Verify the defined retry, rejection, or reconciliation path and that unrelated eligible work can proceed.
