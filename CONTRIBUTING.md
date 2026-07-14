# Contributing

Contributions are welcome! To add a new troubleshooting scenario:

1. Pick the relevant category file under `scenarios/` (or propose a new category).
2. Add an entry using this format:

   ```md
   ## <number>. <Short Title>

   **Symptom:** What the user/operator observes.

   **Root Cause:** The underlying reason this happens.

   **Fix:** The steps to resolve it.

   ```bash
   <a single representative diagnostic or fix command>
   ```
   ```

3. Add a row to the index table in `README.md`.
4. Open a pull request.

Keep entries concise, factual, and command-verified where possible.
