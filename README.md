# Stakeholder Framing in the Swachh Bharat Mission

Code and data for **"A Computational Text Analysis of Stakeholder Framing in the Swachh Bharat Mission"** by Aahwaan Khullar (Shiv Nadar School, Faridabad, India), 2026. Research carried out as a Student Data Analytics Research Intern at Ernst & Young, mentored by Gaurav Thapak and Amrita Saib.

## Question

India's Swachh Bharat Mission (SBM) is described as a major success by government sources and as a story of unused toilets and broken promises by many journalists. This project asks whether that difference is real and measurable: **does the way SBM is written about change with who is writing (source type) and who is being written about (stakeholder group)?**

## Data

`sbm_corpus.csv` holds 55 English-language documents across seven source types:

| Source type | Documents |
|---|---|
| Academic papers | 17 |
| News reports | 11 |
| Government press releases | 7 |
| Policy briefings | 6 |
| Investigative journalism | 6 |
| NGO / development-bank reports | 4 |
| Opinion / analysis | 4 |

Each row has the title, source, source type, stakeholder tags (from a fixed 15-category codebook), date, URL, and a short summary of the document. **The `text` column is a summary, not the full source text**, because most sources are copyrighted. All results should be read as descriptive and preliminary.

## Method

Python, run in `sbm_text_analysis.ipynb`:

1. Cleaning and tokenization (NLTK)
2. Stopword removal, stemming, and POS-aware lemmatization (NLTK, WordNet)
3. Sentiment scoring with VADER (Hutto & Gilbert, 2014)
4. Comparison by source type, with a two-sided Mann–Whitney U test
5. Comparison by stakeholder group
6. Topic modeling with LDA on TF-IDF vectors (scikit-learn, 5 topics, `random_state=42`)

## Main results

- **Source type matters.** Government press releases and policy briefings are mildly positive (mean +0.107, n = 13); investigative journalism and opinion pieces are clearly negative (mean −0.425, n = 10). Mann–Whitney U = 107.5, p = 0.009. Academic papers sit close to neutral (−0.030).
- **Stakeholder groups differ.** Marginalized castes (−0.535), the general public (−0.492), and sanitation workers (−0.263) are framed most negatively.
- **The same deaths read differently depending on who reports them.** Within the sanitation-worker documents, official and parliamentary reports of worker deaths score near neutral or positive, while activist and investigative accounts of the same deaths score strongly negative (−0.78 to −0.90).

## A correction worth knowing about

An earlier version of the corpus summaries included my own research notes (for example, "good source for the paper"). VADER scored those notes as if they were part of the source, which pushed several documents toward positive and made sanitation workers look almost exactly neutral (−0.003). I removed the notes from all 55 summaries and re-ran everything; the results above are from the cleaned corpus. The source-type difference survived the correction; the "neutral sanitation workers" result did not. The paper discusses this in Section 5.2.

## Repository

```
sbm_corpus.csv                  corpus (55 documents)
sbm_text_analysis.ipynb         full analysis, with outputs
sbm_analysis_results.csv        document-level scores and dominant topics
sentiment_by_source_type.csv    summary table (Table 2 in the paper)
sentiment_by_stakeholder.csv    summary table (Table 3 in the paper)
fig2_ … fig5_ ….png             charts used in the paper (Figures 2–5)
requirements.txt
CITATION.cff                    citation details
LICENSE
```

## Run it

**Google Colab:** open the notebook and choose *Runtime → Run all*. The corpus is fetched from this repository automatically.

**Locally:**

```bash
pip install -r requirements.txt
python -m nltk.downloader punkt_tab stopwords wordnet averaged_perceptron_tagger_eng
jupyter notebook sbm_text_analysis.ipynb
```

## License

Code is released under the MIT License. The `text` column of the corpus contains summaries written for this project; titles and URLs refer to the original publishers' work.

## Limitations

Summaries rather than full texts; 55 documents (several stakeholder groups have only 2–3); English-language sources only; stakeholder tags assigned by a single coder; VADER can misread bureaucratic or statistical prose.

## Citation

Khullar, A. (2026). *A computational text analysis of stakeholder framing in the Swachh Bharat Mission* [Preprint].
