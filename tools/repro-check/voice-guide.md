# Voice guide: how I talk upstream

<!--
THIS IS THE PART YOU WRITE (new this week). Live mode reads this file
before any comment of yours goes out the door; eval mode ignores it
entirely, because your voice is yours and carries no gold labels.

This is not etiquette. "Be polite and concise" is advice for everyone
and therefore rules for no one. Write rules YOU need, in your own
words, each one concrete enough that the skill can hold a draft
against it and say which rule it breaks.

Three sections. Fill all three.
-->

## Who I am in threads

<!-- 2-3 lines. Who is talking when you comment on an issue: your
experience level stated plainly, what you are doing in this repo, what
readers can expect from you. This is the register your rules protect. -->

I'm a full-stack developer, new to open source. I pick issues I can actually finish, not ones over my head. My comments say only what I've actually checked, so maintainers can trust and review them quickly.

## Rules I write by

<!-- 3-5 rules, drafted from the lecture's slide-12 moment. Each rule
needs a wrong/right pair from your own hand: one line you might
actually have written that breaks the rule, and the line you would
post instead. The pair is what makes a rule executable; a rule without
one is a wish.

Format each rule like this:

### Rule: <short name>

<The rule, one or two sentences.>

- Wrong: "<a line that breaks it>"
- Right: "<the line to post instead>"
-->

### Rule: No promised timelines

I don't commit to a delivery time I can't guarantee. Reproduction and fixes take as long as they take.

- Wrong: "I'll have a fix up by tomorrow!"
- Right: "I've reproduced this and I'm working on a fix. I'll post an update once I have something to show."

### Rule: Name the environment and steps, not just the result

A claim that I reproduced (or couldn't reproduce) an issue and names the environment and steps I actually used, not just the outcome.

- Wrong: "Yep, I can confirm this happens."
- Right: "Confirmed on macOS 14.5 / Node 20.11, following the issue's steps — same error at step 3."

### Rule: Sound like I did the work, not like a template

My certainty matches how clearly I can walk through what I actually did, and I write it in my own words instead of generic AI-agent phrasing.

- Wrong: "I have thoroughly reviewed the reported issue and successfully reproduced the described behavior in accordance with the provided steps."
- Right: "Reproduced it — same error as the issue, steps below."

## Things I never post

<!-- A short list. Promises you cannot keep, tones you refuse,
shortcuts you know you reach for when tired. The skill quotes this
list back at you when a draft crosses it. -->

- A "reproduced" or "fixed" claim without the steps or artifact behind it.
- A timeline I can't guarantee ("by tomorrow", "this week").
- Generic AI-agent phrasing ("I have thoroughly reviewed...", "I appreciate the opportunity to contribute...").
- Anything I'm still unsure about, stated as certain. If I'm not sure, I ask the maintainer to clarify before I post or ask for review — not after.
