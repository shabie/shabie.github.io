---
layout: post
title:  "Let the kaleidoscope turn"
date:   2025-08-01 00:14:39 +0200
---

> "Any good classifier knows that in the process of classification, information about variety is lost while information about similarities is gained." - Joseph Tainter

Consider this query from a lawyer to a RAG system: '_Show me all fraud cases from the last five years where the defendant was a first-time offender, the amount was under $50K, and the judge showed leniency in sentencing_'. By constraining these variables (first-time, under $50K, lenient), the users create a petri dish so they can observe what else grows consistently in that environment. This captures a fundamental need: we often want to study the in-group, but we should get to redraw those boundaries whenever our curiosity shifts.

Traditional RAG chokes on this. The moment someone says 'give me all cases where...', similarity search fails badly because it is not built for filtering by criteria. Scanning everything with LLMs at query time is prohibitively slow and expensive. Pre-classifying into rigid categories breaks when users slice data in unexpected ways.

### Postgres to the rescue

I found something almost too simple that helped with the problem: Postgres with pgvector as a hybrid engine combining keyword search, vector similarity, and precise SQL filtering.

#### Strategic Structured Extraction

We capture the "obvious" dimensions that appear in the vast majority of documents and queries: timestamps, numeric values, and rich text fields that preserve variety without rigid categorization.

```json
{
  "case_date": "2023-03-15",
  "offense_amount": 45000,
  "defendant_age": 34,
  "prior_convictions": 0,
  "case_summary": "First-time offender charged with wire fraud involving false invoices to small businesses. Defendant showed genuine remorse and made partial restitution before trial.",
  "sentence_details": "18 months probation with community service. Judge cited defendant's cooperation and lack of criminal history as mitigating factors."
}
```

#### Smart Keywords

LLMs can choose keywords wisely from user queries. By ensuring these important terms appear in structured outputs, you achieve effective keyword matching that largely mimics TF-IDF's benefits. The LLM identifies the most distinctive terms even without access to corpus-wide frequency statistics. If users search for "leniency in sentencing," ensure fields contain critical words like "lenient," "mitigating," or "compassionate" when doing structured extraction.

#### Vector Similarity

Use vectors to catch semantic gaps when keywords don't match. "White-collar crime" should find documents about "financial fraud." But vectors complement keyword matching; they don't replace it.

#### Boolean Flags

Add broad flags for common filter categories: `is_first_time_offender`, `received_lenient_sentence`, `involved_financial_crime`. These aren't perfect classifications but help narrow search space quickly. The more "orthogonal" they are to each other, the better.

The system constructs SQL queries combining all approaches:

```sql
SELECT * FROM cases 
WHERE case_date >= '2019-01-01' 
  AND offense_amount < 50000
  AND prior_convictions = 0
  AND is_first_time_offender = true
  AND (
    sentence_details ILIKE '%lenien%' 
    OR sentence_details ILIKE '%mitigat%'
    OR embedding <-> query_embedding < 0.3
  )
ORDER BY case_date DESC;
```

This returns a manageable set fitting in a context window for final LLM filtering and analysis.

We're not solving classification, we're embracing its tension. Sometimes you need boolean flags, sometimes keyword matching, sometimes semantic similarity. The magic is in refusing to pick sides. The goal isn't perfect classification. It's helping humans explore real-world complexity without freezing the kaleidoscope on a single pattern.
