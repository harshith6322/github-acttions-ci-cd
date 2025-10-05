Got it, Harshith — let’s make a **simple, clear cheat-sheet** for GitHub Actions storage options, including **Environment vs Repository**, and **Secrets vs Variables**.

---

# GitHub Actions: Secrets & Variables Cheat-Sheet

## 1️⃣ Repository-Level

| Type          | Purpose                                   | Scope            | Access in Workflow    | Notes                                                                  |
| ------------- | ----------------------------------------- | ---------------- | --------------------- | ---------------------------------------------------------------------- |
| **Secrets**   | Sensitive info (passwords, API keys)      | Whole repository | `${{ secrets.NAME }}` | Encrypted, masked in logs, cannot be viewed once created               |
| **Variables** | Non-sensitive configuration (URLs, flags) | Whole repository | `${{ vars.NAME }}`    | Can be secret or non-secret, visible if non-secret, not masked in logs |

---

## 2️⃣ Environment-Level

> An **environment** is like `staging`, `production`, etc.

| Type          | Purpose                                   | Scope                                | Access in Workflow    | Notes                                                                  |
| ------------- | ----------------------------------------- | ------------------------------------ | --------------------- | ---------------------------------------------------------------------- |
| **Secrets**   | Sensitive info specific to an environment | Only jobs targeting this environment | `${{ secrets.NAME }}` | Can require approval before job runs; safer for production credentials |
| **Variables** | Config specific to environment            | Only jobs targeting this environment | `${{ vars.NAME }}`    | Non-sensitive values like API endpoints, feature flags                 |

---

## 3️⃣ Key Differences Summary

| Feature            | Repository                                 | Environment                                |
| ------------------ | ------------------------------------------ | ------------------------------------------ |
| Scope              | Entire repo                                | Specific environment (staging/prod)        |
| Secrets            | Repo-wide                                  | Environment-specific, optional approval    |
| Variables          | Repo-wide                                  | Environment-specific                       |
| Masked in logs?    | Secrets ✅                                  | Secrets ✅                                  |
| Access in workflow | `${{ secrets.NAME }}` / `${{ vars.NAME }}` | `${{ secrets.NAME }}` / `${{ vars.NAME }}` |
| Use case           | General credentials / config               | Deployment-specific config / credentials   |

---

### 4️⃣ Notes / Best Practices

1. Use **repository secrets** for general tokens, e.g., Docker Hub, GitHub Token.
2. Use **environment secrets** for sensitive production credentials (AWS, DB passwords).
3. Use **variables** for non-sensitive configuration like URLs, flags, version numbers.
4. Access secrets/vars in workflow using:

   ```yaml
   run: echo "${{ secrets.MY_SECRET }}"
   run: echo "${{ vars.MY_VAR }}"
   ```
5. Secrets are **always masked in logs**; variables are **visible if non-secret**.

---

If you want, I can also make a **tiny diagram** showing **Repository vs Environment, and Secrets vs Variables**, which makes this cheat-sheet **super visual and easy to remember**.

Do you want me to do that?
