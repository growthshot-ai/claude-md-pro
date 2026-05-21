# The Plain-English Guide to Claude Code Discipline

*11 rules that turn your AI from a chatty intern into a focused builder. Written for anyone who builds with AI agents, even if you have never written a line of code.*

---

## Why this guide exists

A few nights ago, I asked my AI agent to build 3 things. It built 7.

The four extras were features I never asked for. A schedule I did not want. A batch mode that did not fit the system. Stuff the AI thought "would be nice to have."

I lost about 30 minutes and three full agent runs cleaning up the mess. Worse than that, I lost some trust in the tool.

I stopped and asked a simple question: *what is this, and how do I stop it?*

This guide is the answer. Eleven rules, in plain words. Four come from Andrej Karpathy (a top AI researcher whose ideas are widely shared on GitHub). Five are mine, learned from real long-session builds running voice, WhatsApp, and website AI agents for small and medium businesses. Two more are agent engineering principles widely discussed in the community: how the AI should run autonomously, and how it should fix its own mistakes.

Together they make Claude Code (or any LLM agent) about 10 times more disciplined.

---

## Who this guide is for

- You have used Claude Code or a similar tool and felt it "did too much."
- You are paying real money for AI tokens and want to spend less.
- You build with AI agents and watch them quietly go off-script.
- You do not need to know how to code. The rules are about behavior, not syntax.

---

## A few words you will see

- **AI agent.** A program that uses an AI model to do tasks for you (write code, summarize, plan). Like an intern, but faster and cheaper.
- **Token.** One small piece of text the AI reads or writes. About 4 letters. You pay per token.
- **Context window.** The AI's working memory for a session. Like your brain during an exam. It fills up.
- **Sub-agent.** A helper the main AI sends off to do a side job. Like sending a teammate to the library while you finish the report.
- **CLAUDE.md.** A small text file you put in your project folder. The AI reads it at the start of every session. Like a rulebook on the fridge.

---

## The three-part rulebook

Karpathy's 4 rules cover **what to do at the code level**. Line by line.

My 5 rules cover **how to stay honest over a long session**. Across hours, across sub-agents, across tasks.

The last 2 rules cover **how the AI should run on its own and recover from its own mistakes**. The autonomous-agent layer.

Stacked, they cover three layers. Read all three parts.

---

# Part A: The Code Rules (from Andrej Karpathy)

## Rule 1: Think Before Coding

The AI should ask before it acts when the request is unclear.

**Imagine this.** You walk into a doctor's office and say "I have a pain." A good doctor asks questions before writing a prescription. A bad one guesses and writes you the wrong pills.

Most AI agents are the bad doctor by default. They guess. This rule turns them into the good one.

**How to apply.** Tell the AI to state its assumptions out loud before writing code. If two readings of your request are both valid, the AI should name both and let you pick. If something is unclear, the AI must stop and ask.

**The rule.** *If the AI cannot defend a decision in plain English, it should not make it yet.*

## Rule 2: Simplicity First

Use the smallest amount of code that solves the problem. Nothing extra.

**Imagine this.** You ask a builder for a single shelf. They give you a full cabinet "in case you need more storage later." That is overengineering. You wanted a shelf.

AI agents love to overengineer. They add features "for flexibility" or "for the future" that you did not ask for and may never need.

**How to apply.** Tell the AI: no features beyond what was asked. No abstractions for code that runs once. No "configurable options" you did not request. If the AI writes 200 lines and the same thing fits in 50, it should rewrite the smaller version.

**The rule.** *Minimum surface area. Nothing speculative.*

## Rule 3: Surgical Changes

When the AI edits existing code, it should touch only what you asked it to touch.

**Imagine this.** You ask someone to fix a typo in one sentence of an essay. They come back having rewritten the whole essay "because they noticed some other things." Annoying. Now you have to re-read the whole thing.

AI agents do this constantly. They "improve" code that was not broken. They reformat. They rename variables they thought looked ugly. Every one of those changes is a new place for bugs to hide.

**How to apply.** Tell the AI: do not refactor adjacent code "for cleanliness." Match the existing style even if you would write it differently. If you spot dead code, mention it, but do not delete it without permission.

**The rule.** *Every changed line should trace directly to the user's request.*

## Rule 4: Goal-Driven Execution

Turn fuzzy tasks into ones with a clear "done" line.

**Imagine this.** You give a friend the task "clean the kitchen." Done means what? Wiped counters? Dishes washed? Floor mopped? Without a definition of done, your friend either does too much or too little, and you both end up frustrated.

AI agents need success criteria they can check themselves. Without them, they need you to look over their shoulder constantly.

**How to apply.** Tell the AI to translate vague asks into checkable ones. "Add validation" becomes "write tests for invalid inputs, make them pass." "Fix the bug" becomes "write a test that reproduces the bug, then make it pass." For multi-step work, the AI should write a brief plan up front with the check at each step.

**The rule.** *Strong success criteria let the AI work without constant clarification.*

---

# Part B: The Session Rules (from real builds)

These are the rules that catch the demons that haunt long agent sessions.

## Rule 5: Watch For Context Rot

The AI's working memory gets worse as the session fills up.

**Imagine this.** You are taking an exam. First hour, sharp. Third hour, you start mixing up names and dates. Fourth hour, you contradict your own earlier answers. Your brain has not lost information, it just cannot weigh it correctly anymore.

Same thing happens to AI. Around 70-80% of its memory full, it starts repeating old instructions or going against earlier decisions in the same session.

**How to apply.** At halfway through the session, save a summary of decisions made so far. At 80% full, force a "compact": shrink everything into a tight summary and restart fresh with that loaded. If the AI starts contradicting an earlier rule, do not argue with it. Compact.

**The rule.** *When the window is full, the model is half-deaf.*

## Rule 6: Do Not Hog the Window

Long outputs from sub-agents stay in the main session forever and cost you tokens on every future turn.

**Imagine this.** You ask a friend to research dolphins. They come back and read the entire encyclopedia entry out loud at every team meeting for the rest of the day. You only needed two sentences. Now every meeting is 20 minutes longer.

Sub-agents do this in AI sessions. A research helper dumps 6000 words into your main thread. From that moment on, every message the AI sends has to re-read those 6000 words. You pay tokens for them on every turn, forever.

**How to apply.** Make a strict rule: sub-agents return a one-paragraph summary, not their full output. Their full work goes onto disk (a file), not into the main session. If a long output sneaks in anyway, compact right away.

**The rule.** *If it does not change a decision in this turn, it does not belong in the window.*

## Rule 7: Restate Intent Before Any Sub-Agent Dispatch

When the main AI sends a helper to do a job, it must repeat your words exactly, not summarize them.

**Imagine this.** The school-yard telephone game. You whisper "Mike kicked the ball over the fence" to one kid. Six kids later it has become "Mike's bike is on fire." Each retelling drifts a little. By the end, the message is unrecognizable.

This happens between the main AI and its sub-agents. If the main AI summarizes your request before sending the helper, the helper gets a drifted version. The helper then drifts more in its own work. By the time you see the result, it is far from what you asked for.

**How to apply.** Every time the AI sends a sub-agent, the brief must open with two visible blocks: "User's exact words" (your message quoted) and "Out of scope" (a list of what we are NOT building). When the sub-agent returns, the AI must compare the draft against your original words. Anything not grounded in your words gets flagged to you as "agent's invention, not yours: keep or strip?"

**The rule.** *No sub-agent dispatch without restated intent.*

## Rule 8: Do It Now Or Write It Down

Ideas the AI mentions mid-session but does not save will die when the session ends.

**Imagine this.** You think of a great idea while walking. You say to yourself "I will remember this when I get home." You do not. By the time you reach home, the idea is gone. This happens to humans constantly. It happens to AI sessions even more.

The AI surfaces an insight mid-session ("I notice we should also handle X next time"). If it does not write it down, the session ends, the context window resets, and the insight is gone. You paid tokens for a thought that evaporated.

**How to apply.** Set a rule: whenever the AI surfaces an idea worth keeping, it has two options. Act on it in this turn. Or append one line to a persistent "promises" file before moving to the next message. The phrase "I will revisit this next session" is banned.

**The rule.** *An insight that does not touch disk is already forgotten.*

## Rule 9: Trust By Mechanism, Not By Promise

A rule that lives only in the AI's prompt can be skipped silently. You need a way to verify the AI followed the rules without having to ask the AI itself.

**Imagine this.** A small shop without security cameras. You ask the staff "did anything go missing today?" They say no. You have to trust them. A shop with cameras does not need to trust. It checks the footage.

AI agents are the staff. The audit log is the camera. If you build the rule into a system that records every action separately from the AI, the AI cannot lie about following the rule. The record exists outside its memory.

**How to apply.** Build a small "trust stack" of five layers:

- A LOUD rules file at the project root (this is your CLAUDE.md).
- A persistent memory that loads into every new session.
- A system-prompt extension that gets fed to the AI every turn.
- An external audit log (a small script that records every sub-agent dispatch separately).
- A one-word check you can run any time to audit compliance against the log.

If any one layer fails, including the AI skipping a rule, the next layer still catches the drift.

**The rule.** *Trust by mechanism, not by promise.*

---

# Part C: The Agent Engineering Rules

These two rules cover how the AI should operate when you are not standing over its shoulder. They borrow from a broader conversation in the AI engineering community about how to design agents that can run on their own.

## Rule 10: Run Autonomously, Escalate Only When Necessary

After you give the AI a clear brief, it should work on its own until the job is done.

**Imagine this.** You hire a senior chef. You give them the menu. You do not stand in the kitchen asking "is the soup ready?" every 30 seconds. They cook. They taste. They adjust. They serve. They only call you out if they cannot find an ingredient or a customer has an allergy you did not warn them about.

AI agents should work the same way. Most agents are the opposite of senior chefs by default. They ask "is this OK?" every two steps. That kills momentum and burns your time.

**How to apply.** Tell the AI: after the initial brief, do not ask for validation or progress checks. Test your own work. Diagnose your own failures. Loop until the success criteria are met. Escalate only when truly blocked: a missing piece of information, a real ambiguity, or a constraint that requires the user's input.

Put small guardrails around cost-sensitive actions: limits on API calls per loop, a cap on token spend per task, a log of every external call. These let the AI work alone without burning your budget.

**The rule.** *Come to the user only if you absolutely need to.*

## Rule 11: Self-Anneal. Convert Failures Into Updates

When something breaks, the AI should not just patch the symptom and move on. It should diagnose the root cause, fix the immediate breakage, AND update the system so the same failure cannot return silently.

**Imagine this.** Your body fights off a flu. Once it beats the virus, it remembers. Next time the same flu shows up, your immune system kills it faster. The body did not just "get back to normal." It got stronger than before.

AI agents should treat failures the same way. Each bug they fix should leave the system stronger than it was, not just back to normal. The same mistake should not return six sessions later because the lesson was never written down.

**How to apply.** Build the 4-step loop into how the AI handles failures:

- **Error.** Notice it.
- **Reason.** Diagnose the root cause, not just the symptom.
- **Solve.** Fix the immediate breakage.
- **Update.** Modify the rule, the prompt, the memory entry, the audit log, or the test that should have caught it.

The Update step is the one that gets skipped most often. Without it, the same bug returns over and over. With it, every failure makes your system stronger.

**The rule.** *Every failure earns a permanent update.*

---

## How to install (60 seconds)

1. Go to the GitHub repo (link in the DM you got).
2. Copy the file called CLAUDE.md.
3. Paste it at the top level of your project folder. Same level as your README or package.json.
4. That is it. Claude Code will load it automatically the next time you start a session.

If you already have a CLAUDE.md, add the 11 rules to it. They play well alongside project-specific rules.

---

## How to know it is working

- Your AI starts asking questions before doing things, instead of guessing.
- Diffs are shorter. The AI only changes what you asked it to change.
- Sub-agent briefs visibly contain "User's exact words" blocks.
- Your token bill on long sessions stops climbing for no reason.
- You catch drift through the audit log, not by re-reading the AI's work.
- The AI compacts the session itself when it senses the window getting full.
- The AI works longer between check-ins without losing direction (autonomy is on).
- Failure modes that used to repeat now become permanent rule updates instead (self-annealing is on).

---

## A small glossary, plain words

- **Agent.** An AI program with a task. Like an intern.
- **Sub-agent.** A helper the main AI sends out for a side job.
- **Context window.** The AI's working memory for one session. Has a fixed size.
- **Compact.** Squash a long session into a short summary. Like making a study guide.
- **Token.** One small piece of text the AI reads or writes. About 4 letters. You pay per token.
- **Audit log.** A separate file that records what the AI did. The AI does not write to this file. Something else does.
- **Drift.** The slow movement of the AI away from what you actually asked for.

---

## Credits

- The 4 code-level rules are based on public observations by Andrej Karpathy, an AI researcher widely respected in the field. A community compilation of his ideas lives at github.com/multica-ai/andrej-karpathy-skills.
- The 5 session-level rules are original work by Varuna Jain, founder of GrowthShot.ai (global) and GrowthSetu.ai (India). Drawn from real long-session Claude Code builds running voice, WhatsApp, and website AI agents for small and medium businesses.
- The 2 agent engineering rules are also Varuna's, building on principles widely discussed in the AI agent engineering community.

This guide is free to use, share, and adapt. Credit appreciated but not required.

---

## About the author

I am Varuna Jain, founder of GrowthShot.ai (global) and GrowthSetu.ai (India).

I help small and medium businesses grow revenue and cut costs through voice, WhatsApp, SMS, and website AI agents, smart-website builds, AI automations, and consulting and audits.

If you found this guide useful, LinkedIn is the easiest place to find me.
