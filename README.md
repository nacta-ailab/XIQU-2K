# XIQU-2K: A Benchmark for Chinese Xiqu Knowledge and Historical-Source Understanding

**XIQU-2K** is an in-development benchmark for evaluating how large language models (LLMs) answer questions about **Chinese xiqu (traditional opera)**, particularly fine-grained facts preserved in regional opera gazetteers. It is designed to assess both what a model can answer **without supporting material** and what it can **extract, integrate, and infer when a reference passage is supplied**.

> **Release status:** XIQU-2K is a planned public release targeting approximately **2,000 questions**. The distributions and results below describe the current pilot study and should not be interpreted as final XIQU-2K statistics or a leaderboard for the forthcoming release.

## Motivation

General-purpose LLMs may answer widely documented xiqu questions correctly while struggling with less accessible historical records, specialist terminology, regional performance practices, and relations among performers, troupes, and repertoire. A multiple-choice answer can also be correct because of cues or elimination rather than reliable recall.

XIQU-2K aims to provide **source-grounded, verifiable evaluation material** that distinguishes:

- **Knowledge recall and recognition:** identifying facts, entities, and terms without additional reference material.
- **Historical-text comprehension and integration:** finding relevant evidence and aligning dates, entities, events, and relationships in supplied excerpts.
- **Evidence-based reasoning and scope control:** drawing conclusions supported by a record without overstating what the record proves.

The benchmark also supports controlled comparisons between **question-only** and **question-with-reference** settings to investigate the value of domain-specific xiqu texts.

## Dataset Construction

1. **Source collection and passage selection.** The pilot draws on a xiqu encyclopedia and *Zhongguo Xiqu Zhi* (中国戏曲志, *Gazetteers of Chinese Opera*), including regional volumes. Passages are selected from identifiable entries or sections, such as chronology, opera genres, repertoire, music, and institutions. Tables and image-only OCR segments were excluded from the initial extraction pass.
2. **Evidence-grounded question generation.** A generation model produces three question formats: four-option multiple choice, true/false, and short answer. Questions specify necessary entities, periods, and contextual constraints; distractors are designed to be plausible yet contradicted or unsupported by the evidence. Short-answer rubrics distinguish single required facts from multi-part answers.
3. **Automated validation and independent model review.** Scripts check format, answer-option mapping, potential answer leakage, and whether key quotations occur in the source. A separate model call verifies proposed answers against the cited passage and reviews accepted alternative wordings and short-answer scoring points.
4. **Human verification.** Questions and answers are manually reviewed; items with incorrect gold answers, ambiguous wording, or leaked answers are removed or revised.
5. **Provenance and evaluation metadata.** Records retain source identifiers, volume or entry, section, source-line and character-span locations, reference context (`original_context`), key quotations, and—where available—original PDF page markers. Question type, knowledge category, capability dimension, and grading requirements are tracked separately.

**Copyright and release scope:** Source passages and other accompanying materials will be shared only to the extent permitted by applicable licenses and rights. A question's traceable source is not, by itself, permission to redistribute the full underlying text.

## Dataset Statistics

The **planned XIQU-2K release** targets approximately **2,000** items. The following statistics characterize the **pilot design**; the final release composition may change after expert review and expansion.

| Pilot attribute | Distribution |
|---|---|
| Source | Xiqu encyclopedia: **20%**; *Zhongguo Xiqu Zhi*: **80%** (covering **28 provincial-level volumes**) |
| Question format | Four-option multiple choice: **60%**; true/false: **30%**; short answer: **10%** |
| Capability dimensions | Domain factual knowledge; entity/concept disambiguation; relationship use and error correction; evidence extraction/integration; evidence-based judgment and historical scope (**20% each**) |

**Pilot knowledge categories**

| Category | Share |
|---|---:|
| Roles, performance, and stagecraft | 20% |
| Performers, troupes, and transmission | 19% |
| Repertoire and versions | 19% |
| Vocal styles, music, and accompaniment | 16% |
| Historical performances and customs | 15% |
| Opera genres and geography | 11% |

## Experiments

Pilot evaluations compare **question-only**, **question + original reference passage**, and—for two hosted endpoints—**question + explicitly enabled search**. The percentages below are **overall accuracy on the planned evaluation items**, with timeouts or empty responses receiving no credit. These are **preliminary observations, not general model rankings**.

| Model | Question only | + Original reference | + Search requested |
|---|---:|---:|---:|
| Qwen3.5-122B (institutionally deployed) | **68%** | **97%** | — |
| Qwen3.8-27B (institutionally deployed) | **67%** | **97%** | — |
| qwen3.8-max (hosted endpoint) | **75%** | — | **74%** |
| deepseek-v4-pro (hosted endpoint) | **76%** | — | **72%** |

**Preliminary findings**

- Without references, models performed better on **encyclopedia-sourced questions (75.0%–90.0%)** than on **gazetteer-sourced questions (63.8%–73.8%)**. Short-answer accuracy was **40%** across all four question-only configurations.
- Providing the original passage raised both tested institutionally deployed models to **97%**, suggesting substantial benefit from accessible, relevant historical evidence. This improvement does not imply that all source-supported answers required multi-step reasoning; many involved direct extraction.
- Explicitly requesting search did **not** produce a net gain for the hosted models in this pilot. Actual search invocation and retrieved content were not fully logged; these results **do not establish that retrieval itself is ineffective**.
- The tested models differ in version, configuration, and deployment. Their scores should **not** be interpreted as isolating the effect of parameter count or commercial versus open-weight deployment.

## Future Work

- Expand and expert-review the collection toward the **XIQU-2K** release, with stronger coverage of regional and less-documented genres, periods, and specialist knowledge.
- Distinguish **direct extraction**, **multi-evidence integration**, and **evidence-boundary reasoning** in reference-supported evaluations.
- Create paired multiple-choice and open-ended variants of the **same facts**, reducing confounding from question selection and grading strictness.
- Refine short-answer rubrics to score conclusions, required facts, and source justification separately; calibrate automated graders through expert checks.
- Compare reference injection, general web search, and retrieval from a curated xiqu knowledge base with controlled prompts and logged retrieval traces.
- Publish dataset documentation, evaluation procedures, and reproducibility materials, while respecting source rights and reserving suitable held-out test items.

## Contact

For questions, research collaboration, or dataset-related inquiries: **chenmosha@nacta.edu.cn**
