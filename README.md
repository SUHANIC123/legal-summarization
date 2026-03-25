# Legal Document Summarization using DistilBART

## Overview
Evaluating DistilBART for Indian Supreme Court judgment summarization
on the IL-TUR benchmark.

## Current Status
Pipeline is working. Model is NOT generating accurate summaries yet.

## What Works
- PDF text extraction
- ROUGE and BERTScore evaluation pipeline
- Tested on 10 Indian Supreme Court judgments

## Key Finding — Granularity Mismatch Problem
DistilBART has a 1024 token limit. Indian legal judgments average
21,000+ words. Testing on Pareshkumar vs State of Gujarat showed:

| Chunk Position | Summary Quality | Issue |
|---|---|---|
| 0-20% | Very Poor | Only captures headers |
| 20-40% | Poor | Only captures background |
| 40-70% | Moderate | Partial legal arguments |
| 85-100% | Best | Captures judgment reasoning |

Even the best chunk misses critical legal content because the model
cannot see the full document.

## Why the Model Fails
DistilBART was trained on CNN/DailyMail news articles where:
- Documents are short (500-800 words)
- Conclusions appear at the top

Indian legal judgments are the opposite:
- Documents are extremely long (10,000-100,000+ words)
- Conclusions appear at the very end after extensive arguments

## Proposed Solutions
1. Fine-tuning on In-Abs dataset (7,030 Indian SC judgments)
2. Structure-Aware Chunking — identify Facts, Arguments,
   Judgment sections separately

## Next Steps
- Obtain In-Abs dataset from IL-TUR benchmark
- Implement Structure-Aware Chunking
- Fine-tune DistilBART on legal data

## Results So Far
| Metric | Score |
|---|---|
| ROUGE-1 | 0.3097 |
| ROUGE-2 | 0.2756 |
| ROUGE-L | 0.2974 |
| BERTScore F1 | 0.8411 |

Note: These scores are against extractive baselines,
not human reference summaries.

## Dataset
10 Indian Supreme Court judgments downloaded from Indian Kanoon.
Target dataset: In-Abs (IL-TUR SUMM task) — access pending.
