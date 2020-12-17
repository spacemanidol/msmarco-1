# TREC 2021 Deep Learning Track Guidelines

## Timetable

* July-August (exact date TBA): Deadline for submitting runs for document and passage ranking tasks
* November 17-19: TREC conference

## Previous edition

* 2019 [website](https://microsoft.github.io/msmarco/TREC-Deep-Learning-2019) and [overview paper](https://arxiv.org/pdf/2003.07820.pdf)
* 2020 [website](https://microsoft.github.io/msmarco/TREC-Deep-Learning-2020) (overview paper coming soon)

## Registration

To participate in TREC please pre-register at the following website: [https://ir.nist.gov/trecsubmit.open/application.html](https://ir.nist.gov/trecsubmit.open/application.html)

## Introduction

The Deep Learning Track studies information retrieval in a *large training data* regime. This is the case where the number of training queries with at least one positive label is at least in the tens of thousands, if not hundreds of thousands or more. This corresponds to real-world scenarios such as training based on click logs and training based on labels from shallow pools (such as the pooling in the TREC Million Query Track or the evaluation of search engines based on early precision).

Certain machine learning based methods, such as methods based on deep learning are known to require very large datasets for training. Lack of such large scale datasets has been a limitation for developing such methods for common information retrieval tasks, such as document ranking. The Deep Learning Track organised in 2019 aimed at providing large scale datasets to TREC, and create a focused research effort with a rigorous blind evaluation of ranker for the passage ranking and document ranking tasks. 

In 2020, the track will continue to have the same tasks (document ranking and passage ranking) and goals. Similar to the previous year, one of the main goals of the track in 2020 is to study what methods work best when a large amount of training data is available. For example, do the same methods that work on small data also work on large data? How much do methods improve when given more training data? What external data and models can be brought in to bear in this scenario, and how useful is it to combine full supervision with other forms of supervision? 

## Deep Learning Track Tasks

The Deep Learning Track has two tasks: Passage ranking and document ranking; and two subtasks in each case: full ranking and re-ranking. You can submit up to three runs for each of the subtasks.

Each task uses a large human-generated set of training labels, from the [MS MARCO](http://msmarco.org) dataset. The two tasks use the same test queries. They also use the same form of training data with usually one positive training document/passage per training query. In the case of passage ranking, there is a direct human label that says the passage can be used to answer the query, whereas for training the document ranking task we transfer the same passage-level labels to document-level labels.

For both tasks, the participants are encouraged to study the efficacy of transfer learning methods. Our current training labels (from MS MARCO) are generated differently than the test labels (generated by NIST). This year participants also have access to 2019 NIST test labels for validation or traininig. Participants can also use external corpora for large scale language model pretraining, or adapt algorithms built for one task of the track (e.g. passage ranking) to the other task (e.g. document ranking). This allows participants to study a variety of transfer learning strategies.

Below the two tasks are described in more detail.

### Document Ranking Task

The first task focuses on document ranking. We have two subtasks related to this: Full ranking and top-100 re-ranking.

In the full ranking (retrieval) subtask, you are expected to rank documents based on their relevance to the question, where documents can be retrieved from the full document collection provided. You can submit up to **100 documents** for this task. It models a scenario where you are building an end-to-end retrieval system.

In the re-ranking subtask, we provide you with an initial ranking of 100 documents from a simple IR system, and you are expected to re-rank the documents in terms of their relevance to the question. This is a very common real-world scenario, since many end-to-end systems are implemented as retrieval followed by top-k re-ranking. The re-ranking subtask allows participants to focus on re-ranking only, without needing to implement an end-to-end system. It also makes those re-ranking runs more comparable, because they all start from the same set of 100 candidates.

### Passage Ranking Rask

Similar to the document ranking task, the passage ranking task also has a full ranking and re-ranking subtasks.

In context of full ranking (retrieval) subtask, given a question, you are expected to rank passages from the full collection in terms of their likelihood of containing an answer to the question. You can submit up to **1,000 passages** for this end-to-end retrieval task.

In context of top-1000 re-ranking subtask, we provide you with an initial ranking of 1000 passages and you are expected to re-rank these passages based on their likelihood of containing an answer to the question. In this subtask, we can compare different re-ranking methods based on the same initial set of 1000 candidates, with the same rationale as described for the document re-ranking subtask.

### Use of external information

You are allowed to use external information while developing your runs. When you submit your runs, please fill in a form listing what resources you used. This could include an external corpus such as Wikipedia or a pre-trained model (e.g. word embeddings, BERT). This could also include the provided set of document ranking training data, but also optionally other data such as the passage ranking task labels or external labels or pretrained models. This will allow us to analyze the runs and break they down into types.

IMPORTANT NOTE: It is prohibited to use any datasets from msmarco.org in your submission except those listed below. The original MS MARCO dataset reveals some minor details of how they were constructed that would not be available in a real-world search engine; hence, should be avoided.

### Datasets

We are in the process of finalizing the datasets for TREC 2021 Deep Learning track. This section will be updated soon. Please check back soon!

In the meantime, you can find the datasets from the last year's track here: [https://microsoft.github.io/msmarco/TREC-Deep-Learning-2020](https://microsoft.github.io/msmarco/TREC-Deep-Learning-2020)

## Submission, evaluation and judging

We will be following a similar format as the ones used by most TREC submissions, which is repeated below. White space is used to separate columns. The width of the columns in the format is not important, but it is important to have exactly six columns per line with at least one space between the columns.

```text
1 Q0 pid1    1 2.73 runid1
1 Q0 pid2    1 2.71 runid1
1 Q0 pid3    1 2.61 runid1
1 Q0 pid4    1 2.05 runid1
1 Q0 pid5    1 1.89 runid1
```

, where:

* the first column is the topic (query) number.
* the second column is currently unused and should always be "Q0".
* the third column is the official identifier of the retrieved passage in context of passage ranking task, and the identifier of the retrieved document in context of document ranking task.
* the fourth column is the rank the passage/document is retrieved.
* the fifth column shows the score (integer or floating point) that generated the ranking. This score **must** be in descending (non-increasing) order.
* The sixth column is the ID of the run you are submitting.

As the official evaluation set, we provide a set of test queries, where a subset will be judged by NIST assessors. For this purpose, NIST will be using depth pooling and construct separate pools for the passage ranking and document ranking tasks. Passages/documents in these pools will then be labelled by NIST assessors using multi-graded judgments, allowing us to measure NDCG. The same test queries are used for passage retrieval and document retrieval.

Besides our main evaluation using the **NIST labels and NDCG**, we also have sparse labels for the test queries, which already exist as part of the MS-Marco dataset. More information regarding how these sparse labels were obtained can be found at <https://arxiv.org/abs/1611.09268>. This allows us to calculate a secondary metric **Mean Reciprocal Rank (MRR)**. For the full ranking setting, we also compute **NCG** to evaluate the performance of the candidate generation stage.

The main type of TREC submission is _automatic_, which means there was not manual intervention in running the test queries. This means you should not adjust your runs, rewrite the query, retrain your model, or make any other sorts of manual adjustments after you see the test queries. The ideal case is that you only look at the test queries to check that they ran properly (i.e. no bugs) then you submit your automatic runs. However, if you want to have a human in the loop for your run, or do anything else that uses the test queries to adjust your model or ranking, you can mark your run as _manual_. Manual runs are interesting, and we may learn a lot, but these are distinct from our main scenario which is a system that responds to unseen queries automatically.

## Coordinators

Nick Craswell (Microsoft), Bhaskar Mitra (Microsoft & UCL), Emine Yilmaz (UCL) and Daniel Campos (Microsoft)

# Contributing

This project welcomes contributions and suggestions.  Most contributions require you to agree to a
Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us
the rights to use your contribution. For details, visit https://cla.opensource.microsoft.com.

When you submit a pull request, a CLA bot will automatically determine whether you need to provide
a CLA and decorate the PR appropriately (e.g., status check, comment). Simply follow the instructions
provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.

# Terms and Conditions

The MS MARCO datasets are intended for non-commercial research purposes only to promote advancement in the field of artificial intelligence and related areas, and is made available free of charge without extending any license or other intellectual property rights. The dataset is provided “as is” without warranty and usage of the data has risks since we may not own the underlying rights in the documents. We are not be liable for any damages related to use of the dataset. Feedback is voluntarily given and can be used as we see fit. By using any of these datasets you are automatically agreeing to abide by these terms and conditions. Upon violation of any of these terms, your rights to use the dataset will end automatically.

Please contact us at ms-marco@microsoft.com if you own any of the documents made available but do not want them in this dataset. We will remove the data accordingly. If you have questions about use of the dataset or any research outputs in your products or services, we encourage you to undertake your own independent legal review. For other questions, please feel free to contact us.

# Legal Notices

Microsoft and any contributors grant you a license to the Microsoft documentation and other content
in this repository under the [Creative Commons Attribution 4.0 International Public License](https://creativecommons.org/licenses/by/4.0/legalcode),
see the [LICENSE](LICENSE) file, and grant you a license to any code in the repository under the [MIT License](https://opensource.org/licenses/MIT), see the
[LICENSE-CODE](LICENSE-CODE) file.

Microsoft, Windows, Microsoft Azure and/or other Microsoft products and services referenced in the documentation
may be either trademarks or registered trademarks of Microsoft in the United States and/or other countries.
The licenses for this project do not grant you rights to use any Microsoft names, logos, or trademarks.
Microsoft's general trademark guidelines can be found at <http://go.microsoft.com/fwlink/?LinkID=254653>.

Privacy information can be found at <https://privacy.microsoft.com/en-us/>.

Microsoft and any contributors reserve all other rights, whether under their respective copyrights, patents,
or trademarks, whether by implication, estoppel or otherwise.