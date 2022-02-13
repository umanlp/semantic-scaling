---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
---


This is the online appendix for the paper **Political Text Scaling Meets Computational Semantics** [\[working draft\]](https://arxiv.org/abs/1904.06217) by *Federico Nanni, Goran Glavaš, Ines Rehbein, Simone Paolo Ponzetto and Heiner Stuckenschmidt*.

The paper introduces SemScale, a tool for text scaling aware of the semantics of the documents under study. You can use our [command-line tool](https://github.com/umanlp/SemScale), implemented in Python.

# Main Experiments

## EuroParl Dataset

In our work, we have obtained a dataset from the European Parliament website. It comprises all speeches given in either English, German, French, Italian or Spanish by members of the European Parliament (MEP) during the 5th and 6th legislative terms and translated to all of the other four languages. For each legislation and language, we concatenated all speeches of all members of the same national party in a single textual document, aiming to discover the overall political positions of the party during the legislation. The party-document ids match the ones used by the Chapel Hill expert survey. In [this folder](https://drive.google.com/drive/folders/1ZmLDuCy4u0PvZCJRKd4dSgtE7U36stRW), you will find for each legislation the concatenated party-speeches in each language, together with Chapel Hill positioning of parties during that legislations (scaled between 0 and 1), concerning the European integration dimension and overall left-right ideology. <ins>Note</ins> that the results are computed considering documents in the dataset for which a positional score is present in the integration and ideology files. We are currently working on expanding such files providing more party positioning information.

## Pre-processing tools adopted

For part-of-speech tagging, lemmatization and named entity recognition we have used [Spacy](https://spacy.io). The tool is very easy to use and well documented; you simply need to download the [model](https://spacy.io/models) of the language under study.

For entity-linking we adopted DBpedia Spotlight, as it is the only tool that currently openly supports the five languages under study. As opposed to Spacy, using Spotlight is not quite straight-forward, as it is not extensively [documented](https://github.com/dbpedia-spotlight/dbpedia-spotlight-model) and to run it on your machine you would need to 1) install [Docker](https://docs.docker.com/get-docker/), 2) load the [models](https://github.com/dbpedia-spotlight/spotlight-docker), 3) [send](https://stackoverflow.com/questions/50735033/how-to-use-dbpedia-spotlight-docker-image) texts to be annotated via CURL. Nevertheless, if you work on either English, German or Italian you could use another entity-linker, [TagMe](https://web.archive.org/web/20211015085807/https://services.d4science.org/web/tagme/tagme-help), which is simpler to import in your pipeline.

Due to the above issues, we offer a [.zip folder](https://drive.google.com/file/d/1Ob3OVU8EqW-TdBoA3al63uQxlvsJPBtd/view?usp=sharing)  (around 700mb) containing the entire dataset already processed as a colleciton of enriched json files. Each file, in each language, contains the output of Spacy plus isolated NERs (PER, ORG, LOC) and linked entities (“ents”). For each entity, you will have the DBpedia entry and its mention in the document. This way, our evaluation setting can be reproduced without the need of employing Spacy or DBpedia Spotlight. To parse the dataset, we offer simple [Python](https://drive.google.com/file/d/1yUW8tsvDDPOzbKO-daYvw1EHgrtV4dbh) and [R scripts](https://drive.google.com/file/d/1zLVhKXncA1gLrWC6-gyI3zCqYawqDs8P).
Scaling tools

[SemScale](https://github.com/umanlp/SemScale) is implemented in Python. When using the tool, you can import the word embeddings of your choice (as a language-prefixed file – see the documentation on Github). In our work, we used a)  (prefixed) in-domain embeddings [(a)](https://drive.google.com/file/d/1yPVJwez-I4xc6NdR8603-I_mKi6m4ul6), [(b)](https://drive.google.com/open?id=1WyapadDr7vRgJVgA_MIylRIc_XZW1tNf) trained using the entire collection of speeches for each language and legislation as corpus, which we share with the community [in-domain-embs; in-domain-lemma-embs] and b) pre-trained general purpose [FastText embeddings](https://fasttext.cc/docs/en/crawl-vectors.html) (300d), for each language under study. For testing purposes, we provide you with a [single file](https://drive.google.com/file/d/1Oy61TV0DpruUXOK9qO3IFsvL5DMvwGwD), containing the 100k most-frequently used (prefixed) words on Wikipedia for the following languages: English, French, German, Italian and Spanish. Be aware that by employing this file you are limiting the size of the vocabulary under study, but will drastically speed-up the analysis.

[Wordfish](https://tutorials.quanteda.io/machine-learning/wordfish). We have used the R implementation of Wordfish, available in Quanteda, keeping all characters (i.e, flagging out number and punctuation removal and keeping all words when building the word-frequency matrix). Note that this process is computationally quite expensive, depending on the size of the documents under study.

## Evaluation script

To compute pairwise accuracy, Pearson and Spearman correlation, we provide a simple [Python script](https://drive.google.com/file/d/15WInytvxmURO1cuudeFlltcouWzZ48po) that you can use as a command-line tool. You just need to provide the path of the scaling file and the Chapel Hill party positions, concerning either European integrations or left-right ideology. For instance:

python eval-script.py EuroParl-Dataset/5/integration.txt scaling-results/semscale.txt

## Further Evaluation

We have also tested SemScale in other three different evaluation settings.

## Different Text Representations

In this folder you can find all results obtained using [TF-IDF](https://drive.google.com/open?id=1_uF0bpDp0W2EtUa-RIsBZPTzdMAQXk6J) and [Party2Vec](https://drive.google.com/drive/folders/1s322wgNWgESiU_JDgBKQZeu5HyljsmSu) as input features for SemScale over the EuroParl datasets.

## Scaling Manifestos

Here you can find [all results](https://drive.google.com/drive/folders/1VeQREcJWP9ykJlObF0qAg9lMC8m8oBnN) obtained when scaling manifestos from a [single election](https://drive.google.com/drive/folders/1nUI94BMWMvbM4gaxJ_fZznRvMVZ3vNUJ) and from [multiple elections](https://drive.google.com/drive/folders/11EaIG2QnXlHPwcyxzi4IN3OArz9N5fuY), together with the related RILE positions.

## Supervised Scaling

Here you can find [all results](https://drive.google.com/drive/folders/1or10awO_pZ8xlTTLvTlPoKybjbpshDY0) obtained when comparing [Wordscores](https://tutorials.quanteda.io/machine-learning/wordscores) and our supervised version of SemScale, that we dubbed [SemScores](https://github.com/umanlp/SemScale#other-functionalities).


