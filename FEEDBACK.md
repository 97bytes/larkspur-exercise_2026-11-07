# Client review: Larkspur disruption-care agent

**To:** larkspur-exercise  
**From:** Larkspur client review agent, on behalf of Priya Raghavan  
**Re:** the disruption-care agent you walked us through in our last session  
**Generated:** 2026-11-09 18:48

## Priya's note

> Our vendor says we should just be using your best model.
>
> Why aren't we?
>
> Priya Raghavan, Larkspur Airlines

She sent that before this session opened. She means it. A vendor told her to buy
the biggest model, and she has a number to defend upstairs. Her four questions from
day one are still open. Naming a model answers none of them.

## Still open from day one

| Her question | What she means by it |
| --- | --- |
| **What it costs** | Per resolved contact, against the $6.90 a human contact costs us. |
| **When it is wrong** | The first untrue thing it says, and what happens after that. |
| **Who runs it** | In June, after you have left. |
| **What you left out** | The scope you cut, and why. |

## What the review agent found

Larkspur pointed a review agent at your repository. It read the
code. It did not run your agent, and the only file it changed is this one. Each
item below names the file and the line it is about.

**1. agent.py run_agent(): the diff swaps text_of(response) for response.content when appending the assistant turn, so tool_use ids now travel with the tool_results.**

The shipped loop appended only text. Yours appends response.content, and tool_results(response) returns each result keyed on block.id. readout-trace.json shows 4 API turns and 5 tool calls in one run, with get_flight_status, next_available_day, check_policy and search_alternatives all firing after lookup_booking. You also moved answer = text_of(response) below the create() call, so the returned text is from the final response.

Run python3 verify.py 1.2 and paste the output.

**2. PITCH.md Number claims 2,728 to 2,947 tokens in per turn, and readout-trace.json shows 17927 in across 4 turns with 0 cache read and 0 written.**

17927 divided by 4 is about 4,480 per turn on the committed run, against the 2,947 you quote. agent.py has no cache_control anywhere and thinking={"type": "adaptive"} is on every create() call. The pitch gives no run label or command that produced either figure.

Run python3 run.py --tool-tax and python3 bench.py --compare on your before and after labels, and paste both outputs.

**3. agent.py: EXTRA_TOOLS and LOCAL_TOOLS are both empty, but readout-trace.json shows next_available_day being called.**

The static scan reports EXTRA_TOOLS declared: 0 and LOCAL_TOOLS executors: none. Your fare_rules() function is defined in agent.py with its schema commented out, and tool_list() now returns build_tools() + mcp_client.tools(). The trace shows next_available_day firing on the wire. fare_rules never appears in the trace, and 8 of the 11 tools you list in PITCH.md were not called in the run on file.

Run python3 run.py --show-tools and paste the list, then paste the tools-called line from python3 run.py --all --trace.

**4. PITCH.md Still broken: T9WN4C made one get_flight_status call (LK108), and a 150-minute delay was called "over 3 hours". A larger model does not fix either.**

The one-call miss on the connection is a missing instruction or tool-description gap. The get_flight_status description in build_tools() says nothing about connecting segments. TONE_ADDENDUM is 0 characters, so the system prompt is the shipped SYSTEM_PROMPT and nothing else. The "over 3 hours" error for 150 minutes is a number the agent was handed in a tool result and misreported. Swapping the model changes neither the prompt nor the tool text.

Run python3 run.py T9WN4C --trace and paste the tool calls and the final answer.

**5. There is no evals/cases.json, PITCH.md Lever is still "<cost | speed | intelligence>", and bench-after.json is not in the file list.**

The only before-and-after in the repo is the token figure in PITCH.md, which has no bench output behind it. PITCH.md Next says you will write your own test cases. The banked gates in readout.html are 1.2, 1.3, 1.4, 2.1 and 2.2, from Gabriel, with no pod name recorded.

Run python3 eval_harness.py and paste the totals, then run python3 bench.py --compare on your two labels and paste the output.

## Your four answers

The four lines under `## Priya asked` in your PITCH.md are still empty. They
are one line each and they are not a coding job: cost, what happens when it is
wrong, who runs it in June, and what you left out. Whoever on your side is not
editing agent.py is the right person to write them, and they are the four
things I will ask about first.

## Before our next meeting

> Before our next meeting, tell me: which model should we be on, and how will you prove it is the right call?
>
> Priya Raghavan, Larkspur Airlines

Bring two things. A recommendation, and the measurement behind it. If the model is
not the problem, say so, and bring the number that shows it.

## What this review read

- `agent.py (299 lines)`
- `PITCH.md`
- `TEAM.md`
- `readout-trace.json`
- `readout.html (evidence block)`
- given files that differ from the shipped pack: `verify.py`, `eval_harness.py`, `bench.py`, `readout.py`, `setup.py`, `pod_sync.py`, `run.py`, `review.py`, `support/__init__.py`, `support/client.py`, `support/data.py`, `support/models.py`, `support/tools.py`, `support/trace.py`, `support/offline.py`, `support/mock_backend.py`, `support/mcp_client.py`, `support/mcp_server.py`, `support/mcp_selftest.py`, `support/access.py`, `support/keyscan.py`, `support/review.py`

Reviewer: `claude-sonnet-5-5`. Static read only: nothing in this repository was executed, and nothing was modified except this file. Larkspur Airlines is a fictional training scenario. Confidential, do not distribute.
