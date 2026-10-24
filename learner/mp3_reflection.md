---
title: MP3 Reflection
description: Reflection on grounding, citation validation, abstention, and evaluation for the MP3 grounded Q&A assistant
ms.date: 2026-10-24
ms.topic: concept
---

## Grounding and abstention

At the default retrieval threshold of 0.30, all three questions labelled as should-abstain were correctly refused in the final run. Abstention accuracy was 1.0, and hallucination rate was 0.0. The six answerable questions were answered with valid citations, giving a grounded response rate of 1.0.

The first run exposed an issue that the threshold alone could not fix. Retrieval hit rate was already 1.0 with the default top three chunks, but grounded response rate was only 0.67. The pawnshop-assistant and Julia Stoner questions retrieved the correct story without retrieving enough answer evidence. The model then followed its grounding instruction and abstained. Increasing the context size from three to ten chunks brought in the needed evidence and raised the grounded rate to 1.0.

I compared thresholds using the ten-chunk context. The retrieval hit rate remained 1.0 across the sweep because the expected story was retrieved for every answerable question. At 0.30 and 0.50, the other rates were also perfect. At 0.55, grounded response rate fell to 0.83 because the Julia Stoner question's top score was 0.528 and it was incorrectly rejected.

| Threshold | Retrieval hit rate | Grounded and cited rate | Abstention accuracy | Hallucination rate |
|------------|--------------------|-------------------------|---------------------|--------------------|
| 0.30       | 1.00               | 1.00                    | 1.00                | 0.00               |
| 0.50       | 1.00               | 1.00                    | 1.00                | 0.00               |
| 0.55       | 1.00               | 0.83                    | 1.00                | 0.00               |

I kept the default 0.30 for the submitted run. It tied with 0.50 on this test set and leaves more score margin for answerable questions, while the model correctly abstained on the out-of-scope questions. This small question set is not enough to establish a generally optimal threshold.

## Citations

All citations in the final evaluation mapped to retrieved chunks, and the validator found no fabricated citation numbers. For example, the Blue Carbuncle answer cited the evidence about James Ryder and John Horner. The learner-authored multi-hop question uses a different story, The Red-Headed League, and requires connecting the assistant's name with his identity across retrieved chunks.

The validator checks whether a marker such as `[9]` refers to an available source number. It does not determine whether that source actually supports the adjacent claim. Semantic citation correctness still needs human review or a stronger evaluation method.

## Reading the evaluation table

The final evaluation included six answerable questions and three should-abstain questions. Retrieval hit rate was 1.0, grounded response rate was 1.0, abstention accuracy was 1.0, and hallucination rate was 0.0. Each measure was perfect on this small set, but this should not be taken as proof of production reliability. A larger and more varied question set would be needed to assess retrieval and grounding more broadly.

The threshold comparison showed that context size mattered more than lowering the threshold for this run. Retrieving ten chunks supplied relevant evidence that the top three missed. Raising the threshold to 0.55 caused a false abstention, so I would keep the threshold below the lowest observed answerable top score and test it against more questions before deployment.

## One surprise

The retrieval hit rate was perfect even when the initial grounded response rate was only 0.5. The expected story appeared in the results, but the specific fact needed to answer the question was sometimes in a lower-ranked chunk. That showed me that a source-level hit is not the same as having enough evidence to answer. Increasing top-K improved this test set, though a larger context can also add irrelevant material and should be validated rather than assumed to help.
