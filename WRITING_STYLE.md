# Writing Style

How posts on this blog are written, based on the English posts in `blog/content/posts/`.

Posts are an engineer's record of what happened or what to remember. Each sentence carries a fact, a step, or a result. The reader is another engineer, or me six months from now.

## Evidence

Counts cover post text with code blocks removed. `sporter` and `useful_webtools` are left out because they read differently from the other posts: they are the only posts with em dashes. Whether they should count as the current voice is still undecided.

| Pattern | Count |
|---|---|
| Inline code spans | 183 |
| Bold phrases | 81 |
| `->` | 31 |
| `Now` | 24 |
| `ex)` | 23 |
| `a lot of` | 15 |
| `However` | 9 |
| Semicolons | 4 |
| Capitalized `But` | 0 |
| Em dashes | 0 |
| Numbered headings | 121 of 241 |

## Pick a Post Type

| Writing about | Type |
|---|---|
| A work project with production results | Case study |
| How I built or set up something | Build log |
| A concept, paper, or tool to look up later | Study notes |

## Voice

### Openings

Start at the situation: what I wanted, what I was doing, or what the problem was.

> I want to create a RTS-Style commander view in UE5 that will make it easier to control multiple units.

> In my recent project I had to optimize handling 75 TPS

### Endings

Stop at the last fact. A post ends on the final result, the working test, or the remaining to-do list. Posts don't close with a takeaway. The one `# Conclusion` heading, in `rtmdet1`, holds the remaining work.

> Now our animation blueprint is ready. Set it to our character.

> After extensive tuning we were able to serve our model with 250TPS with only 4 V100 GPUs.

### I and We

Use "I" for decisions and "we" for the steps: "I want to...", then "Now we add...". In work posts, "we" means the team.

### Sentences

- Explanations run long and plain, chaining clauses with so, because, which, since, and while.
- Instructions and results run short.
- Sentences connect with Now, So, However, Since, and Also.

> All of our models are small models with parameter size less than a billion, so these models, if used individually, weren't big enough to utilize one V100 to maximum capacity, meaning a lot of computation power was being wasted assigning one model for each GPU.

### Word Choice

- Plain intensifiers: "very good", "a lot of", "way better".
- Honest hedges: "It's probably because", "seems simple", "Surprisingly".
- Exact numbers, with the baseline when there is one: "from 35% to 80%", "x10 faster", "25 Million image requests per day".

## Formatting

### Headings

Headings are Title Case and name the action or topic.

- Number sections that run in sequence: `# 1. Create Animation Blueprint`, `## 2-1. Create a DecalComponent Class`.
- Use `# 0.` for background before the first step.
- Use plain headings for posts organized by topic: `## Limitations:`, `# Reason For Bug`, `# Fix`.

### Emphasis

Bold whole clauses that hold the key finding, not labels:

> The model **cannot use the given pawn's status by itself, because it works in a different thread.**

Put code names, settings, menu paths, and defined terms in backticks: `Edit->Project Settings->Collision->New Trace Channel`.

### Punctuation and Shorthand

| Use | For | Example |
|---|---|---|
| `ex)` | An immediate example | `ex) a photo of meal with title 2025/04/03` |
| `->` | Consequence or sequence | `press 1 -> speak -> release 1` |
| `x3` | Multipliers | `speed up our image operations by x3` |
| `>>>` | Strong comparison | `Data Quality >>> Data Quantity` |
| `...` | Closing an open list | `(standardization, letterboxing, ...)` |
| `(` with no space | Parentheticals | `geospatial data(mostly .shp files and point clouds)` |
| ` - ` | Joining a clause | `We can solve this using classmethods - we can use classmethod to call the class initializer` |
| `:` | Introducing a list | `There were two challenges:` |

### Lists and Tables

Show lists as bullets or numbers. Number procedures, and keep comparisons as bullets. Posts don't use tables for comparisons.

## Post Types

### Case Study

Past tense and mostly prose, with bullets for constraints and options. This is the longest type.

1. Business context in a few plain sentences.
2. One sentence on my role.
3. `# 0.` for what makes the problem technically hard, if needed.
4. Project stages in order: what we needed, what blocked us, what we decided, and what number came out.
5. Failed attempts stay in, with the reason they failed.
6. End on the production result.

Common moves:

- Name limits as a short list: "There were two challenges:"
- Reduce problems to one bold diagnosis: "To summarize, our main problem was that **Our teacher model wasn't "teaching" good enough.**"
- State the decision with "So": "So we decided to take it slow..."

### Build Log

Present tense, short, and led by screenshots and code. Most steps are one or two lines.

1. The goal in one or two sentences, with a familiar comparison if useful ("Just like Starcraft").
2. Numbered steps, each headed with an action.
3. Under each step, a short instruction followed by a screenshot or code.
4. Bugs stay in the flow: the problem, the guess, then the fix.
5. End on the test or working state.

Common moves:

- Write bare instructions: "Set skeleton to `Yin`", "Open the animation blueprint."
- Repeat with fragments: "Same for Jog."
- Introduce bugs plainly: "There's one problem:"
- Mark guesses: "It's probably because **I gave the units vector movement instead of AI navigation.**"

### Study Notes

The tersest type, written for lookup. Fragments are normal and articles may drop out.

1. A one-line definition or purpose, or go straight to the first heading.
2. Topic headings, which can be bare terms.
3. Under each term, a one-line definition and nested bullets for details, examples, and tradeoffs.
4. Stop after the last item. Unfinished sections are fine.

Common moves:

- Define in a fragment: "Transfer big model's knowledge to smaller model"
- Fit a tradeoff on one bullet: "Post Training Quantization: perform on an already trained model - simple but more performance loss"
- Follow a claim with its consequence: "Very useful, but also costly -> if the class must have the component, **declare as a member function instead of doing this dynamic search.**"

## Before Publishing

1. The first sentence states the situation.
2. Headings are Title Case, numbered when sections run in sequence.
3. The last line is a fact, result, or to-do.
4. Numbers are exact and come from the real project.
5. Typos and grammar slips are fixed, and the wording stays plain.
