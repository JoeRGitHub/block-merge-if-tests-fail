# Block Merge if Tests Fail

## Instructions

### Add a Basic Python Test File

Create `test_app.py`:

```python
def test_add():
    assert 2 + 2 == 4
```

Create `requirements.txt`:

```
pytest
```

---

### Add the Workflow File `.github/workflows/pr-test.yml`

```yaml
name: PR Test Workflow

on:
  pull_request:
    branches:
      - main

jobs:
  test:
    runs-on: ubuntu-latest
    name: Run Pytest on PR
    steps:
    # Add steps here
```

---

### Enable Branch Protection

1. Go to **GitHub repo → Settings → Branches**
2. Under "Branch protection rules":
   - Click "Add rule"
   - Set branch name to `main`
   - Enable:
     - ✅ "Require status checks to pass before merging"
     - ✅ Select the workflow name (e.g., `PR Test Workflow`)
     - ✅ Optional: Require pull request review

Now GitHub will **block merges to `main`** until the test workflow succeeds.

---

### Test It

1. Create a new branch:

   ```bash
   git checkout -b feature/test-failing
   ```

2. Change `test_app.py` to make it fail:

   ```python
   def test_add():
       assert 2 + 2 == 5  # Wrong on purpose
   ```

3. Push and open a PR:

   ```bash
   git add .
   git commit -m "Breaking test"
   git push origin feature/test-failing
   ```

4. Observe:
   - The PR triggers the test
   - The test fails
   - GitHub blocks the "Merge" button

source: https://github.com/dor-amar/DevopsCourse-INT-DEC-2024/blob/main/labs/github-actions-block-merge.md
