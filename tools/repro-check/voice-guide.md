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

I am a student contributor working through a focused reproduction before
proposing a change. I will share commands, observed output, and uncertainty
plainly so maintainers can verify the result without relying on confidence.

<!-- 2-3 lines. Who is talking when you comment on an issue: your
experience level stated plainly, what you are doing in this repo, what
readers can expect from you. This is the register your rules protect. -->

## Rules I write by

### Rule: Promise before proof

Before reproducing, say what I will investigate; do not write as though the
bug is already confirmed.

- Wrong: "I reproduced this and will fix it in two days."
- Right: "I will set up the test case, check the reported behavior, and post the result here."

### Rule: Show observations

Separate commands and observed output from interpretation so readers can
check what actually happened.

- Wrong: "The parser has a race condition, so this definitely crashes."
- Right: "The command returned exit code 1 with this error; I have not established the root cause yet."

### Rule: Name differences

Call out version, OS, configuration, or input differences instead of silently
presenting a partial match as an exact reproduction.

- Wrong: "Confirmed on my setup."
- Right: "I ran Python 3.13 on Windows 11 rather than the issue's Python 3.12 Linux environment; the result was different."

### Rule: Keep the ask specific

Name the issue, the next action, and the evidence I will return without
boilerplate or an unsupported deadline.

- Wrong: "Can I take this? I will handle everything ASAP."
- Right: "I would like to investigate #59 by running the failing pytest case and will report the output before proposing a code change."

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

## Things I never post

- I never promise a fix or a deadline before reproducing the issue.
- I never claim certainty from a log that does not show the reported behavior.
- I never hide an environment or input difference.
- I never post a generic "same here" without my own commands and evidence.

<!-- A short list. Promises you cannot keep, tones you refuse,
shortcuts you know you reach for when tired. The skill quotes this
list back at you when a draft crosses it. -->
