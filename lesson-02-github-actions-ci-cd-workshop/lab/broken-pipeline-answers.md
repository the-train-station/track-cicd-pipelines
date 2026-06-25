# Broken Pipeline - Answer Key

Below are the 6 bugs in `broken-pipeline.yml`, with line numbers, explanations, and fixes.

---

## Bug 1: Invalid runner name (Line 33)

**What's wrong**: `runs-on: ubuntu-latests` has a typo - extra "s" at the end.

**Why it breaks**: GitHub will fail to find a runner matching this label, and the job will error immediately with "The requested runner was not found."

**Fix**:
```yaml
# Before
runs-on: ubuntu-latests

# After
runs-on: ubuntu-latest
```

---

## Bug 2: Incorrect indentation on `run` key (Line 50)

**What's wrong**: The `run:` key under "Run ESLint" step is indented with extra spaces, making it a child of `name:` instead of a sibling.

**Why it breaks**: YAML parser sees `run` as a property of the `name` scalar rather than a step-level key. GitHub Actions will report the step has no `run`, `uses`, or other action defined.

**Fix**:
```yaml
# Before
      - name: Run ESLint
          run: pnpm lint

# After
      - name: Run ESLint
        run: pnpm lint
```

---

## Bug 3: Typo in action reference (Line 57)

**What's wrong**: `uses: action/checkout@v4` is missing the "s" - should be `actions/checkout@v4`.

**Why it breaks**: GitHub will fail to find the repository `action/checkout` and the job will error with "Unable to resolve action."

**Fix**:
```yaml
# Before
uses: action/checkout@v4

# After
uses: actions/checkout@v4
```

---

## Bug 4: Node version not quoted (Line 42)

**What's wrong**: `node-version: 20` is unquoted. While this often works, it can cause issues because YAML may interpret bare numbers differently than strings, and some Node version values (like `18.0`) get interpreted as floats and truncated to `18`.

**Why it breaks**: The value `20` works in this case, but the inconsistency with the test job (which correctly quotes `"20"`) is a code smell. More critically, if you change this to `20.0` it will be parsed as the float `20` not the string `"20.0"`. Always quote version numbers.

**Fix**:
```yaml
# Before
node-version: 20

# After
node-version: "20"
```

---

## Bug 5: Wrong cache type for package manager (Line 85)

**What's wrong**: `cache: "npm"` is specified but the workflow uses pnpm. The cache key must match the actual package manager.

**Why it breaks**: `setup-node` will look for `package-lock.json` (npm's lockfile) to generate the cache key. Since the project uses pnpm and has `pnpm-lock.yaml`, the cache will never hit. Every run reinstalls from scratch.

**Fix**:
```yaml
# Before
cache: "npm"

# After
cache: "pnpm"
```

---

## Bug 6: Missing `path` in upload-artifact (Line 95)

**What's wrong**: The "Upload build" step uses `actions/upload-artifact@v4` but is missing the required `path` input that specifies which files to upload.

**Why it breaks**: The action requires knowing which files/directories to upload. Without `path`, the action will error with "Input required and not supplied: path."

**Fix**:
```yaml
# Before
      - name: Upload build
        uses: actions/upload-artifact@v4
        with:
          name: build-output
          retention-days: 7

# After
      - name: Upload build
        uses: actions/upload-artifact@v4
        with:
          name: build-output
          path: dist/
          retention-days: 7
```
