# Prompt: Update constructs.md after a code change

Use this prompt after a meaningful architecture or product responsibility change.

```text
Review the recent code changes and update constructs.md only if the construct model changed.

Check whether:

1. A new product/system job was introduced.
2. An existing construct changed how it fulfills its JTBD.
3. A construct should be split into sub-constructs.
4. A construct is no longer needed.
5. A collaboration surface changed.
6. A boundary was added, removed, or violated intentionally.
7. Primary code ownership changed.
8. There is new architecture drift that should be captured as an open question.

Do not add noisy implementation details.
Do not document every file touched.
Do not invent future constructs.

Return:

1. Whether constructs.md needs an update.
2. The exact patch you propose.
3. The reason for each change.
```
