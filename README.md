# Named-Entity-Relation-Extraction
This project extracts different syntactic, lexical and contextual features of pairs of entities, which are then used to train models to identify the semantic relation, as well as the direction of the relation, within a pair of entities in a sentence. Specifically, the entity relations that this project is designed to identify are:

1. Cause-Effect
2. Instrument-Agency
3. Product-Producer
4. Content-Container
5. Entity-Origin
6. Entity-Destination
7. Component-Whole
8. Member-Collection
9. Message-Topic
10. No Relation (a special class that does not have any direction)

## Feature Extraction
The following features are extracted from each sentence using [SpaCy](https://github.com/explosion/spaCy) and [NLTK](https://github.com/nltk/nltk) libraries, where e1 denotes the first entity and e2 denotes the second entity:
- **Tokens** or words in the sentence
- **Lemma** of each word in the sentence
- **Part of Speech (POS) Tag** of each word in the sentence
- **Dependency Parse Tag** of each word in the sentence and its head
- **Dependency Parse Part of Speech Tag** of the head of each word in the sentence
- **Shortest Dependency Path** between entities if direction is from e1 to e2, else e2 to e1
- **Length of the Shortest Dependency Path** between e1 and e2
- **Hypernym, Hyponym, Meronym, Holonym** of each entity
- **WordNet Path Similarity** between the synsets of e1 and e2
- **Named Entity Recognition (NER) Tag** of each entity
- **Stem** of each entity
- **Prefixes of length 5** of the words between e1 and e2
- **Word before e1**
- **Word after e2**
- **POS Tags** of the words between e1 and 2
- **Count of words** between e1 and e2
- **Gloss**: Given a pair of annotated nominals, e1 and e2, these features are set to 1 every time either e1 appears in the gloss of e2, or vice-versa.

Additionally, the **POS tag** of each entity is determined from the **POS tags** of all the words in a sentence and the **shortest dependency path length** is determined from the **shortest dependency path** between the entities in a sentence.

Finally, the following features are selected for training the models to achieve the best performance:
- Shortest Dependency Path
- Length of the Shortest Dependency Path
- e1
- e2
- POS tag of e1
- POS tag of e2
- Prefixes of length 5 between e1 and e2
- Word before e1
- Word after e2
- Count of the words between e1 and e2
- POS tags of the words between e1 and e2
- Hypernyms, hyponyms, holonyms and meronyms of the entites

## Models and Accuracy
The identification of the entity relation and the identification of the relation direction, i.e. whether the relation is from entity1 to entity2 or vice-versa, are separated into two different classification tasks. The TAC KBP dataset reduced to just 10 relation classes (dataset renamed to "SemEval") is used as the training corpus for the classification tasks. 

The following set of Machine Learning models are trained on the selected features to classify the type of semantic relation present between two entities in a given sentence:
- A baseline Bag-of-Words model
- A Decision Tree model
- A Random Forest Model
- A Guassian Naive Bayes model
- A Voting Classifier model that combines the predictions from the Decision Tree, Random Forest and Gaussian Naive Bayes models

After training the models on how to classify the semantic relation between two entities, the following set of Machine Learning models are trained on the same set of features to identify the direction of the relation:
- A Random Forest Model
- A Voting Classifier model that combines the predictions from a Decision Tree, a Random Forest and a Gaussian Naive Bayes model

It was observed that for the classification of the semantic relation between two entities, the Voting Classifier model performed the best, achieving an accuracy of 61.87%. For the classification of the direction of the semantic relation, the Voting Classifier model performed the best as well, achieving an accuracy of 92.36%.

*Refer to "report.pdf" for more details*

## Running the code
1. Open the .ipynb file in google colab
2. Run 1st cell and load all the files required i.e. semeval_train, semeval_test set, demo file and pre loaded features csv files (if available)
3. Import all the libraries in the next cell
4. If the features for the train and test sets are available directly go to step 7 or continue with step 5
5. Run cells in order to read data files and represent it in a dataframe
6. Run all the cells in task 2 in order (takes time to generate all the features)
7. Once the features are available, run all cells in task 3 and run the models (save pickle files if necessary)
8. For performance evaluation run cells in task 4
9. For demo upload a text file containing example sentence and relation (using first cell in notebook or using other possible ways in colab)
10. Run the cells in demo part to get predictions for given sentence
