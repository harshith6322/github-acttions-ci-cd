Ah, Harshith — GitHub Actions **doesn’t automatically stop gracefully** on errors unless you handle them. Let’s break this down clearly so you can manage errors in your workflows.

---

## 1️⃣ Default Behavior

* If a step **fails** (non-zero exit code), the job **stops** and the workflow **fails**.
* Example:

```yaml
jobs:
  example:
    runs-on: ubuntu-latest
    steps:
      - name: Fail step
        run: exit 1
      - name: This will not run
        run: echo "Hello"
```

* `Fail step` returns `1` → workflow stops → `This will not run` is skipped.

---

## 2️⃣ Handle errors in **steps**

### a) `continue-on-error`

* Lets the workflow **continue even if a step fails**.

```yaml
- name: Run risky command
  run: exit 1
  continue-on-error: true
```

* The workflow continues, but the step is **marked as failed** in GitHub Actions UI.
* Useful if you want to run cleanup steps or always continue the pipeline.

---

### b) `if: ${{ success() }}` or `if: ${{ failure() }}`

* Run steps **conditionally** based on previous steps.

```yaml
- name: Run only if previous step failed
  if: ${{ failure() }}
  run: echo "Previous step failed"

- name: Run only if previous step succeeded
  if: ${{ success() }}
  run: echo "Previous step succeeded"
```

* Functions you can use:

  * `success()` → all previous steps succeeded
  * `failure()` → any previous step failed
  * `always()` → run regardless of success or failure

---

### c) `try-catch` style in bash

* You can handle errors **inside a step** using bash:

```yaml
- name: Handle errors in bash
  run: |
    set -e
    {
      echo "This may fail"
      false  # simulate failure
    } || {
      echo "Error handled, continue"
    }
```

* `set -e` stops on errors **unless** you catch it in `{ } || { }` block.

---

## 3️⃣ Job-level error handling

* You can **always run cleanup steps** using `if: always()`:

```yaml
jobs:
  example:
    runs-on: ubuntu-latest
    steps:
      - name: Run main task
        run: exit 1

      - name: Cleanup
        if: always()
        run: echo "This runs no matter what"
```

---

## 4️⃣ Example Workflow with Error Handling

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Step 1 - May fail
        run: |
          echo "Running step 1"
          exit 1
        continue-on-error: true

      - name: Step 2 - Conditional
        if: ${{ failure() }}
        run: echo "Step 1 failed, but workflow continues"

      - name: Step 3 - Always run
        if: always()
        run: echo "Cleanup or final step always runs"
```

---

✅ Key Takeaways:

1. `continue-on-error: true` → step failure doesn’t stop workflow
2. `if: success()/failure()/always()` → conditional step execution
3. Bash `||` blocks → handle errors inline
4. Use `always()` for **cleanup, notifications, or artifacts**

---

If you want, I can make a **robust template workflow** showing **error handling, conditional steps, and notifications** that you can drop into your projects.

Do you want me to do that?
