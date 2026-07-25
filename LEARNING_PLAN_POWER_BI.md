# Text Mining with R: Analytics and Power BI Learning Plan

This plan follows the nine substantive chapters in *Text Mining with R: A Tidy Approach*. Each chapter connects the book's tidy-text workflow to an analytics use case and a Power BI report. The R portion produces a tidy analytical table; the Power BI portion uses that table to communicate the result.

## Chapter 1: The tidy text format

### Core ideas

Tidy text represents text as a table with one token per row. A token is a meaningful unit such as a word, sentence, paragraph, or n-gram. This structure follows tidy-data principles: variables are columns, observations are rows, and each observational unit has its own table.

`unnest_tokens()` converts text stored in strings into tokens. Once text is tidy, familiar operations can remove stop words, group observations, count word occurrences, and compare word frequencies. Text does not need to remain tidy throughout an entire workflow; analysts can move between strings, corpora, document-term matrices, and tidy tables as the task requires.

### Analytics and Power BI example

Suppose an analyst wants to compare vocabulary across Jane Austen's novels. The analytical grain is one token in one book and, where useful, one line or section. After tokenizing the text, the analyst removes stop words and calculates the frequency of each remaining word by book.

The exported result could contain `book`, `word`, and `word_count`. In Power BI, a learner could create:

- A ranked bar chart of the most frequent words.
- A book slicer that changes the ranking.
- A matrix with books as rows, words as columns, and counts as values.
- A tooltip showing a word's count and share of all retained tokens in the selected book.

The central modeling lesson is that a well-defined row grain makes filtering and aggregation predictable. Keeping one token per row also preserves the option to regroup tokens by book, chapter, or another document identifier.

### Comprehension questions

1. What does one-token-per-row mean, and how does it satisfy tidy-data principles?
2. How are tokenization and a token different?
3. Why is a tidy token table easier to filter and summarize than a single string containing an entire document?
4. What information must remain alongside each token if words will later be compared across books?
5. Why might an analyst convert tidy text into a document-term matrix and then convert the result back to tidy form?

### Practical exercise

Use the Jane Austen workflow in the chapter to create a tidy word table. Remove stop words, then count and rank words within each book. Export a table containing the book, word, count, and within-book rank. Load the export into Power BI and build a page with a book slicer, a top-words bar chart, and a book-by-word matrix. Verify that selecting a book changes both the ranking and the matrix consistently.

**Sources:** [01-tidy-text.Rmd](01-tidy-text.Rmd) — “The tidy text format,” “Contrasting tidy text with other data structures,” “The `unnest_tokens` function,” “Tidying the works of Jane Austen,” “Word frequencies,” and “Summary.”

## Chapter 2: Sentiment analysis with tidy data

### Core ideas

Sentiment analysis uses words with known emotional or opinion content to estimate the sentiment expressed in text. In a tidy workflow, this can be implemented by joining tokens to a sentiment lexicon.

The chapter compares three lexicons. Bing classifies words as positive or negative, AFINN assigns scores from negative to positive, and NRC includes positive and negative labels plus emotions such as anger, anticipation, disgust, fear, joy, sadness, surprise, and trust. Because the lexicons use different vocabularies and scoring systems, they can produce different results for the same text.

Word-level sentiment is useful but limited. Context, especially negation, can change meaning. The chapter therefore also examines units larger than individual words and shows how sentiment can vary across the course of a narrative.

### Analytics and Power BI example

An analyst could measure the emotional arc of a novel by dividing it into ordered sections, joining its tokens to a lexicon, and calculating net sentiment in each section. A suitable exported table could contain `book`, `section`, `lexicon`, `sentiment`, `word`, and either a category count or score.

In Power BI, the analysis could become:

- A line chart of net sentiment by section.
- Small multiples or a slicer for comparing books.
- A stacked bar chart showing NRC emotion counts.
- A ranked table of the words contributing most to positive and negative totals.
- A lexicon selector for examining how the analytical conclusion changes with the dictionary.

This example emphasizes that a sentiment score is a model output shaped by the selected lexicon, aggregation rule, and unit of analysis—not a direct observation of a reader's interpretation.

### Comprehension questions

1. How does an inner join turn a tidy token table into a sentiment-analysis table?
2. How do the Bing, AFINN, and NRC lexicons differ in their outputs?
3. Why might two sentiment lexicons produce different emotional profiles for the same document?
4. What does a narrative sentiment trend reveal that a single document-level score does not?
5. Why can analyzing isolated words misinterpret phrases containing negation?

### Practical exercise

Choose one Jane Austen novel and divide it into ordered sections using the chapter's approach. Join the tokens to one sentiment lexicon, calculate section-level sentiment, and identify the words contributing most strongly to the positive and negative results. Export both a section summary and a contributing-word table. In Power BI, create a sentiment trend line, positive/negative contributor bars, and a section slicer. Add a note describing the limitations of word-level sentiment.

**Sources:** [02-sentiment-analysis.Rmd](02-sentiment-analysis.Rmd) — “Sentiment analysis with tidy data,” “The `sentiments` datasets,” “Sentiment analysis with inner join,” “Comparing the three sentiment dictionaries,” “Most common positive and negative words,” “Looking at units beyond just words,” and “Summary.”

## Chapter 3: Analyzing word and document frequency with TF-IDF

### Core ideas

Term frequency measures how often a term appears in a document. High frequency alone does not necessarily mean that a word characterizes the document, because common words can appear frequently throughout an entire corpus.

Inverse document frequency reduces the weight of words that appear in many documents and increases the weight of words that occur in fewer documents. TF-IDF multiplies term frequency by inverse document frequency to estimate how important a word is to one document within a collection. It is a useful heuristic rather than a perfect theory of meaning.

The chapter also explores Zipf's law, uses `bind_tf_idf()` to calculate the statistic in tidy form, and compares results across novels and physics texts.

### Analytics and Power BI example

Imagine an analyst comparing a collection of reports. Raw frequency answers “Which words appear most?” TF-IDF answers a different question: “Which words distinguish this report from the rest of the collection?”

The exported table could include `document`, `term`, `term_count`, `term_frequency`, `inverse_document_frequency`, and `tf_idf`. In Power BI, a learner could build:

- A document slicer and ranked TF-IDF bar chart.
- A scatterplot comparing term frequency with TF-IDF.
- A matrix that highlights terms with high TF-IDF in particular documents.
- A tooltip that shows the components used to calculate each TF-IDF value.

Comparing count and TF-IDF prevents the dashboard from treating “common” and “distinctive” as interchangeable.

### Comprehension questions

1. What analytical question does term frequency answer?
2. Why does inverse document frequency give less weight to terms found in many documents?
3. What does a TF-IDF value of zero indicate about a term within the corpus used in the calculation?
4. Why can changing the set of documents change a term's TF-IDF value?
5. How is TF-IDF more informative than a fixed stop-word list for identifying document-specific language?

### Practical exercise

Reproduce the chapter's TF-IDF analysis for the Jane Austen novels. Retain the term count, term frequency, inverse document frequency, TF-IDF, and within-book TF-IDF rank. Export the result and load it into Power BI. Create one visual ranked by raw count and another ranked by TF-IDF, controlled by the same book slicer. Add a scatterplot of count versus TF-IDF and write a short interpretation of at least two words whose apparent importance changes between the measures.

**Sources:** [03-tf-idf.Rmd](03-tf-idf.Rmd) — “Analyzing word and document frequency: tf-idf,” “Term frequency in Jane Austen's novels,” “Zipf's law,” “The `bind_tf_idf()` function,” “A corpus of physics texts,” and “Summary.”

## Chapter 4: Relationships between words—n-grams and correlations

### Core ideas

Single-word analysis cannot show which words immediately follow one another or which words tend to occur in the same section. N-grams preserve sequences of adjacent words; a bigram contains two adjacent words. Counting bigrams reveals common phrases and provides context that a unigram analysis loses.

After separating a bigram into its component words, analysts can remove stop words, count relationships, and use the pairs as edges in a network. Bigrams also improve sentiment interpretation by revealing words such as “not” that modify a sentiment-bearing word.

Co-occurrence and pairwise correlation answer a related but different question. Co-occurrence counts how often words appear within the same unit, while correlation measures how consistently their appearances are associated across units.

### Analytics and Power BI example

For a collection of documents, the analyst could create two relationship tables:

- An adjacent-word table with `word1`, `word2`, and `bigram_count`.
- A co-occurrence table with `word1`, `word2`, `pair_count`, and `correlation`.

In Power BI, the report could show the strongest word pairs in a ranked table, compare pair count with correlation in a scatterplot, and let the user select one word to inspect its most important neighbors. A matrix with `word1` on rows and `word2` on columns can reveal clusters of strong relationships without requiring a specialized network visual.

The report should keep adjacency, co-occurrence, and correlation clearly labeled because each measures a different kind of relationship.

### Comprehension questions

1. How does a bigram differ from two words that merely occur in the same document section?
2. Why should stop-word filtering be handled carefully after a bigram is separated into two words?
3. How can bigrams reveal a limitation of unigram sentiment analysis?
4. What is the difference between a high pair count and a high pairwise correlation?
5. What columns are needed to represent word relationships as an edge table?

### Practical exercise

Use the chapter's Jane Austen examples to create bigram counts and pairwise word correlations. Export one table for adjacent pairs and one for correlated pairs. In Power BI, build a word selector, a ranked neighbor table, a count-versus-correlation scatterplot, and a relationship matrix. Include a filter that removes rare pairs, and explain how changing the threshold alters the visible network of relationships.

**Sources:** [04-word-combinations.Rmd](04-word-combinations.Rmd) — “Relationships between words: n-grams and correlations,” “Tokenizing by n-gram,” “Counting and filtering n-grams,” “Analyzing bigrams,” “Using bigrams to provide context in sentiment analysis,” “Visualizing a network of bigrams with ggraph,” “Counting and correlating pairs of words with the widyr package,” “Counting and correlating among sections,” “Pairwise correlation,” and “Summary.”

## Chapter 5: Converting to and from non-tidy formats

### Core ideas

Tidy text is effective for manipulation and visualization, but many text-mining tools require other structures. A document-term matrix (DTM) has one row per document, one column per term, and values that usually represent term counts or TF-IDF. Because most document-term combinations are absent, these matrices are often sparse.

The chapter demonstrates how to tidy `DocumentTermMatrix` and `dfm` objects, cast tidy data into a matrix, and convert corpus objects with document metadata into data frames. These conversions act as “glue” between tidy analysis tools and models that expect matrix or corpus inputs.

The financial-article example shows why metadata should remain connected to the text during conversion: attributes such as document identifiers, dates, or sources are needed for interpretation after modeling.

### Analytics and Power BI example

A Power BI-friendly version of a DTM is usually a long table with one row per observed document-term combination rather than thousands of mostly empty columns. The table could contain `document_id`, `term`, `count`, and `tf_idf`, with a separate document table holding metadata.

This structure supports:

- A document-by-term matrix visual.
- Document metadata slicers.
- Ranked terms for the selected document or category.
- Drill-through from a summary document to its term profile.

The broader analytics lesson is to choose the data structure required by the current task while preserving identifiers that allow results to be joined back to their source documents.

### Comprehension questions

1. What do the rows, columns, and cell values of a document-term matrix represent?
2. Why is a document-term matrix usually sparse?
3. When would an analyst cast tidy text into a matrix?
4. Why should a model's matrix output be converted back to tidy form for interpretation?
5. What metadata must be preserved so term-level results can be connected to their original documents?

### Practical exercise

Start with a tidy document-word count table from an earlier chapter. Cast it into a document-term matrix, then tidy the matrix again. Check that document-term counts survive the round trip. Export the final long table plus a document metadata table. In Power BI, relate the tables by document identifier and create a document-by-term matrix, metadata slicers, and a drill-through page showing the selected document's leading terms.

**Sources:** [05-document-term-matrices.Rmd](05-document-term-matrices.Rmd) — “Converting to and from non-tidy formats,” “Tidying a document-term matrix,” “Tidying DocumentTermMatrix objects,” “Tidying `dfm` objects,” “Casting tidy text data into a matrix,” “Tidying corpus objects with metadata,” “Example: mining financial articles,” and “Summary.”

## Chapter 6: Topic modeling

### Core ideas

Topic modeling is an unsupervised method for finding groups of words that characterize a document collection. Latent Dirichlet allocation (LDA) treats every document as a mixture of topics and every topic as a mixture of words. Documents therefore do not need to belong to only one topic, and a word can contribute to more than one topic.

The model produces two especially useful probabilities. Beta describes the probability of a term within a topic, while gamma describes the topic mixture within a document. Tidying these outputs makes it possible to rank terms, compare documents, and visualize the model with familiar tools.

The library-heist example applies LDA to book chapters, examines per-document classification and word assignments, and investigates mistaken assignments. These errors are important evidence about the model's limitations, not merely results to hide.

### Analytics and Power BI example

An analyst could export:

- A topic-term table with `topic`, `term`, and `beta`.
- A document-topic table with `document`, `topic`, and `gamma`.
- Document metadata for interpretation.

Power BI could present a ranked bar chart of the leading terms in a selected topic, a stacked bar chart showing each document's topic mixture, and a table of documents whose strongest assigned topic conflicts with a known label. Cross-filtering would let a user select a topic and inspect both its vocabulary and the documents most associated with it.

The topic labels should be treated as analyst interpretations. The model returns topic numbers and probability distributions, not inherently meaningful names.

### Comprehension questions

1. Why is topic modeling described as unsupervised classification?
2. What does it mean to say that every document is a mixture of topics?
3. What does beta describe in a tidied LDA model?
4. What does gamma describe in a tidied LDA model?
5. Why should incorrectly assigned words or documents be examined when evaluating a topic model?

### Practical exercise

Follow the library-heist workflow to fit an LDA model to book chapters. Export the topic-term probabilities, document-topic probabilities, and known book titles. In Power BI, build a topic-detail page with leading terms, a document page with topic-mixture bars, and an exception table for chapters whose highest-probability topic does not match the dominant topic of their book. Write a short interpretation based on the probabilities rather than assigning meaning from a single word.

**Sources:** [06-topic-models.Rmd](06-topic-models.Rmd) — “Topic modeling,” “Latent Dirichlet allocation,” “Word-topic probabilities,” “Document-topic probabilities,” “Example: the great library heist,” “LDA on chapters,” “Per-document classification,” “By word assignments: `augment`,” “Alternative LDA implementations,” and “Summary.”

## Chapter 7: Case study—comparing Twitter archives

### Core ideas

This case study combines earlier techniques in an end-to-end comparison of the authors' Twitter archives. It begins by loading timestamps and examining tweet distributions, then compares word frequencies and word usage between the two accounts.

The log odds ratio identifies words that are relatively more likely to be used by one author. The chapter also models changes in word use over time and examines which words are associated with more favorites and retweets. These analyses distinguish total activity, relative vocabulary, temporal change, and engagement.

The case study demonstrates a reusable workflow: tidy the text, preserve author and time identifiers, calculate measures at the correct grain, and interpret multiple measures together.

### Analytics and Power BI example

An exported analytics model could include:

- Tweet-level data with author, timestamp, favorites, and retweets.
- Author-word counts and log odds.
- Word-time trends.
- Word-level engagement summaries.

Power BI could use these tables for an author-comparison page, an activity timeline, a vocabulary-difference chart, and an engagement page. A word slicer could reveal whether usage is increasing or decreasing and how the word's tweets perform. The report should avoid equating high frequency with distinctiveness or high engagement; each is a separate measure.

### Comprehension questions

1. Why must author and timestamp remain attached to the text during preprocessing?
2. What does the log odds ratio reveal that a comparison of raw word counts does not?
3. How can an analyst distinguish a word's changing usage rate from a change in total tweeting activity?
4. Why should favorites and retweets be analyzed separately from word frequency?
5. Which parts of this case-study workflow could be reused for another collection of short, timestamped messages?

### Practical exercise

Use the repository's Twitter archive data and reproduce the author-word comparison. Create an author-word table with counts and log odds, then create either a time-trend or engagement summary for selected words. Export the results to Power BI. Build an author slicer, a vocabulary comparison chart, a word trend, and an engagement table. Use the report to explain one word that is common, one that is distinctive, and one whose use or engagement changes over time.

**Sources:** [07-tweet-archives.Rmd](07-tweet-archives.Rmd) — “Case study: comparing Twitter archives,” “Getting the data and distribution of tweets,” “Word frequencies,” “Comparing word usage,” “Changes in word use,” “Favorites and retweets,” and “Summary.”

## Chapter 8: Case study—mining NASA metadata

### Core ideas

Metadata is data that describes other data. The NASA case study uses dataset titles, descriptions, organizations, and human-assigned keywords to understand relationships among more than 32,000 datasets. The chapter begins with JSON data, wrangles nested fields into tidy structures, and performs initial exploration.

It then applies word co-occurrence and correlation, TF-IDF, and topic modeling. Because titles, descriptions, and keywords describe related aspects of the same datasets, the analysis can compare machine-derived patterns with human-assigned keywords.

The results can help identify related datasets, reveal important keyword combinations, and suggest keywords from description text. The case study demonstrates how multiple text fields and analytical methods can reinforce one another.

### Analytics and Power BI example

An analyst could organize the exported results into dataset metadata, dataset-keyword relationships, description-term TF-IDF, word-pair relationships, and document-topic probabilities.

Power BI could provide:

- A dataset search and detail page.
- Keyword and organization slicers.
- Ranked distinctive description terms.
- A related-keyword matrix.
- Topic filters that surface datasets with high topic probabilities.
- A comparison between assigned keywords and words or topics derived from descriptions.

The report turns text analysis into dataset discovery while keeping the original metadata available for context.

### Comprehension questions

1. What is metadata, and which NASA metadata fields are analyzed in the chapter?
2. Why must nested JSON fields be wrangled before tidy-text methods can be applied?
3. How do keyword co-occurrence and TF-IDF answer different questions about the datasets?
4. How can description text be connected to human-assigned keywords?
5. What potential use does the chapter identify for the topic model's results?

### Practical exercise

Use the chapter's prepared NASA metadata workflow to select a manageable subset of datasets. Produce a dataset table, a keyword relationship table, and either a description TF-IDF table or document-topic table. Export the tables and relate them in Power BI by dataset identifier. Build a discovery page where organization, keyword, and topic or distinctive-term selections filter the dataset list. Add a detail view containing the title, description, assigned keywords, and derived analytical result.

**Sources:** [08-nasa-metadata.Rmd](08-nasa-metadata.Rmd) — “Case study: mining NASA metadata,” “How data is organized at NASA,” “Wrangling and tidying the data,” “Some initial simple exploration,” “Word co-ocurrences and correlations,” “Networks of Description and Title Words,” “Networks of Keywords,” “Calculating tf-idf for the description fields,” “Connecting description fields to keywords,” “Topic modeling,” “Casting to a document-term matrix,” “Interpreting the topic model,” “Connecting topic modeling with keywords,” and “Summary.”

## Chapter 9: Case study—analyzing Usenet text

### Core ideas

The final case study performs a start-to-finish analysis of messages from 20 Usenet newsgroups. Preprocessing must extract message content from files while handling headers, signatures, and the folder structure that identifies each newsgroup.

The analysis then combines most of the book's methods. TF-IDF identifies vocabulary distinctive to each newsgroup; topic modeling finds latent groups; sentiment is analyzed both by word and by message; and n-grams restore context and reveal common word sequences.

Using several methods on the same collection demonstrates that no single measure fully explains a text corpus. Frequency, distinctiveness, topics, sentiment, and word relationships provide complementary views, all of which can be explored using a consistent tidy workflow.

### Analytics and Power BI example

The exported model could contain message metadata, newsgroup-term TF-IDF, document-topic probabilities, message sentiment, and n-gram counts. A multi-page Power BI report could include:

- A corpus overview with message counts by newsgroup.
- A distinctive-vocabulary page based on TF-IDF.
- A topic page showing term and document probabilities.
- A sentiment comparison at both word and message grain.
- An n-gram page showing common contextual phrases.

Shared newsgroup and message identifiers allow selections to carry across the analytical views. Separate fact tables preserve each method's natural grain rather than forcing all results into one oversized table.

### Comprehension questions

1. Which parts of a Usenet file must be separated during preprocessing, and why?
2. What does within-newsgroup TF-IDF reveal about the corpus?
3. How does topic modeling complement known newsgroup labels?
4. Why can word-level and message-level sentiment lead to different interpretations?
5. Why is it useful to apply TF-IDF, topic modeling, sentiment, and n-gram analysis to the same corpus?

### Practical exercise

Select several newsgroups from the repository data and reproduce the chapter's preprocessing. Create message metadata and at least three analytical outputs: newsgroup TF-IDF, message sentiment, and bigram counts. Export each result as a separate table. In Power BI, build an overview plus one page for each method, using shared newsgroup identifiers for cross-filtering. Finish with a short comparison explaining what each method contributes and where two methods produce different perspectives.

**Sources:** [09-usenet.Rmd](09-usenet.Rmd) — “Case study: analyzing usenet text,” “Pre-processing,” “Pre-processing text,” “Words in newsgroups,” “Finding tf-idf within newsgroups,” “Topic modeling,” “Sentiment analysis,” “Sentiment analysis by word,” “Sentiment analysis by message,” “N-gram analysis,” and “Summary.”

