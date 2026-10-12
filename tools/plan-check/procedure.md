# Procedure: how this skill grades a plan package

These steps are for grading a plan, not for writing one. Follow them in order and do only what each step says.

## Read order

1. Read the repro-evidence block first, before the plan. Write down, step by step: what was run, what was observed, and every control run (the same action with one thing removed or changed) with its result. Note which component or layer the controls and timings point at, and which they rule out. This is the confirmed cause and its supporting data.
2. Read the issue context and the thread highlights. Write down any explicit maintainer direction (a requested action, a stated cause, a "please do X").
3. Read the repo-facts block. Write down the bug-template asks and any contribution rule, including any AI-use disclosure requirement.
4. Read the candidate plan: its Cause/Diagnosis, In/Out scope, Changes, Test plan, and Risks/Unknowns. Do not treat any cause the plan quotes from the thread as established. Only the repro evidence from step 1 establishes things.
5. Read the candidate plan comment last.

## Evidence gathering

Take each check in the rubric's table order and write notes for it. Guidance on how to find the evidence is listed below:

1. For cause-fits-repro: look in the plan's Cause/Diagnosis. Copy its one-sentence cause. Then go through each repro step from Read order step 1 and write "consistent" or "ruled out" next to it, with the quote that decides it. This compares the plan's cause with the cause the repro confirms.
2. For scope-bounded: look in the plan's In/Out scope lines and its Changes list. List every item in the Changes list and write whether the issue title or the repro's Actual line asks for it. Copy the plan's out-of-scope sentence, or write "no out-of-scope statement".
3. For executable: look in the plan's Changes or Approach text. Write the file(s), function(s) or area it names and the action it takes there. If any decision is left for build time, copy the words that defer it.
4. For test-observable: look in the plan's Test plan. Copy the test sentence. Write the observable outcome it names (an output, exit code, timing, or test result) and which repro step it re-runs.
5. For comms-fit: look in the plan comment. List each maintainer direction from the thread highlights and quote where the comment engages it, or write "not engaged". List each repo-facts rule and quote where the comment meets it, or write "not met". If the policy requires an AI-use disclosure, look for it in the comment.
6. For honest-unknowns: look in the plan's Risks/Unknowns. Copy those lines, or write "none stated". Note any unverified claim written as fact.

## Check execution

1. Run the checks in the rubric's table order. Judge each one from your Evidence gathering notes. Do not re-read the whole package for a check unless your notes for it are empty.
2. Apply the pass condition exactly as written. If the notes show a fail condition, grade `fail` even when the rest of the plan is strong. If they show the pass condition clearly met, grade `pass` even when the plan is short.
3. If the package has no text at all on that check (for example, the plan has no Test plan section), grade `fail`. If text exists but is vague or ambiguous, so you cannot tell whether the pass condition is met, grade `unclear`. Use `unclear` only after you have tried to apply the pass condition and could not decide.
4. For each check, write one line: the grade and the quote or fact that decided it.
5. A plan's cause that is only quoted from the thread is checked against the repro like any other cause. Do not accept it for being confident or for being the thread's consensus.

## Verdict assembly

1. List the grades of the required checks: cause-fits-repro, scope-bounded, executable, test-observable, comms-fit.
2. If all five are `pass`, the verdict is `accept`.
3. If any is `fail` or `unclear`, the verdict is `reject`.
4. Ignore honest-unknowns when deciding. Report its grade only.
5. In the output, give the quote or fact that decided each check, and for a reject name the failed check or checks first. Then write the JSON block, with the verdict last.
