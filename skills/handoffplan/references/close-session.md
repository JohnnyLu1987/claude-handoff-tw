# Close Session Flow

Used by Step 8 of the handoff skill, after the user answers Yes / No / "close without commit".

## If the user says YES (or "yes"):

### 1. Commit all session work

```bash
git status -s
git diff --stat
```

If uncommitted changes exist, stage all changed/new files relevant to this session's work, then commit:

```
session: {slug} [{chain_tag}]

{One-line summary of what this session accomplished}

Handoff: {handoff_filename}
Bead(s): {bead_ids or "none"}

Generated with [Claude Code](https://claude.ai/code)

Co-Authored-By: Claude <noreply@anthropic.com>
```

**Be surgical:** only commit files related to this session's work.

If working tree is clean: "Working tree clean — nothing to commit."

### 2. Append to the handoff file

```
## Session Closed
**Closed at:** {timestamp}
**Commit:** {short hash}
**Session status:** Handed off to next session
```

### 3. Output the paste prompt

**Language — match the user's conversation language.** If the user has been communicating mainly in Traditional Chinese (繁體中文), output the 繁體中文 version below. Otherwise output the English version. Output ONLY one version, not both.

**English version:**
```
-------------------------------------------------------
PASTE THIS INTO YOUR NEXT SESSION:
-------------------------------------------------------
Read `{path to file}` (seq {N}, {chain_tag}) and continue from "Where We're Going".
Check `bd list --status=in_progress` for active work.

Before starting work, narrate your onboarding:
1. Read the handoff file and summarize what you understand
2. Show which bead(s) you're claiming and what phase/step you're starting
3. State what you'll verify first
4. Read the listed key files, then explore 2-3 adjacent files not listed
5. Explain your planned first action and why
Then wait for my go-ahead before executing.
-------------------------------------------------------
```

**繁體中文版：**
```
-------------------------------------------------------
貼到你的下一個 SESSION：
-------------------------------------------------------
讀取 `{path to file}`（seq {N}，{chain_tag}），從「Where We're Going」繼續。
執行 `bd list --status=in_progress` 查看進行中的工作。

開始工作前，先敘述你的 onboarding：
1. 讀取 handoff 檔案，摘要你的理解
2. 說明你要認領哪些 bead、從哪個 phase/step 開始
3. 說明你會先驗證什麼
4. 讀取列出的關鍵檔案，再額外探索 2-3 個未列出的相鄰檔案
5. 解釋你計畫的第一個動作及原因
然後等我同意再執行。
-------------------------------------------------------
```

### 4. Tell the user

"Session is closed. Paste the prompt above into a fresh session to continue."

## If the user says "close without commit":

Do steps 2-4 above, skip the commit. Warn: "Changes are uncommitted — next session or other sessions may see dirty state."

## If the user says NO:

1. Tell the user: "Handoff saved. When you're ready to close, say 'close session' or run `/handoff` again."
2. Continue the conversation normally.
3. On any subsequent `/handoff` or "close session" or "done" or "wrap up", repeat this close flow.
