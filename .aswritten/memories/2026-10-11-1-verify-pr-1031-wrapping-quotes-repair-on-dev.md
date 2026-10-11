---
reviewers:
  - xena+staging@aswritten.ai
---

# Steering is ours, recall is an engine's job: the frame-index recall tests and the shoulders-of-giants rule

Date: 2026-10-11, the narration-context session, epic parent for task-766 (The Frame Index). Present: Scarlet Dame and the session. This memory extends the 2026-10-08 ruling that frames are the index and narrated digests retire (memory 2026-10-09-1). Nothing here is exported to other sessions as a build input until the design is tested; Scarlet's 2026-10-09 rule.

## The question the day started on, and her slowing-down

The 10/08 ruling had been passed to other sessions as rulings. Scarlet stopped that: "we need to slow down. This is a new conversation, so previous things I said, like this ruling on 766, I don't think are something that we should be passing off to other sessions yet. If this session is going to do the core, it needs to do the full design and then test it before we start implementing it in ad hoc places around the codebase."

She then set the format of the index in her own terms:

> I would expect it to be multi-tiered, right? An actor hub wouldn't include everything that that actor said. The actor hub would include a link to all of the calls that they were in, all of the narratives, and all of the positions, right? It is one level down, and you'd see the frame.

> You want to tightly control the unit of expand. What I don't want is, if you're trying to get a call, the unit of expand is the call, right, or the memory. You don't want to get a single position. I might even go so far as to say that you don't want to let a single position be expanded. You want it always to be under a narrative or call or another door, because, to the point of steering, not retrieval, we don't want to just return a single position. We want to return the context around it.

> If you list the call with its IRI, then you would list all of the positions underneath it as frames with no IRI, right? It can just expand the call. The same with narrative: you list the narratives with IRI, and then everything underneath it as just frames.

And on what a page must carry: "if I'm looking at a document, I should see everything, fully expanded: narratives, positions, the whole graph inside that document, and the edges that would lead me elsewhere and how to get to them. Actors, narratives, the strata up, and other positions outside the document that do not include the statements. … I want to see the full package and its edges at the navigational fidelity." On size: the MCP page stays 15K; "what you really don't want to have is dump navigation to file, because that breaks the ability for the agent to understand the whole. Dumping the root to file breaks the structure of the navigator, but I think dumping a large document to file doesn't." A long call is one expand result and may spill whole past the bell curve; there is no split.

On the dead tier the sizing found (561 empty narratives, 952 unframed, 29% of statements in no position): "most of the weirdness in that breakdown sounds like a 355 re-extraction would fix almost all of it. If we fix strata and then do a full re-extraction, wouldn't that change very substantially?" Tentative: "narrative then shows calls as the unit of expansion, with frames under them." On a name: "Let's call it frame-based indexing or something. I don't know. You got a better name for it than that." Still open.

## The order every reader follows

> You need steering first, and then you need the recall of the full set of things to consider, and then applying the result of steering, the navigated context, to the recalled choices becomes precision.

Applied to every consumer, including the room agent, the judgment passes ("the decision on how to frame, how to interpret a frame, is highly context dependent") and cite:

> The graph contextualizes meaning through strata, narrative, position. And so statements out of that context lose their context and the ability for the agent to infer or to read their direction. A statement is always support for a direction, not an end in itself. Taking a statement out of context is exactly that, as opposed to using a statement as support for the position, narrative, strata direction that it is underneath and applying that to the document. That actually might not be quite what the cite prompt says and should probably get ticketed and looked at as part of the read pass epic.

Filed as task-767 under the read-path epic.

## The frame is the lens, not the carrier

> I don't think frame plus a selection of statements is ever a good design or was ever a good design. A frame is not designed to be the carrier of meaning. The frame is designed to be the frame through which its statements are read and that combination produces meaning, which is why the narration is arguably the result of that. I think that the frame only is unquestionably losing something. The question we're trying to answer is exactly that: can you use the frame only to locate whether something is relevant? And our answer is maybe.

Consequence: a judge, or any reader deciding relevance, sees a position whole, frame and every member statement, never a sample of three.

## On the reference and the tests

The first tests scored against the 41 positions production's own runs had placed. She rejected that as the target: "that seems like a pretty questionable metric to be running against, right? Because isn't our entire stance here that production isn't doing it right? So saying that we're trying to replicate production doesn't make any sense to me." The judged set that replaced it, in her words: produce exhaustive recall, "then do a precision pass over it and throw out all the things that are obviously not relevant and make picks and say why and have some ambiguous ones noted and have other ones that are closely rejected still in the list so that they can be looked at, and have this done by a couple different models … do this set with the big models and then have the small models' judgments overlaid"; a big model must also take an arm in the exhaustive pool, because "trusting the small models there is something we want to do by confirmation, not faith." She reviews "a list of dozens of rows" and agrees or disagrees with placements. On whether the pool was exhaustive: "are you sure that everything that might be relevant is being included? And what's the mechanism for that?" It was not; a complement pass over every unpicked frame, judged whole, was added. On 82 to 88 percent: "that to me is pretty bad. What are you actually missing and how do we understand what the gap between that number is and full recall?"

On output caps: "I do not understand why builders are constantly setting output caps on really anything. If you want a shorter reply, put it in the prompt, but an output cap, if anything, should be solely for runaway behavior and it should be like a circuit breaker. If you're using one amp of electricity, the circuit breaker is at 15 amps. It's designed to never flip unless something bad happens." Now a rule in `.claude/rules/llm-calls.md`.

On supersession and the unit of expand: "isn't our approach in the frame index that the unit of expand is the transaction? Which means that if you find any transaction sibling, you would always end up with the full set, right? The danger there is probably too much data." Recall was re-scored at the unit of expand: every arm reaches more, and narrative pages alone cost 6 to 14K tokens per document while call pages are 90 percent of the weight.

## Session findings (measured on our own graph, scored first against production's floor, then against the judged set)

Frames alone, read by a cheap model in one call over the whole frame index (about 49K tokens), recovered 26 to 27 of the 41 positions production had placed; the navigator after full expansion recovered 23; with today's navigator's picks handed in as direction, Haiku reached 34, and three of its eight gains were not on the direction list. By unit of expand, 33 to 37 of 41. On other document shapes, memos and a voice memo read well and a two-person call transcript read weakly. GLM 5.3 flash ran away when uncapped and lost calls; Haiku 5.5 never hit a limit. The judged set on three documents found production's targets recover only 5 of 17, 1 of 34 and 2 of 16 of what two judges agreed was relevant, so the floor was a poor reference. Program spend $1.41 for the recall tests and $3.00 for the judged set.

## The governing split and the rule

> Rolling our own perspective load, the steering portion, makes sense because that's novel. But I think part of our issue is that that same engine got used for recall inside all of the consumers that we listed out above. And separating the two could give us continued steering that we do and a query engine that does recall and precision.

> Re-implementing or creating a new database is a generally known stupid thing to do unless what you're trying to do is build a database. And I feel like we've come to do that a bit by accident. Storing RDF in the repo was never meant to implement a new query and recall engine, right? It was just a store that we were gonna run an in-memory database over, i.e. Apache Jena or something related. And I feel like now we're running into this recall problem because we're trying to roll our own query engine.

The session's concession: the hand-rolled read layer, a token-rarity lexical ranker with no BM25, a vector index that embedded a frame plus three excerpts, a fusion rule chosen on five specimens, and a navigator doing recall by reading pages, was the weak link, each built as a feature without an oracle. What a mature engine adds is complete, benchmark-tuned primitives and a declarative query; what it does not add is the domain decisions (what a node's text is, the unit of expand, what a frame is for) and the oracle.

> One of the big shortcomings of AI's development approach generally is the lack of trust of pre-made components. It's something that we've been seeing in both the option zero idea, but also in getting sessions to consider the extension of existing features and their slight modification and adaptation as the right way to build something new, instead of taking the constraints that are proposed to a design task and rolling something new within it because that was the only way to perfectly fit what was specified, when in fact a better system would be to slightly change what is specified and leverage the previous work that exists. I'd really like us to far more take a standing on the shoulders of giants approach at every step over and over and over again. And if we're not doing that, ask why and really what's different and what we're actually adding. We're taking a lot of cowboyism.

Ruled consequences, 2026-10-11: steering stays ours (the perspective load, the frame index pages, the fuzzy edges, the navigator as orientation); recall and precision come from engine primitives, in process first (Lucene BM25 and HNSW over whole positions with parent context, reciprocal rank fusion, SPARQL traversal by unit of expand) and a real RDF store if in-process is not enough; every consumer composes steering from ours and recall from the engine, never recall from steering; the hand-rolled ranker, index and fusion retire if the textbook stack ties or wins on the judged oracle. The rule is written into `.claude/rules/extend-first.md`. The prompt inventory across all surfaces is parked until the design is solid and implemented, then done as one rewrite.

## Open

The name of the index. The complement pass, the dense whole-position arm and the Lucene stack arm, all in flight against the judged oracle. Whether a real store is needed. The metaphor for review versus interpretation.
