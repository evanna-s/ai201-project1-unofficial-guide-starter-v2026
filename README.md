# The Unofficial Guide

<!-- Yi-Chen Shen, campus_life -->

> **This file is your submission.** Fill it in as you go — most sections get written during the milestone that produces them, not at the end.
>
> How the starter works, and every command you'll need, is in `RUNNING.md`. Leave that file alone.
>
> **Paste everything as text.** No screenshots, no video. A typed table gets full credit; a picture of the same table gets none.
>
> Delete these instruction blocks as you replace them. The `<!-- -->` comments are notes to you and don't show up when the page renders — you can leave them or remove them.

------------------------------------------------------------------------

# Unit 1

## What This Does

```{=html}
<!-- Three or four sentences. Which corpus you picked, and the kinds of
     questions your system answers. Write it for someone who has never seen
     this repo.

     Milestone 5. --> This project uses the campus_life corpus, a collecton of 88 short documents covering everyday answers about student life on campus. The system answers practical questions about campus logistics such as dropping courses, financial aids, and dining dollars. Each answer is grounded in retrieved passages form the corpus and cites its source document. When the question is out of scope, the system would declare: "I don't have enough information about that."
```

## Chunking Strategy

**Chunk size:** **Overlap:**

```{=html}
<!-- What about YOUR documents made you pick these numbers? Short posts and
     long sectioned guides don't want the same chunking, and "800 seemed
     reasonable" earns nothing. Point at something you noticed when you read
     the documents in Milestone 1.

     If you changed your mind partway through, say so and say why. That's worth
     more than pretending you got it right first time.

     Milestone 3. --> My corpus, campus_life, is 88 short documents averaging 317 characters, with the maximum length of 549. So I picked 590 as a ceiling as I do not wish to split my documents, ensuring no single-topic post gets cut in the middle. Overlap set as 0, because paragraph boundaries are already clean breaks. Those two parameters are too forward and plain for my corpus, I changed the code in Chunkers.py to further filter the contents.
```

## Sample Chunks

```{=html}
<!-- Five chunks, pasted as text. Label each one and name the file it came from
     AND the function that produced it — the grader checks your code against
     what you claim here.

     `python app.py chunks -n 5` prints all three for you. Copy them straight
     across.

     Milestone 3. -->
```

**Chunk 1** —

source: admin_add_drop_deadline.txt#0  \|  produced by: chunker.py::split_documents

```         
On the add/drop deadline

You can add a course through the end of the second week. Dropping is a longer window — through the end of week six — but a drop after week two shows as a W on your transcript. Nothing anywhere on the registrar's site says this plainly, and students find out from each other.
```

**Chunk 2** —

source: course_cs_340.txt#2  \|  produced by: chunker.py::split_documents

```         
The one piece of advice: start the term project in week three, not week eight; everyone learns this the hard way.
```

**Chunk 3** —

source: course_phys_130_workload.txt#0  \|  produced by: chunker.py::split_documents

```         
Workload for PHYS 130 Mechanics

People keep asking so: 7 hours a week, plus 3 on lab weeks. That's real time, not optimistic time.

It's front-loaded — the first month is heavier than the rest, partly because you're learning the format.
```

**Chunk 4** —

source: dining_verrill_street_grill.txt#1  \|  produced by: chunker.py::split_documents

```         
Hours are 11:00am to 1:00am daily during term. Costs declining balance, or cash after 11:00pm.
```

**Chunk 5** —

source: housing_morrow_house_laundry.txt#0  \|  produced by: chunker.py::split_documents

```         
Laundry in Morrow House

Machines take $1.50 wash, $1.25 dry, coin or card. There are eight washers and six dryers for the building, which is the wrong ratio and means the dryers back up on Sunday evenings.
```

## Sample Answer

```{=html}
<!-- One complete question and answer, pasted as text, with the source line
     visible. Milestone 4. -->
```

**Question:**

Does dining dollars balance roll over from spring to the next fall semester?Does dining dollars balance roll over from spring to the next fall semester?

**Answer:**

```         
No, dining dollars do not roll over from the spring semester to the following autumn; whatever is left in May disappears. 

Source: admin_dining_dollars.txt

Sources retrieved: admin_dining_dollars.txt, admin_meal_plan_changes.txt, dining_north_kitchen.txt, money_jobs.txt
```

**My relevance cutoff:**

```{=html}
<!-- The number you set in config.py, and how you got there.

     You ran five questions your corpus covers and the five in OUT_OF_SCOPE
     that it clearly doesn't, and wrote down the best distance for each. What
     did those two groups look like? Where was the gap? Put the actual numbers
     here — the table below wants all ten rows.

     Milestone 4. --> I agree with the original 0.6 cutoff, I got this by testing my 5 in-corpus questions against the 5 out of scope questions. (TMI: For top-k, I kept it at 5 because I tried changing it to 4 but the chunks weren't significantly different, only reducing a little noise. So I would reckon that the root cause probably wasn't top-k.)
```

| Question | In corpus? | Best distance |
|----|----|----|
| Does dining dollars balance roll over from spring to the next fall semester? | Yes | 0.172 |
| What will happen to my financial aid if I choose to do a study abroad program? | Yes | 0.219 |
| When would student permits for the west lots go on sale? | Yes | 0.298 |
| When should I book an advisor for registration? | Yes | 0.375 |
| What's the format of ECON 101? | Yes | 0.399 |
| What is the capital of Mongolia? | No | 0.787 |
| Who won the 1994 World Cup? | No | 0.847 |
| What is the recommended dosage of ibuprofen for a headache? | No | 0.824 |
| How do I write a for loop in Rust? | No | 0.877 |
| How do I change the oil in a diesel engine? | No | 0.923 |

According to the numbers above, I could tell that there's a clean gap between the two groups: in-corpus questions ranged from 0.172 to 0.399, out-of-scope questions ranged from 0.787 to 0.923. The reason why I kept the default cutoff of 0.6 is it sits almost exactly in the middle of that gap.

## How I Used AI

```{=html}
<!-- Two specific moments. For each: what you asked for, what came back, and
     what you changed about it.

     "I asked Claude to write the chunking function from my notes. It ignored
     the overlap, so I added that myself" is the level of detail we're after.
     "I used AI to help me code" is not.

     Milestone 5. -->
```

1.  First, while I was running my codes and going through milestones, I wonder if every single time I run code in my terminal, it reflects my changes in the code that I modified just now. So I asked Claude that and Claude states that as long as I save the file, every time I run something in my terminal, it's using my newest code in the directory. So I remember to save my code as I move forward.
2.  I gave Claude five chunks of mine and asked whether all of them could be understood alone and is there anything I should revise. Claude says those looks fine but there's still minor issues about the "it"s, one may not understand what it refers to without context. So I go ahead and improved my Chunker.py.

```{=html}
<!-- ── Stretch features ─────────────────────────────────────────────────────
     Doing one? Say so here BEFORE you start. A feature this README never
     claims earns nothing.
     ───────────────────────────────────────────────────────────────────────── -->
```

------------------------------------------------------------------------

# Unit 2

```{=html}
<!-- These sections get ADDED to what's already above. Don't delete or rewrite
     unit 1 — the point is that someone can see what you said before you knew
     how it went. -->
```

## Run Log — Before

```{=html}
<!-- Your five criteria, three runs each. `python run_eval.py --label before`
     runs the questions, puts the OUT_OF_SCOPE ones through the gate, and
     writes it all into results/ for you. Targets come from criteria.md; the
     verdict column is your call.

     Criterion 3 is measured in one deterministic pass rather than three, so
     the same number goes in all three run columns. That's correct, not lazy.

     Milestone 1. -->
```

| Criterion                               | Target | Run 1 | Run 2 | Run 3 | Verdict |
|-----------------------------------------|--------|-------|-------|-------|---------|
| 1\. Retrieved chunk contains the answer | 4 of 5 |       |       |       |         |
| 2\. Every answer names a source         | 5 of 5 |       |       |       |         |
| 3\. Gate stops out-of-corpus questions  | 4 of 5 |       |       |       |         |
| 4\.                                     |        |       |       |       |         |
| 5\.                                     |        |       |       |       |         |

```{=html}
<!-- Underneath, paste the REAL output for each criterion from one of your
     runs — the actual text your system produced, not a description of it.
     Name the file and function that produced it. -->
```

## Verdicts

```{=html}
<!-- MET or MISSED for each of the five, against the target you wrote last
     unit — not a new one. Plus a sentence on how you decided. That sentence
     matters most where it was close.

     If your target said 4 of 5 and your runs came out 4, 3, 4, that's a MISS.
     The target has to hold, not show up occasionally.

     Milestone 2. -->
```

| \#  | Criterion | Verdict | How I decided |
|-----|-----------|---------|---------------|
| 1   |           |         |               |
| 2   |           |         |               |
| 3   |           |         |               |
| 4   |           |         |               |
| 5   |           |         |               |

## Diagnoses

```{=html}
<!-- For each miss: which stage caused it, and how. The stage alone isn't
     enough — you need the mechanism.

     Not a diagnosis: "Question 3 didn't work."
     A diagnosis:     "Question 3 asks about laundry costs. The answer is in
                       one sentence that got split across two chunks, so
                       neither chunk on its own contains it."

     The five stages: loading → chunking → embedding → retrieval → generation.

     Look for a pattern. If three misses all ask about numbers, that's one
     problem, not three.

     Missed nothing? Say so, then say honestly whether your targets were set
     low, and which one you'd tighten and to what.

     Milestone 3. -->
```

## The Improvement

**What I changed:**

**Why I picked it:**

```{=html}
<!-- Connect it to a specific diagnosis above in one sentence. If you can't,
     you picked a fix because it sounded impressive. -->
```

### Run Log — After

```{=html}
<!-- Same format, same five criteria, three runs each.
     `python run_eval.py --label after` -->
```

| Criterion                               | Target | Run 1 | Run 2 | Run 3 | Verdict |
|-----------------------------------------|--------|-------|-------|-------|---------|
| 1\. Retrieved chunk contains the answer | 4 of 5 |       |       |       |         |
| 2\. Every answer names a source         | 5 of 5 |       |       |       |         |
| 3\. Gate stops out-of-corpus questions  | 4 of 5 |       |       |       |         |
| 4\.                                     |        |       |       |       |         |
| 5\.                                     |        |       |       |       |         |

**Did it help?**

```{=html}
<!-- Say plainly whether it did, and how you know. If it made things worse,
     say that — a change that backfired, honestly reported, earns full credit
     and is more interesting than one that worked. What matters is that you can
     tell.

     Milestone 4. -->
```

## What's Still Broken

```{=html}
<!-- For each criterion still missed after your fix: what you'd do about it,
     and why you stopped where you did.

     "I ran out of time" is fine if it's true. Pretending nothing is left is
     not.

     Milestone 5. -->
```

## What I'd Do Differently

```{=html}
<!-- Knowing what you know now — which of your five criteria would you write
     differently, and why?

     Milestone 5. -->
```
