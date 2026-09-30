# Atlas Proof Action Demo

Official demonstration repository for [`choreoatlas2025/action`](https://github.com/choreoatlas2025/action) - FlowSpec-Trace alignment validation in CI/CD.

## What This Demonstrates

This repository shows **real CI validation** using the atlas-proof GitHub Action with two scenarios:

- ✅ **Pass Scenario**: Complete trace with 100% match rate
- ❌ **Fail Scenario**: Incomplete trace with 75% match rate (missing payment step)

## Quick Start

### 1. Fork and Run

```bash
# Fork this repository
gh repo fork choreoatlas2025/action-demo --clone

# Trigger workflow
cd action-demo
git commit --allow-empty -m "Test atlas-proof action"
git push
```

### 2. View Results

Check the [Actions tab](https://github.com/choreoatlas2025/action-demo/actions) to see:
- **Test Pass Scenario** job - validates complete trace (succeeds)
- **Test Fail Scenario** job - validates incomplete trace (fails but marked as success due to `continue-on-error`)

## Files Structure

```
.github/workflows/test-atlas-proof.yml  # CI workflow using action@v0.0.1
specs/order-flow.yaml                   # FlowSpec definition (4 steps)
traces/
  ├── complete-order.json               # All 4 steps present
  └── incomplete-order.json             # Only 3 steps (missing payment)
```

## Example Output

### Pass Scenario (100% match)
```
🔍 Atlas Proof Report
==================================================
Match Rate: 100% (threshold: 90%)
Expected: 4 | Actual: 4 | Matched: 4

✅ PASSED
==================================================
```

### Fail Scenario (75% match)
```
🔍 Atlas Proof Report
==================================================
Match Rate: 75% (threshold: 90%)
Expected: 4 | Actual: 3 | Matched: 3

❌ Missing Steps (1):
   - paymentService.processPayment

❌ FAILED
==================================================
```

## Related Examples

This follows ChoreoAtlas's **non-invasive demonstration** approach:

- [cli#49](https://github.com/choreoatlas2025/cli/pull/49) - Minimal CI Gate in cli repository
- [quickstart-demo#1](https://github.com/choreoatlas2025/quickstart-demo/pull/1) - 3-step Fail→Fix→Pass demo

## Usage in Your Repository

Add to your `.github/workflows/validate.yml`:

```yaml
name: Validate FlowSpec Alignment

on: [pull_request]

jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Validate trace
        uses: choreoatlas2025/action@v0.0.1
        with:
          flowspec: 'specs/your-flow.yaml'
          trace: 'traces/your-trace.json'
          threshold: '0.90'
          format: 'text'
```

## License

Apache-2.0

## Links

- [Action Repository](https://github.com/choreoatlas2025/action)
- [ChoreoAtlas Website](https://cq365.eu.org/)
- [Documentation](https://cq365.eu.org/docs/)
