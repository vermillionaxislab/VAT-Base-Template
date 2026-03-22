# System Audit & Debug Protocol — VAT-Base-Template

**Trigger:** Say `"audit"`, `"check everything"`, `"run tests"`, or `"full debug"` to execute this protocol.

This is a multi-angle diagnostic that tests every spec of the application from every possible avenue. Run after every significant change. Report all findings before marking any task complete.

---

## ANGLE 1 — JavaScript Syntax Validation

**What it catches:** Syntax errors, unclosed brackets, typos that break the entire app.

```bash
# Extract and validate all JS in index.html
sed -n '/<script>/,/<\/script>/p' index.html | sed '1d;$d' > /tmp/app_js.js
node -c /tmp/app_js.js

# Extract and validate book.html JS
sed -n '/<script>/,/<\/script>/p' book.html | sed '1d;$d' > /tmp/book_js.js
node -c /tmp/book_js.js
```

**Pass criteria:** Both output `OK` with no errors.

---

## ANGLE 2 — HTML Structure Validation

**What it catches:** Unclosed tags, malformed attributes, missing required elements.

```bash
# Count opening vs closing tags for critical elements
echo "=== DIV balance ==="
grep -c "<div" index.html && grep -c "</div>" index.html

echo "=== Script tags ==="
grep -c "<script" index.html && grep -c "</script>" index.html

echo "=== Style tags ==="
grep -c "<style" index.html && grep -c "</style>" index.html

# Check required meta tags exist
grep -c 'charset="UTF-8"' index.html
grep -c 'name="viewport"' index.html
grep -c 'rel="manifest"' index.html
grep -c 'name="theme-color"' index.html
```

**Pass criteria:** Opening/closing tag counts match. All required meta tags return `1`.

---

## ANGLE 3 — PWA & Manifest Validation

**What it catches:** Broken PWA install, missing icons, incorrect paths that fail on GitHub Pages sub-path.

```bash
# Validate manifest.json is valid JSON
node -e "JSON.parse(require('fs').readFileSync('manifest.json','utf8')); console.log('manifest.json: VALID JSON')"

# Check start_url and id are relative (not absolute /)
grep '"start_url"' manifest.json
grep '"id"' manifest.json
# Both must be "./" NOT "/"

# Check all icon files referenced in manifest actually exist
grep '"src"' manifest.json
ls icons/

# Verify service worker registration in index.html
grep 'serviceWorker' index.html | grep 'register'

# Verify sw.js uses relative paths
grep "'./'\\|\"./\"" sw.js
```

**Pass criteria:**
- `manifest.json` is valid JSON
- `start_url` and `id` are `"./"` not `"/"`
- All icon files exist in `icons/`
- Service worker registers with `./sw.js`

---

## ANGLE 4 — Asset & Path Audit

**What it catches:** 404 errors on GitHub Pages due to absolute paths or missing files.

```bash
# Find any absolute paths that would break on sub-path hosting
echo "=== Absolute paths in index.html (should be 0) ==="
grep -n 'href="/' index.html | grep -v "//fonts\|//cdn\|http" | head -20
grep -n "src='/" index.html | grep -v "//fonts\|//cdn\|http" | head -20

# Verify all local file references exist
echo "=== Checking referenced local files ==="
grep -o 'src="[^h][^t][^t][^p][^"]*"' index.html | grep -v "data:" | head -20
grep -o "href=\"[^h][^t][^t][^p][^\"]*\"" index.html | grep -v "//fonts\|//cdn" | head -20

# Verify icons exist
for f in icons/icon-192.png icons/icon-512.png icons/icon-152.png icons/icon-120.png icons/apple-touch-icon.png apple-touch-icon.png; do
  [ -f "$f" ] && echo "EXISTS: $f" || echo "MISSING: $f"
done

# Verify core files exist
for f in index.html book.html sw.js manifest.json 404.html .nojekyll .github/workflows/static.yml; do
  [ -f "$f" ] && echo "EXISTS: $f" || echo "MISSING: $f"
done
```

**Pass criteria:** No absolute paths to local files. All referenced assets exist.

---

## ANGLE 5 — Function Reference Audit

**What it catches:** `onclick` handlers calling functions that don't exist (causes silent button failures).

```bash
# Extract all onclick function names called in index.html
grep -o 'onclick="[^"]*"' index.html | sed 's/onclick="//;s/".*//' | grep -o '^[a-zA-Z_][a-zA-Z0-9_]*' | sort -u > /tmp/onclick_calls.txt

# Extract all defined function names
grep -o 'function [a-zA-Z_][a-zA-Z0-9_]*' index.html | sed 's/function //' | sort -u > /tmp/defined_fns.txt

# Find onclick calls with no matching function definition
echo "=== UNDEFINED function calls (should be empty) ==="
comm -23 /tmp/onclick_calls.txt /tmp/defined_fns.txt
```

**Pass criteria:** Output is empty — every onclick references a real function.

---

## ANGLE 6 — Navigation & Render Chain Audit

**What it catches:** Screens that won't render, broken navigation, missing render functions.

```bash
# Verify core navigation functions exist
for fn in go push pop renderDashboard renderClients renderSessions renderSchedule renderEarnings renderNutrition renderProgramBuilder; do
  grep -c "function $fn" index.html | grep -q "^0$" && echo "MISSING: $fn()" || echo "EXISTS: $fn()"
done

# Verify tab definitions match render functions
grep -o "go('[a-z]*')" index.html | sort -u

# Check render priority order is correct (nutrition before programBuilder)
grep -n "renderNutrition\|renderProgramBuilder\|_editProgram\|_editWorkout\|_editMealPlan" index.html | head -20
```

**Pass criteria:** All core functions exist. Render chain order is correct.

---

## ANGLE 7 — Data Integrity Audit

**What it catches:** localStorage key mismatches, broken save/load cycles.

```bash
# Verify localStorage keys are consistent
echo "=== localStorage keys used ==="
grep -o "localStorage\.\(getItem\|setItem\|removeItem\)('[^']*'" index.html | sort -u

# Verify LS helper is defined and used consistently
grep -c "var LS\s*=" index.html
grep -o "LS\.\(get\|set\|del\)('[^']*'" index.html | sort -u | head -30

# Check Supabase table names are consistent
grep -o "from('[^']*')\|\.from('[^']*')" index.html | sort -u
```

**Pass criteria:** LS keys are consistent. Supabase table names match across insert/select/delete.

---

## ANGLE 8 — Modal & UI System Audit

**What it catches:** Modals that won't open, close buttons that don't work, scroll issues.

```bash
# Verify showModal and closeModal functions exist
grep -c "function showModal" index.html
grep -c "function closeModal" index.html

# Check for nested overflow — this breaks iOS modals
grep -n "overflow-y:auto\|overflow-y: auto" index.html | head -20
# Any inside a modal content div is a potential iOS bug

# Verify all closeModal calls reference valid modal IDs
grep -o "closeModal('[^']*')" index.html | sort -u > /tmp/close_calls.txt
grep -o "showModal('[^']*')" index.html | sort -u > /tmp/show_calls.txt
echo "=== Modals opened but never closed (potential leak) ==="
comm -23 /tmp/show_calls.txt /tmp/close_calls.txt
```

**Pass criteria:** `showModal` and `closeModal` exist. No nested scroll overflow in modals.

---

## ANGLE 9 — Theme & CSS Variables Audit

**What it catches:** Hardcoded colors that won't respect theme switching, broken CSS variables.

```bash
# Find hardcoded colors that should be CSS variables (flag for review)
echo "=== Hardcoded color values (should use CSS vars) ==="
grep -n '#D4A830\|#D03030\|#020202\|#050505' index.html | grep -v ':root\|--\|var(' | head -20

# Verify CSS variables are defined in :root
grep -A 40 ':root{' index.html | head -45

# Verify both themes are defined
grep -c 'theme-crimson\|theme-gold' index.html
```

**Pass criteria:** No hardcoded colors outside `:root` and `var()` declarations. Both themes defined.

---

## ANGLE 10 — GitHub Actions & Deploy Audit

**What it catches:** Broken deploy pipeline, wrong branch triggers.

```bash
# Validate workflow YAML structure
cat .github/workflows/static.yml

# Verify .nojekyll exists (required for GitHub Pages with non-Jekyll sites)
[ -f .nojekyll ] && echo ".nojekyll: EXISTS" || echo ".nojekyll: MISSING — GitHub Pages will fail on underscore dirs"

# Verify no CNAME file (no custom domain for this repo)
[ -f CNAME ] && echo "WARNING: CNAME exists — remove if no custom domain" || echo "CNAME: not present (correct)"

# Check git status
git status
git log --oneline -5
```

**Pass criteria:** Workflow is valid. `.nojekyll` exists. No unexpected CNAME. No uncommitted changes after all tasks.

---

## ANGLE 11 — Security Audit

**What it catches:** Exposed credentials, insecure patterns, XSS risks.

```bash
# Check for hardcoded API keys or secrets
echo "=== Potential exposed secrets ==="
grep -n "apikey\|api_key\|secret\|password\|token" index.html | grep -iv "var\|function\|comment\|//\|PIN\|sessionStorage" | head -20

# Check Supabase URL/key (should be present but is a public anon key — OK)
grep -n "supabase\|SUPABASE" index.html | grep -i "url\|key" | head -5

# Verify PIN security uses sessionStorage (not localStorage — sessionStorage clears on tab close)
grep -n "PIN\|pin\|unlock\|lock" index.html | grep "sessionStorage\|localStorage" | head -10
```

**Pass criteria:** No private keys or passwords hardcoded. PIN state uses `sessionStorage`.

---

## FULL AUDIT REPORT FORMAT

When running a full audit, report back in this format:

```
AUDIT COMPLETE — [timestamp]

ANGLE 1 JS Syntax:        PASS / FAIL [details if fail]
ANGLE 2 HTML Structure:   PASS / FAIL
ANGLE 3 PWA & Manifest:   PASS / FAIL
ANGLE 4 Asset Paths:      PASS / FAIL
ANGLE 5 Function Refs:    PASS / FAIL [list undefined if fail]
ANGLE 6 Navigation:       PASS / FAIL
ANGLE 7 Data Integrity:   PASS / FAIL
ANGLE 8 Modals & UI:      PASS / FAIL
ANGLE 9 Theme & CSS:      PASS / FAIL
ANGLE 10 Deploy Pipeline: PASS / FAIL
ANGLE 11 Security:        PASS / FAIL

OVERALL: [X/11 passed]
ISSUES FOUND: [list]
FIXES APPLIED: [list]
STATUS: [Ready to ship / Needs fixes]
```

---

## Quick Single-Command Sanity Check

Run this after any small change as a fast baseline:

```bash
sed -n '/<script>/,/<\/script>/p' index.html | sed '1d;$d' > /tmp/app_js.js && node -c /tmp/app_js.js && echo "JS: OK" && node -e "JSON.parse(require('fs').readFileSync('manifest.json','utf8')); console.log('Manifest: OK')" && git status
```

If this passes: JS is valid, manifest is valid, and nothing is accidentally left uncommitted.
