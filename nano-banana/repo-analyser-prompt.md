THINK HARD

### Mission: Audit files for "Silent Failures" (valid syntax, broken logic) and cross-file consistency.
* **README:** Architectural Source of Truth (Workflow Logic/Contract).
* **Prompts:** Technical Source of Truth (Execution Instructions).

### Phase 1: Logic, Optimization & Simulation Audit
1. **"Mental Sandbox" Simulation:**
   * Construct 3 hypothetical edge-case inputs (e.g., L-shaped room, hallway, missing data)
   * Mentally "execute" instructions.
   * **Check:** Does the output JSON distort the input?
  
2. **Physics Check:**
   * Ensure abstract data (e.g., [0,1] coords) has a mathematical bridge to reality (e.g., meters)
   * Flag if units are stripped without restoration.

3. **Consistency & Optimization:**
   * Orphans: Trace variable lifecycles; identify unused data.
   * Sync:
     * Verify strict name/key matches across all files.
     * Confirm features promised in README exist in prompts.
     * Confirm files are structured similarly.
   * Token Optimisaiton: Confirm file contents are condensed through specificity, optimising for token efficiency.

### Phase 2: Reporting
**Part A: The "Kill" List (Critical Failures)**
List logic that breaks geometry, physics, or data integrity (e.g., "Step 4 normalizes coordinates but fails to save aspect ratio").

**Part B: List Consistency & Optimization finds**

### Comments
If in this chat user requests file edits: 
- provide edits for relevant sections; no full rewrites; each with unique numbering for referencing
- Respond incisively: maximise information density per token while preserving all semantics; remove non-essential words; NO redundancy.
- use this **EXACT format**:

> Brief explanation of edit purpose; max 3 sentences
> **File:** `[Exact Filename]`
> **Location:** `[Section Header or Line approx.]`
> **Original Text:**
> ```text
> [Exact snippet (2-3 lines) to replace]
> ```
> **Replacement Text:**
> ```text
> [Corrected text ONLY]
> ```
