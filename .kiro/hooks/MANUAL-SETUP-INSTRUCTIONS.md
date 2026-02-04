# Manual Hook Setup Instructions

The `.kiro.hook` files in this directory are templates. Kiro hooks must be created through the Kiro IDE UI.

## How to Create Hooks Manually

1. **Open Kiro IDE**
2. **Click "Create New Hook"** in the Agent Hooks panel (left sidebar)
3. **Copy the information** from each `.kiro.hook` file below
4. **Paste into the Kiro UI** fields

---

## Hook 1: Check Console.log ⭐⭐⭐ ESSENTIAL

**File:** `check-console-log.kiro.hook`

**Copy these values into Kiro UI:**
- **Title:** `Check Console.log`
- **Description:** `Check for console.log statements after agent completes and warn before committing`
- **Event:** Select `Agent Stop`
- **File path(s) to watch:** Leave empty
- **Action:** Select `Ask Kiro`
- **Instructions:** 
  ```
  Check all modified files for console.log statements. Run this command:
  grep -rn "console.log" --include="*.ts" --include="*.tsx" --include="*.js" --include="*.jsx" src/
  
  If any console.log statements are found, warn me to remove them before committing. List the file names and line numbers.
  ```

---

## Hook 2: TS File Formatter ⭐⭐⭐ ESSENTIAL

**File:** `ts-formatter.kiro.hook`

**Copy these values into Kiro UI:**
- **Title:** `TS File Formatter`
- **Description:** `Automatically runs Prettier to reformat TypeScript files whenever they are saved`
- **Event:** Select `File Saved`
- **File path(s) to watch:** `**/*.ts`
- **Action:** Select `Shell Command`
- **Shell command:** `npx prettier --write "$filePath"`

---

## Hook 3: JS/React File Formatter ⭐⭐ IMPORTANT

**File:** `js-react-formatter.kiro.hook`

**Copy these values into Kiro UI:**
- **Title:** `JS/React File Formatter`
- **Description:** `Automatically runs Prettier to reformat JavaScript and React files whenever they are saved`
- **Event:** Select `File Saved`
- **File path(s) to watch:** `**/*.{js,jsx,tsx}`
- **Action:** Select `Shell Command`
- **Shell command:** `npx prettier --write "$filePath"`

---

## Hook 4: Security Scanner ⭐⭐⭐ ESSENTIAL

**File:** `security-scanner.kiro.hook`

**Copy these values into Kiro UI:**
- **Title:** `Security Scanner`
- **Description:** `Scan for hardcoded secrets and security issues when code files are saved`
- **Event:** Select `File Saved`
- **File path(s) to watch:** `**/*.{ts,tsx,js,jsx}`
- **Action:** Select `Ask Kiro`
- **Instructions:**
  ```
  Scan the saved file for security issues:
  1. Hardcoded API keys (look for: sk-, api_key, password, token, secret)
  2. SQL injection risks (string concatenation in database queries)
  3. XSS vulnerabilities (unescaped HTML rendering)
  
  Report any findings with severity level (CRITICAL/HIGH/MEDIUM). If no issues found, stay silent.
  ```

---

## Hook 5: TypeScript Type Checker ⭐ OPTIONAL

**Copy these values into Kiro UI:**
- **Title:** `TypeScript Type Checker`
- **Description:** `Run TypeScript compiler to check for type errors after saving .ts/.tsx files`
- **Event:** Select `File Saved`
- **File path(s) to watch:** `**/*.{ts,tsx}`
- **Action:** Select `Shell Command`
- **Shell command:** `npx tsc --noEmit 2>&1 | grep "$filePath" | head -10`

---

## Hook 6: Test File Suggester ⭐ OPTIONAL

**Copy these values into Kiro UI:**
- **Title:** `Test File Suggester`
- **Description:** `Remind to create test files when new source files are created`
- **Event:** Select `File Saved` (or `File Created` if available)
- **File path(s) to watch:** `src/**/*.{ts,tsx}`
- **Action:** Select `Ask Kiro`
- **Instructions:**
  ```
  A new source file was created. Check if a corresponding test file exists (*.test.ts or *.spec.ts in the same directory).
  
  If no test file exists, remind me to create one following TDD principles. Suggest 3-5 test cases based on the file name and typical patterns.
  
  If a test file already exists, stay silent.
  ```

---

## Priority Order

Create in this order:
1. ✅ Hook 1 (Check Console.log) - Most important
2. ✅ Hook 2 (TS Formatter) - Very useful
3. ✅ Hook 4 (Security Scanner) - Critical
4. ⏸️ Hook 3 (JS/React Formatter) - Test first 3, then add
5. ⏸️ Hook 5 (Type Checker) - Optional
6. ⏸️ Hook 6 (Test Suggester) - Optional

---

## Testing Your Hooks

After creating each hook:

1. **For File Saved hooks:** Edit and save a matching file
2. **For Agent Stop hooks:** Send a message to the agent and wait for response
3. **Check if hook triggered:** Look for formatter output or agent messages
4. **Adjust if needed:** Modify file patterns or instructions

---

## Troubleshooting

**Hook not triggering:**
- Check file pattern matches your files
- Verify hook is enabled in Kiro UI
- Try saving the file again

**Shell command fails:**
- Test command in terminal first
- Check if tool is installed (prettier, tsc, etc.)
- Verify $filePath variable is supported

**Agent too chatty:**
- Add "If no issues found, stay silent" to instructions
- Narrow file patterns to reduce triggers

---

## Alternative: Import Hooks

If Kiro supports importing hooks, you might be able to:
1. Look for "Import Hooks" option in Kiro UI
2. Select the `.kiro.hook` files from this directory
3. Kiro might parse and create them automatically

**Check Kiro documentation** for hook import features.

---

**Bottom Line:** You need to manually create these hooks through the Kiro UI by copying the values from the `.kiro.hook` files. Start with the 3 essential hooks (1, 2, 4) and test them before adding more.
