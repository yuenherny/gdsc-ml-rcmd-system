# Recommendation Systems

## Table of Content
Introduction (10 min)
- [What are recommendations?](#what-are-recommendations)
- [Why recommendations?](#why-recommendations)
- [Terminologies](#terminologies)
- [Recommendations System Overview](#recommendation-systems-overview)
- [Short Quiz 1](#short-quiz-1)

Candidate Generationv (20 min)
- [Basic Concepts](#basic-concepts-embedding-space-and-similarity-measure)
- [Short Quiz 2](#short-quiz-2)

## What are Recommendations?
Recommendations are the suggestions served up based on other things the user (e.g. you) like.

## Why Recommendations?
Recommendations allow users to get in touch with compelling content in large corpora, particularly items that the user might not have thought to search for their own.

## Terminologies
Few terminologies to know:
1. Items
    - A.K.A documents
    - For YouTube / Netflix, the items are videos and shows
2. Query:
    - A.K.A context
    - The information used to make recommendations
    - Can be a combination of user information (i.e. user ID, user history) and additional context (i.e. time of day, user device)
3. Embedding
    - A.K.A mapping to vector space
    - A mapping from a discrete set (set of items) to an embedding space

## Recommendation Systems Overview
A recommendation system consists of:
1. Candidate generation
    - Generates a smaller subset of candidates from large corpus
    - E.g. YouTube candidate generator reduces billions of videos down to thousands
2. Scoring
    - Scores and ranks candidates in order
    - Selects a set of sorted items to display to user
3. Re-ranking
    - Take into account additional constraints for final ranking
    - E.g. removing disliked items or boost fresher content

![Recommendation System Overview](https://developers.google.com/machine-learning/recommendation/images/Process.svg)

## Short Quiz 1
1. Why wouldn't you use recommendations systems?
    - A. You want to direct users to sponsored items.
    - B. You think you have to sprinkle ML on everything.
    - C. Having recommendation engine makes browsing content easier. (Answer)
2. What are the primary components of a recommender system?
    - A. Embedding, similarity metrics and serving
    - B. Matrix factorization, DNN and re-ranking
    - C. Candidate generation, scoring and re-ranking (Answer)

## Candidate Generation
This is the first stage of recommendation: given a query, a set of relevant candidates are generated. Two common approaches are:
1. Content-based Filtering: 
    - Uses **similarity between items** to recommend items similar to what the user likes.
2. Collaborative Filtering
    - Uses **similarities between queries and items simultaneously** to provide recommendations.

![Recommendation approaches](https://miro.medium.com/max/998/1*O_GU8xLVlFx8WweIzKNCNw.png)

### Basic Concepts: Embedding Space and Similarity Measure
Visit [here](http://projector.tensorflow.org/) for interactive tool to visualize embeddings and similarity measure.

In short:
1. Embedding space is is a low-dimensional space which captures some latent structure of item or query set.
2. Similarity measure is a function that takes a pair of embeddings and returns a scalar measuring their similarity. Some common measures are:
    - Cosine: Cosine of angle between two vectors
    - Dot product: The cosine of angle multipled by product of norms
    - Euclidean distance: Usual distance in Euclidean space

**Comparing similarity measures**
1. Dot product
    - Sensitive to norm
    - Larger norm, higher rank
2. Cosine
    - Sensitive to angle
    - Smaller angle, higher rank
3. Euclidean distance
    - Sensitive to distance
    - Smaller distance, higher rank

## Short Quiz 2
Consider the example in the figure below. The black vector illustrates the query embedding. The other three embedding vectors (Item A, Item B, Item C) represent candidate items. Depending on the similarity measure used, the ranking of the items can be different.

![Short Quiz 2](https://developers.google.com/machine-learning/recommendation/images/Similarity.svg)

Using the image, try to determine the item ranking using all three of the similarity measures: cosine, dot product, and Euclidean distance.

### Content-based Filtering

### Collaborative Filtering

## Retrieval

## Scoring

## Re-ranking

## Key Takeaways

## Short Quiz

## Demo