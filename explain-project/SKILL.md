---
name: explain-project
description: Introduce a project, codebase, module, or system in the style of a design report. Start with background and why it matters, then walk through the core design ideas one section at a time (each as tension, approach, example, figure), instead of listing directories, files, classes, and functions. Use this whenever the user wants to understand a project, for example "what is this project", "what does this repo do", "walk me through this system", "explain the design of this module", "what are the core ideas here", "onboard me", "give me an overview of this codebase", "explain this architecture", even if they never mention a report or a style. Also use it to rewrite an existing technical explanation so it is easier to follow. Not for questions about one specific line or function, debugging an error, or code review.
---

# Explain a project in the style of a design report

## The problem this skill solves

The easiest mistake when introducing a codebase is to follow the way the code is organized: the directory tree, the file list, the classes and interfaces, the constants. That makes a good index for someone who already understands the project. For someone trying to understand it, every sentence carries several terms nobody has explained, and none of it answers why things are built this way. The reader has to ask about each one.

A good introduction follows the order in which a reader comes to understand something: why this needs doing, where the difficulty lies, what trade-offs the authors made, and what the result looks like. Afterwards the reader can explain the project's ideas to someone else without remembering a single file name.

Compare two ways of describing the same design:

> **Following the code:** "Intake stores the Trigger and fans it out to the tickets subscribed to its Thread; if no ticket's worker owns it, it is queued for Eval."
>
> **Following the reader:** "If every follow-up message of a task had to pass through the dispatcher, that single global node would soon be overloaded. So the system treats the messages under one pull request or one chat topic as a continuous conversation. Once a worker takes a conversation on, it subscribes to it, and new messages there go straight to that worker. If you add 'please fix the comments too' on the pull request, the worker simply carries on, and the dispatcher is not involved."

The second version is longer, but it leaves the reader with nothing to ask.

## Before you write

The quality of the introduction depends on whether you actually understand the project, so read first and write second.

1. **Read the code, not only the docs.** The README and design documents describe what the authors meant to build. The code is what they built, and the two often differ. Read at least the entry point, the core data structures, and the main flow yourself.
2. **Find the three to six core designs.** The test is: take it away, and this is no longer the same project. Group by the problem each one solves, not by directory. One directory may hold several designs, and one design may be spread over several directories.
3. **Work out the tension behind each design.** For each one, ask yourself: what is the most naive way to do this, and where does it break? If you cannot answer, you have not understood it yet, so go back and read more.
4. **Look for one thread that runs through everything.** A good project usually has a starting premise that every design traces back to. Find it and put it at the end of the background section. The later sections then share a common reason.
5. **Know where the project sits in its field.** What change in the industry is it responding to? How does it compare with the mainstream approach? Your own knowledge may be out of date, so search for recent developments when you have a search tool. If you find nothing, say only what you are sure of and do not invent a trend.
6. **If the project comes with a paper, blog post, or design document, read it and compare it with the code.** Note what the document describes that the code does not contain.

## Structure

### 1. Background and why it matters

Start from a situation the reader already knows and move step by step to this project:

- How people do this today, and what has changed recently. Connect it to the current direction of the technology where you can.
- Where the current approach gets stuck. Make the bottleneck concrete, ideally with one fact that anyone grasps at once, such as "a person can follow only three to five active sessions at a time".
- What this project sets out to do, who it is for, and what using it looks like.
- Its starting premise or central claim, in one or two sentences.

Then give one overview figure and one sentence of signposting that says what each later section covers and which goal it serves. The reader gets the map before the details.

### 2 to N. Core designs, one per section

Build every section in the same four steps:

1. **Tension:** the problem or trade-off this section resolves. Spell out what is wrong with each naive approach, so the reader sees the solution as necessary and not arbitrary.
2. **Approach:** what the project does about it. If it borrows an idea from an established field, name that idea (see "Analogies" below).
3. **Example:** one concrete scenario that shows the design at work.
4. **Figure:** a simple diagram, when it is clearer than words.

Order the sections by dependency. A later section may use a concept that an earlier one introduced, never the other way round.

### Optional closing sections

Include the following only when the project has them. Do not add a section just to make the structure look complete:

- The role of people in the system, or how it is used.
- Experiments or performance figures. Quote only what the project itself reports, along with its conditions.
- Open problems the authors themselves acknowledge.
- Scope: whether this is a prototype or a mature product. If the documents describe something the code does not implement, say so in a sentence or two and do not elaborate.

## How to write it

All of these serve one purpose: the reader should not need to ask a follow-up question.

**Introduce a concept when it is first needed.** A term appears where it is first useful, with one sentence saying what it is. Do not open with a glossary, and do not use a term before explaining it. Keep any one sentence to at most two terms the reader has not met.

**Name things by their role, not by their identifier in the code.** Say "the dispatcher", "the worker", "the queue of outgoing messages", not class or function names. If the project's own documents use a common name such as Router or Worker, you may use it, as long as you say what it does the first time.

**Use analogies to what the reader already knows.** Many designs carry a classic idea from an established field into a new setting. Pointing that out lets the reader understand at once: "it manages the context window as a cache", or "it tries the work in the background, delivers it if it turns out useful, and discards it otherwise, which is speculative execution". The analogy has to fit. A forced one is worse than none.

**Keep file paths, class names, function names, and configuration keys out of the main line.** They interrupt the reasoning, and the reader cannot use them yet. Give them when the reader asks how something is implemented.

**Use few numbers, and only persuasive ones.** "Replies are capped at 100 words" helps the reader see the intent of a design and is worth keeping. Buffer sizes and timeout values usually are not.

**Make examples concrete enough to picture.** "A user asks for plan A in chat and later switches to plan B in a pull request comment" tells the reader far more than "supports context continuity across channels".

**Keep figures simple.** One figure has three to five boxes and makes one point. Each box holds a role and a short phrase about what it does, never a class name. Draw one overview figure, and add another only where a design is clearly easier to see than to read. Use a diagramming tool if you have one. Without one, leave the figure out and do not draw elaborate ASCII art.

**Use tables only for side-by-side comparison,** such as the difference between two modes or what each of several settings means. Write reasoning and cause and effect as paragraphs.

**Length.** An overview usually runs from about 800 to 2,000 words in English, in five to eight sections. Beyond that the reader loses the overall picture, so drop a minor design before you go longer.

**Answer in the user's language.**

## Honesty

- Present as "what this project does" only what the code actually contains.
- Put anything the documents describe but the code lacks into the scope note, in a sentence or two.
- If you have not verified something, verify it or say plainly that it is an inference.
- Do not overstate. Call a prototype a prototype, and call an experiment on a synthetic workload exactly that.

## When the reader asks follow-up questions

After the overview, readers often ask about one specific mechanism: "what does this sentence mean?" or "how does the code do that?". Now code-level detail is welcome, including files, functions, and exact values. Keep the same order, though: say in a sentence or two what problem the mechanism solves, then describe the implementation, preferably by walking through one concrete example.

If a follow-up shows that the overview left a concept unclear, that passage broke the rule about introducing concepts when they are needed. Fix it the next time you write an introduction.

## Check before you deliver

- Go through every term in the text. Is each one explained where it first appears?
- Does each section open with the problem, or does it jump straight to the solution?
- Does each section have a concrete example?
- Are there still file paths, class names, or function names in the main text? If so, move them out.
- If you read only the first paragraph of each section, do they add up to a complete story?
- Is any section there because the code has a directory for it, and not because it is a core design?
- Is every design you describe really in the code?
