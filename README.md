
# Product Launch Dataset

*Who launches what and when?*

<br>

Dennis M. Schuler, July 2020


---
<br>


This dataset covers announcements of **product launches**. It builds on a natural language processing (NLP) engine that searches news from the [Media Cloud API](https://mediacloud.org/), and thus, from more than 25 million online sources worldwide. Based on [dependency parsing](https://spacy.io/usage/linguistic-features#dependency-parse) and entity matching, the engine detects new product announcements, links companies, and extracts product mentions from source text.


<br>

## Why product launch announcements?

Product launches reflect companies' innovative capabilities. By tracking product launch announcements, it is possible to capture not only their number/volume but also their sentiment. Both factors show how the market perceives a new product and signal its expected value. Timely and reliable data on product launch announcements can thus serve as a powerful, forward-looking indicator of corporate performance.

<br>

## Data pipeline

At the heart of the data pipeline is an NLP engine that screens millions of media sources on a continuous basis. The engine is designed to identify product launch announcements from English-language news. When it detects an announcement, it extracts the name of the company and the description of the product mentioned.

The pipeline then performs a sentiment analysis and matches extracted companies to entries in the [Legal Entity Identifier (LEI) database][1]. This enables users to query the data by ISIN or LEI codes.

[1]: https://www.gleif.org/en/


![data_pipeline](https://github.com/schuler-d/product-launch-dataset/raw/master/images/data%20pipeline.PNG "Data pipeline")


## Data structure

Product launch announcements are stored as individual records with the following fields:

Field | Type | Description
:--- | :--- | :---
`_id`| string | Unique identifier of the product launch announcement record
`timestamp`| datetime | Timestamp of when the record was added to the database (UTC)
`story_id` | string | Unique identifier of the media story in which the announcement was made
`media_url` | string | URL of the media source
`parsed_company_names` | list of strings| Extracted company names
`launch_description` | string | Extracted text span referring to the product launch / market introduction
`sentiment_positivity` | float | Estimate of positive sentiment ranging from -1 (low positivity) to 1 (high positivity)
`sentiment_objectivity` | float | Estimate of objectivity ranging from -1 (low objectivity) to 1 (high objectivity)
`tense_category` | string | Approximate indicator of the tense (signaling the time of launch relative to the time of the announcement). Four values are possible: "future", "past", "recent past", and "other" (residual category)
`lei_identifiers_matched` | list of strings | Legal Entity Identifiers (LEI) matched to `parsed_company_names`
`lei_identifiers_uplinked` | list of strings | Field that enables users to query the product launch announcements of a company and those of its subsidiaries. It lists the LEI codes of `lei_identifiers_matched` plus the codes of the respective parent companies (both intermediate and ultimate parents) as provided in the [LEI "who owns whom" data][1].

Users can query the data by these fields or by **ISIN codes**. This is possible due to the [ISIN-to-LEI mapping][2] covering both current and historical ISIN codes.

[1]: https://www.gleif.org/en/about-lei/common-data-file-format/relationship-record-cdf-format
[2]: https://www.gleif.org/en/lei-data/lei-mapping/download-isin-to-lei-relationship-files

<br>

## Sample records

https://github.com/schuler-d/product-launch-dataset/blob/master/README_JNB.ipynb


## Coverage

As of July 2020, the dataset covers **2.4 million product launches** parsed from English-language news. Most of the records are based on news published after 2014.

<br>

## Data access

The full dataset is hosted on the Google Cloud Platform. Please contact dennis.schuler@protonmail.com in case you are interested in using the data for research or a commercial project.

