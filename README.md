# Review 1 — Plan: Shared Python Study Guides

**Team:** Angelo Bustamante and Bryan\
**Due:** November 17, 2026, 1 p.m. Eastern\
**Status:** Plan stage — not yet built. No application, local prototype, retrieval index, or demo exists in this repository.\
**Repository and shared record:** [https://github.com/ab2737/Review-1-Plan-Group-Content-System](https://github.com/ab2737/Review-1-Plan-Group-Content-System)

This is the single evolving group record. Angelo and Bryan have completed their assigned reviews, discussed the findings, and confirmed the finalized plan and section responsibilities.

## 1. Problem, reader, and bounded scope

Students learning introductory Python often save explanations in separate bookmarks, messages, and personal chats. Useful material becomes difficult for a study partner to find, verify, or improve. Our educational-content publishing project will turn selected learning sources into a shared collection of short, cited study guides.

**Primary reader:** an introductory programming student who wants a clear explanation and a small example. **Contributors/reviewers:** Angelo and Bryan, taking turns drafting and checking content.

**Research question:** How can a small team use approved Python documentation and a shared RAG workflow to create accurate, source-cited study guides that students can review, reuse, and improve?

The first version will cover Python lists, tuples, sets, and dictionaries using at most 10 approved sections from the official Python tutorial. It will produce five reviewed study guides. It will not collect social-media feeds, private course materials, student records, or graded assignment answers. It will not attempt to answer questions outside its approved collection.

A guide will contain a question/title, a short explanation, an original example, source-section links, author, reviewer, source version, and review status.

**Shared value:** sources, drafts, feedback, and approved guides persist in one collection. A second person must check a draft before publication. Both teammates can reuse and revise the same approved guide. Copying an individual chat does not provide this source register, review queue, or version history.

### Observations and assumptions

**Observations from the current project:**

- The proposed educational source is the official Python documentation, with its license and reuse conditions recorded in Section 2.
- The team has two members who can take turns creating and reviewing study guides.
- The project is currently at the Plan stage; no application, retrieval index, or publishing workflow has been built.
- G-001 provides one manually traced example, but it does not demonstrate a working ETL or RAG pipeline.

**Assumptions to investigate:**

- Introductory programming students would find short explanations and small examples useful.
- Retrieving up to three relevant passages will provide enough evidence for most questions in the four selected Python topics.
- A small vector-based RAG approach will be sufficient for the initial collection.
- A two-person review process will help catch incorrect explanations, missing citations, and unsupported claims.

These assumptions are not user-research findings. The team will evaluate them during Build using the planned user stories and success criteria.

## 2. Permitted sources and source records

| **Source** | **Use** | **Permission/access rule** |
|---|---|---|
| [Python tutorial: Data Structures](https://docs.python.org/3/tutorial/datastructures.html) | Initial educational corpus; select sections on the four scoped topics | Public documentation under the [PSF license](https://docs.python.org/3/license.html); retain required license/copyright notices for redistributed source text and describe modifications. |
| Team-written examples and explanations | Original guide content | Include only material authored for this project; record its author. |
| Research references in Section 4 | Architecture decisions | Link and paraphrase for research; not automatically part of the indexed educational corpus. |

Public access alone will not qualify a new source for ingestion. The source register must record its permission basis first. Do not copy classmates' work, LMS content, private messages, or unlicensed third-party tutorials.

Each source record will include: source ID, title, exact URL and section anchor, publisher, documentation version, access date, permission/license URL, required notices, topic, and content hash. Each chunk will retain its source ID, heading, chunk ID, and source version.

The unversioned Python URL can change. At Build, pin a documentation version and keep the retrieved source hash so updates are detectable. Permission to reuse documentation does not imply permission to ignore its conditions.

## 3. Source-to-transform-to-reviewable-output workflow

| **Step** | **Planned behavior** | **Reviewable evidence/output** |
|---|---|---|
| Extract | A contributor manually selects an allowed section and records source metadata. Fetch only approved pages. | Source register entry and source snapshot/hash. |
| Clean and split | Remove menus/navigation, preserve headings and code blocks, split by section into approximately 200–400-word chunks, and detect duplicates by normalized-text hash. | Chunk records linked to original sections; rejected-duplicate log. |
| Index and retrieve | Embed permitted chunks and the question; filter by topic/version; rank by cosine similarity and return up to three chunks. | Ranked chunk IDs, scores, source URLs, and model/version. |
| Draft | Supply the retrieved passages to a language model to draft a short guide with citations. Treat source text as data, not instructions. | Draft, prompt/model record, retrieved chunk IDs, and claim-to-source mapping. |
| Review | The other teammate checks claims, citations, example behavior, and source permissions; requests changes or approves a specific revision. | Reviewer identity, decision, feedback, and revision ID. |
| Publish and maintain | Add approved revisions to the shared guide list. Changes to a guide or its source return it to review. | Approved collection and retained history of revisions. |

Planned states: **draft → needs review → changes requested → needs review → approved**. A reviewer can also approve directly from needs review. The author cannot approve their own guide. Source changes mark affected guides stale until rechecked.

**Planned implementation:** Python for extraction and embeddings, JSON/SQLite for metadata and review records, a small local web interface for the shared collection, and GitHub for versioned project evidence. Exact cosine search is sufficient for the small initial corpus; a dedicated vector database is unnecessary for this version. Model selection, execution environment, and generation-service access remain Build decisions. If generation access is unavailable, manually authored drafts can test the review workflow, but will be labeled manual and will not count as an implemented RAG feature.

## 4. Research and decision record

References were reviewed during preparation on November 16, 2026. The following summaries are limited to the cited material; the implementation choice is a project judgment, not a measured performance claim.

| **ID** | **Research** | **Finding and relevance** |
|---|---|---|
| R1 | [Lewis et al., Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks](https://arxiv.org/abs/2005.11401) | RAG combines retrieval from external information with generation. This motivates retrieving source passages before drafting rather than relying only on model memory. It does not guarantee correctness. |
| R2 | [Sentence Transformers: Semantic Search](https://www.sbert.net/examples/sentence_transformer/applications/semantic-search/README.html) | Semantic search embeds queries and documents and ranks their similarity. Query/document model suitability matters. This supports retrieving a relevant explanation even when a learner uses different wording. |
| R3 | [Microsoft GraphRAG documentation](https://microsoft.github.io/graphrag/) | GraphRAG extracts entities and relationships and uses graph structures and community summaries. It targets questions requiring connections across information or summaries of broader collections. This introduces extra extraction and review work for our small corpus. |

**Vector:** a numeric representation of text used to search for similar meaning. **Graph:** entities/concepts connected by explicit relationships. **RAG:** retrieve evidence and use it as context for generation; it is a workflow, not itself a database.

### Two approaches compared

| **Dimension** | **A: Vector RAG with metadata and human review** | **B: Graph-assisted RAG with vector retrieval** |
|---|---|---|
| Retrieval | Similarity-ranked passages, filtered by topic/version | Passages plus concept relationships and graph traversal |
| Best fit | Focused questions such as how a list method works | Connected questions about concepts and prerequisites across sources |
| Additional stored data | Chunks, embeddings, provenance, reviews | All of A plus extracted entities, edges, and their evidence |
| Main weakness | Similar text can still be irrelevant; multi-step connections may be missed | Incorrect/missing relationships can mislead retrieval; graph maintenance adds work |
| Two-person feasibility | Smaller implementation and evaluation surface | More components to build and independently validate |
| Decision | Selected for the first version | Deferred; not implemented |

**D1 — Select vector-based RAG, without a graph database.** The scoped output is a short explanation grounded in a few sections, so passage retrieval is a reasonable starting point. Metadata links between a guide and its sources are provenance records, not a claim that GraphRAG has been implemented.

**D2 — Require independent human approval.** Retrieval scores and fluent writing cannot establish factual correctness. Review must verify each claim against its cited section and run each example.

**D3 — Reconsider graphs only if evidence justifies them.** If at least three of ten later evaluation questions require cross-source relationships that passage retrieval consistently misses, document those failures before proposing graph support. This threshold is a planning rule, not an observed result.

A simple keyword search will also be measured as a low-cost retrieval baseline during Build. If vectors do not help on the same questions, report that result instead of assuming the more complex option is better.

## 5. Four checkable stories and ownership

| **ID** | **User story** | **Owner** | **Different reviewer** | **Acceptance check planned for Build** |
|---|---|---|---|---|
| S1 | As a contributor, I can add a permitted section so teammates can trace its content. | Angelo | Bryan | A valid entry retains all required source fields; an entry without permission evidence is rejected; importing identical content twice creates no duplicate chunks. |
| S2 | As a learner, I can request a cited draft so I can inspect its supporting evidence. | Bryan | Angelo | For a supported question, show up to three retrieved chunks and a draft with valid source links; for an unsupported question, report insufficient evidence instead of publishing an answer. |
| S3 | As a reviewer, I can request changes or approve a particular revision so only checked guides enter the collection. | Angelo | Bryan | Self-approval is rejected; requested changes prevent publication; another person's approval publishes that revision; editing it resets approval. |
| S4 | As a teammate, I can find approved guides and their history so the group can reuse and maintain them. | Bryan | Angelo | Both teammate accounts can find the same approved guide by topic, see its citations and review record, and distinguish a stale/revised version from the previously approved one. |

## 6. Success criteria and evaluation plan

These are future targets, not completed test results.

| **Measure** | **Target** | **How to check** |
|---|---|---|
| Source traceability | 100% of indexed chunks have valid source ID, section, version, and permission evidence | Inspect all records in the small corpus; attempt one missing-permission import. |
| Retrieval relevance | At least 8 of 10 supported questions return a reviewer-designated supporting chunk in the top three | Freeze questions and expected sections before tuning; save question, expected IDs, returned IDs, and result for vector and keyword retrieval. |
| Unsupported questions | All 3 out-of-scope questions remain unpublished and receive an insufficient-evidence response | Test weather, personal grades, and an excluded programming topic. Similarity alone is not an evidence-sufficiency rule. |
| Citation support | Every factual claim in the 5 approved guides is supported by its cited passage | Reviewer records claim, supporting section, pass/fail, and correction. |
| Example accuracy | All code examples in the 5 approved guides run with documented expected results | Save runtime version, input, expected output, actual output, and result. |
| Shared workflow | All S3 approval cases and both-account visibility in S4 pass | Record actions, revision IDs, actor/reviewer identities, and outcomes. |

At Build, each teammate will author five supported evaluation questions and check the other's expected source labels before tuning. Keep failed cases. At Improve, record a real peer-requested change, its linked revision, and a repeat of the relevant check; no such change or retest has happened yet.

## Simple review section 4-6 (Angelo Bustamante, 11/16/2026)

Reviewer: Angelo Bustamante Sections reviewed: Approach comparison and evaluation criteria, Sections 4–6.

Finding: Vector retrieval fits the small initial Python collection. Comparing it with keyword search will help determine whether the added complexity improves retrieval. The evaluation criteria also address source permissions, unsupported questions, factual accuracy, and independent approval.

Recommended clarification: Explain that the keyword comparison may favor either approach. The three-out-of-ten threshold should trigger investigation of graph support, rather than an automatic switch.

Review outcome: The approach is reasonable for the planned scope. Its effectiveness remains untested until Build.

## Bryan's review of assigned sections (November 16, 2026)

**Reviewer:** Bryan  
**Review date:** November 16, 2026  
**Sections reviewed:** Section 4 (research and architecture decisions), Section 5 (user stories S2 and S4), and Section 6 (success criteria).

### Research links and architecture

- **R1 — Lewis et al., RAG paper:** The paper describes combining a language model with information retrieved from an external collection. This supports the project's general plan to retrieve source passages before drafting, but it does not guarantee that the generated study guide will be correct.
- **R2 — Sentence Transformers semantic search:** The documentation describes embedding queries and documents and ranking them by similarity. This supports the proposed vector retrieval approach, but similarity does not prove that a passage is correct or sufficient.
- **R3 — Microsoft GraphRAG:** The documentation describes extracting entities and relationships and using graph structures and community summaries. This supports the plan's description of GraphRAG as a more involved approach that may be useful for questions connecting information across sources.

**Architecture finding:** Vector RAG is a reasonable starting point for a small collection of Python tutorial sections and focused questions. Comparing it with keyword search is important because either method could perform better. The team should use the evaluation results rather than assume vector search will win.

**Graph-support clarification:** The three-out-of-ten threshold should trigger an investigation, not an automatic switch to GraphRAG. Before adding graph features, the team should review the failed questions and decide whether they genuinely require cross-source relationships that passage retrieval cannot handle.

### S2 and S4 review

- **S2 — Learner requests a cited draft:** The story is understandable and has a checkable outcome. During Build, record the question, expected source section, top-three retrieved chunks, and whether the draft's claims are supported. For an unsupported question, check that the system reports insufficient evidence instead of publishing a guide.
- **S4 — Teammate finds approved guides and history:** The story is understandable and has a checkable outcome. During Build, check with both teammate accounts that they can find the same approved guide, view its citations and review record, and distinguish a revised or stale guide from an older approved version.

These are reviews of the written acceptance criteria only. The application and two-account workflow do not exist yet, so S2 and S4 have not been functionally tested.

### Success-criteria review

The targets are measurable for a Plan-stage project. Freeze the evaluation questions and expected source sections before tuning retrieval, and keep failed cases as well as successful ones. The 8-of-10 retrieval target should be based on whether the returned chunks actually support the question, not simply whether they look related.

**Review outcome:** The written research, architecture choice, S2/S4 stories, and evaluation plan are reasonable for the proposed scope. Their effectiveness remains untested until Build.

**Preparation note:** This review text was prepared with ChatGPT assistance from the repository README and its linked research references. Bryan reviewed and confirmed the content before submission.

## 7. Actual Plan-stage checks and worked example

| **Check** | **Actual result** | **Evidence/limit** |
|---|---|---|
| Inspect repository before drafting | GitHub contents API reported an empty repository. | No existing implementation or plan was available to review. |
| Open primary research references | R1, R2, and R3 returned readable content supporting the summaries above. | Direct links in Section 4; this is source reading, not a runtime benchmark. |
| Check candidate corpus access and permission basis | Python Data Structures and its license page returned readable content. | Section 2 links; documentation license identifies reuse conditions. No corpus ingestion has run. |
| Walk through one source-backed guide manually | The example below can be traced to the lists section, and its code output was checked during review. | This is a manual example/code check only; no retrieval or generation pipeline was tested. The independent peer review has been completed; this check remains a manual example/code check only, not a test of a retrieval or generation pipeline. |
| Inspect ownership matrix | Four stories have named owners and different reviewers; each teammate owns two. | Section 5. Assignment does not establish completed effort. |

### Manual walkthrough: example guide G-001

- **Question:** How do I add one item to the end of a Python list?
- **Source:** [Python tutorial, More on Lists](https://docs.python.org/3/tutorial/datastructures.html#more-on-lists).
- **Source record sketch:** PY-LISTS-001; publisher: Python Software Foundation; accessed: November 16, 2026; permission: PSF documentation license; version/hash: pending actual ingestion.
- **Draft explanation:** Use the list's append method to add one item at its end.
- **Original illustrative example:**

```python
topics = ["lists"]
topics.append("dictionaries")
print(topics)
# Expected: ['lists', 'dictionaries']
