# Recommendation Systems

## Table of Content
Introduction (10 min)
- [What are recommendations?](#what-are-recommendations)
- [Why recommendations?](#why-recommendations)
- [Terminologies](#terminologies)
- [Recommendations System Overview](#recommendation-systems-overview)
- [Short Quiz 1](#short-quiz-1)

Candidate Generation (35 min)
- [Basic Concepts](#basic-concepts-embedding-space-and-similarity-measure)
- [Short Quiz 2](#short-quiz-2)
- [Content-based Filtering](#content-based-filtering)
- [Short Quiz 3](#short-quiz-3)
- [Collaborative Filtering](#collaborative-filtering)
- [Short Quiz 4](#short-quiz-4)
- [Recommendations using DNNs](#recommendations-using-deep-neural-networks)
- [Retrieval](#retrieval)

Scoring (20 min)
- [Objective Function](#objective-function-for-scoring)
- [Positional Bias](#positional-bias-in-scoring)

Reranking (20 min)
- [Challenges](#challenges)

Conclusion (20 min)
- [Key Takeaways](#key-takeaways)
- [Demo](#demo)

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

Using the image, try to determine the item ranking using all three of the similarity measures: cosine, dot product, and Euclidean distance. **(Note: Instructor is recommended to demo the solution.)**

### Content-based Filtering
Content-based filtering uses **item features to recommend other items similar to what the user likes, based on their previous actions or explicit feedback**.

#### How does content-based filtering work?
Let's use the apps in Google Play Store as a demo.  The following figure shows a feature matrix where each row represents an app (see **first 3 rows**) and each column represents a feature. To simplify, assume this feature matrix is binary i.e. `[0, 1, ..., 1]`: a non-zero value means the app has that feature - i.e. the first app has the **Education** and **Science R Us** features.

![Apps in Google Play Store vs User](https://developers.google.com/machine-learning/recommendation/images/Matrix1.svg)

You also represent the user in the same feature space (see **final row**). Some of the user-related features could be explicitly provided by the user.

Your objective is that the model should recommend items relevant to this user:
1. Pick a similarity metric (for example, dot product)
2. Set up the system to score each candidate item according to this similarity metric

**Note: Recommendations are specific to this user, as the model did not use any information about other users.**

Based on the feature matrix above and using dot product $\sum_{n=1}^{d} x_i y_i$, and let user embedding as $x$ while app embedding as $y$ (binary vectors) - if the feature both appearing in $x$ and $y$, then `1 * 1 = 1`.
**(Note: Instructor is recommended to demo the solution.)**

#### The Good and the Bad
| The Good | The Bad |
| --- | --- |
| Easily scalable | Recommendations limited to users' existing interests |
| Can capture specific interests of users | Requires a lot of domain knowledge for feature engineering |

## Short Quiz 3
Calculate the dot product for each app in the preceding app problem. Which app should we recommend?
- A. The casual app created by TimeWastr.
- B. The educational app created by Science R Us. (Answer)
- C. The health app created by Healthcare.

### Collaborative Filtering
Collaborative filtering uses **similarities between users and items simultaneously to provide recommendations**. Hence collaborative filtering can recommend an item to user A based on the interests of similar user B.

![Collaborative filtering](https://miro.medium.com/max/720/1*QvhetbRjCr1vryTch_2HZQ.jpeg)

From the figure above, notice that there are two types of collaborative filtering: user-based and item-based. Let's look at user-based first.

With user-based collaborative filtering, the model is trying to do "given user A and user B have similar interests, the things that user A is interested should be relevant to user B":
1. Let the following be the situation:
    - The girl in red is interested in grape, strawberry, watermelon and orange.
    - The girl in blue is only interested in strawberry.
    - The boy in purple is interested in strawberry and watermelon. 
2. The model thinks that the boy in purple is similar to girl in red, hence it recommends grape and orange to the boy.

With item-based collaborative filtering, the model is trying to do "given item A and item B have similar characteristics, if someone likes item A then probably they like item B too":
1. Let the following be the situation:
    - The girl in red is interested in grape, watermelon and orange.
    - The girl in blue is interested in grape and watermelon.
    - The boy in purple is interested in watermelon.
    - Grape and watermelon is similar in some ways.
2. The model thinks that the boy in purple would like grape, given that grape and watermelon is similar in some ways.

#### The Good and the Bad
| The Good | The Bad |
| --- | --- |
| No domain knowledge necessary | Cannot handle fresh items during production |
| Can help users discover new interests | Hard to include side features |

## Short Quiz 4
The model recommends a shopping app to a user because they recently installed a similar app. What kind of filtering is this an example of?
- A. Collaborative filtering
- B. Content-based filtering (Answer)

### Recommendations using Deep Neural Networks
Collaborative filtering learn embeddings via matrix factorization. This method is limited in terms of:
1. Hard to include side features
2. Popular items tend to be recommended for everyone

DNN models can address these limitations of matrix factorization - easily incorporate query features and item features (due to the flexibility of the input layer of the network), which can help capture the specific interests of a user and improve the relevance of recommendations.

For the sake of time, we will not touch DNNs further. You can find out further [here](https://developers.google.com/machine-learning/recommendation/dnn/softmax).

### Retrieval
After candidate generation (means that we have an embedding model), the system can do two (2) things at serve time:
1. Matrix factorization - query embedding is known statically: Simply look up query embedding from embedding matrix
2. DNN model - query embedding unknown: Simply compute query embedding at serve time (inference)

Now that we have:
1. Query embedding, $q$ - represented by blue circle below;
2. Item embeddings, $V_j$ - represented by green circle below;

then we can return top K items using similarity score $s(q, V_j)$.

![Retrieval](https://developers.google.com/machine-learning/recommendation/images/2Dretrieval.svg)

Process is the same for related-item recommendations.

### Scaling Up
To find nearest neighbours, the system can exhaustively score every potential candidate - but it can be very expensive for large corpora. Possible workarounds:
1. Statically-known query: Offline exhaustive scoring -> precompute and store list of top candidates
2. Unknown query: Approximate nearest neightbour method

## Scoring
Generated candidates can come from multiple candidate generators using different sources:
- Related items
- User features
- Geographic info
- Popular or trending items
- Social graph (liked or recommended by friends)

Then, 
1. These sources are combined into a common pool of candidates, like:
    - Get personalization info
    - Get trending items in respective area
2. Scored by single model and ranked according to score, like:
    - Model predict the probability of user watching a video using query and video features
    - Items ranked according to probability of watch

### Objective Function for Scoring
Choice of objective function can affect rankings and subsequently quality of recommendations:
1. **Maximize click rate** - may recommend click-bait videos
2. **Maximuze watch time** - may recommend very long videos
3. **Increase diversity and maximize watch time** - may recommend short and engaging videos

### Positional Bias in Scoring
Items that appear lower on the screen are less likely to be clicked than items appearing higher on the screen. Querying the model with all possible positions is too expensive and the system still might not find a consistent ranking across multiple ranking scores:
1. Create position-independent rankings
2. Rank all candidates as if they are in the top position on the screen

## Re-ranking
Re-ranking can improve the recommendations by considering additional criteria or constraints:
1. **Use filters to remove some candidates** - i.e. removing click-baits:
    - Training a separate model that detects whether a video is click-bait.
    - Running this model on the candidate list.
    - Removing the videos that the model classifies as click-bait.
2. **Transform the score returned by the ranker** - i.e. modifying score:
    - As a function of video age (promote fresher content)
    - As a function of video length (increase viewing time)

### Challenges
1. **Freshness**: Aim to incorporate the latest usage information, e.g. current user history and the newest items
    - Warm-start and re-run training as often as possible
    - Create an "average" user to represent new users in matrix factorization models
    - Use a DNN such as a softmax model or two-tower model
    - Add document age as a feature
2. **Diversity**: Lack of diversity can cause a bad or boring user experience
    - Train multiple candidate generators using different sources
    - Train multiple rankers using different objective functions
    - Re-rank items based on genre or other metadata
3. **Fairness**: Treat all users fairly and reduce unconscious bias in data
    - Include diverse perspectives in design and development
    - Train ML models on comprehensive data sets and add auxilliary data when certain groups are underrepresented
    - Track metrics on each demographic to watch for biases
    - Make separate models for underserved groups

## Key Takeaways
The key takeaways are:
1. Recommendations are the suggestions served up based on other things the user (e.g. you) like and they allow users to get in touch with compelling content in large corpora, particularly items that the user might not have thought to search for their own.
2. Recommendations can be performed via three (3) steps:
    - Candidate generation
    - Scoring
    - Re-ranking
3. Items and queries can be represented using embeddings.
4. Candidate generation is performed via:
    - Content-based filtering
    - Collaborative filtering (user or item-based)
    - Deep neural networks
5. Scoring can be performed for:
    - Maximizing click rate, but might promote click-baits
    - Maximuzing watch time, but might promote long videos
    - Both increase diversity and maximize watch time
6. Re-ranking can be performed via:
    - Filtering to remove candidates - i.e. click-baits
    - Transformt the score - i.e. fresher content
7. Challenges for a recommendation system:
    - Freshness of model
    - Diversity of content
    - Fairness for users

## Demo
Please visit this [Streamlit app](https://share.streamlit.io/mnobeidat13/handm-recommender-system/main) for the recommendation system demo.

At **Try** section (sidebar):
1. *Find similar items* is an example of content-based filtering.
2. *Customer recommendations* is an example of user-based collaborative filtering.

**Have fun with it and share your findings!**