
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


```python
import pandas as pd
from IPython.display import display

# download and display records
url = "https://github.com/schuler-d/product-launch-dataset/raw/master/data-samples/sample_records_small.json"
records = pd.read_json(url, orient="records")
records["lei_identifiers_matched"] = "[...]"
records["lei_identifiers_uplinked"] = "[...]"
display(records.T.style.set_caption(None))
```


<style  type="text/css" >
</style><table id="T_211651ac_cc13_11ea_a7ba_4889e732b477" ><thead>    <tr>        <th class="blank level0" ></th>        <th class="col_heading level0 col0" >0</th>        <th class="col_heading level0 col1" >1</th>        <th class="col_heading level0 col2" >2</th>        <th class="col_heading level0 col3" >3</th>        <th class="col_heading level0 col4" >4</th>        <th class="col_heading level0 col5" >5</th>        <th class="col_heading level0 col6" >6</th>        <th class="col_heading level0 col7" >7</th>        <th class="col_heading level0 col8" >8</th>        <th class="col_heading level0 col9" >9</th>        <th class="col_heading level0 col10" >10</th>        <th class="col_heading level0 col11" >11</th>        <th class="col_heading level0 col12" >12</th>        <th class="col_heading level0 col13" >13</th>        <th class="col_heading level0 col14" >14</th>        <th class="col_heading level0 col15" >15</th>        <th class="col_heading level0 col16" >16</th>        <th class="col_heading level0 col17" >17</th>        <th class="col_heading level0 col18" >18</th>        <th class="col_heading level0 col19" >19</th>        <th class="col_heading level0 col20" >20</th>        <th class="col_heading level0 col21" >21</th>        <th class="col_heading level0 col22" >22</th>        <th class="col_heading level0 col23" >23</th>        <th class="col_heading level0 col24" >24</th>        <th class="col_heading level0 col25" >25</th>        <th class="col_heading level0 col26" >26</th>        <th class="col_heading level0 col27" >27</th>        <th class="col_heading level0 col28" >28</th>    </tr></thead><tbody>
                <tr>
                        <th id="T_211651ac_cc13_11ea_a7ba_4889e732b477level0_row0" class="row_heading level0 row0" >_id</th>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row0_col0" class="data row0 col0" >1882080860.0_7742bc9786cd51b2d8f350134e48bbc1</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row0_col1" class="data row0 col1" >2152788158_290763096976654b8e4122600df74fcb</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row0_col2" class="data row0 col2" >2697092730_9aacc7e1e5f1a7d6d359e8541c817935</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row0_col3" class="data row0 col3" >1031576328.0_8c317376d71d68a2cfca641d3a4ed500</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row0_col4" class="data row0 col4" >2643278798_fb2b185383e8cf135753df0396e9b305</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row0_col5" class="data row0 col5" >2945702628_50a0eb86767ce374d1da346366e71278</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row0_col6" class="data row0 col6" >1772565236.0_7ea60226269ca56fd5d818477ae91b78</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row0_col7" class="data row0 col7" >2643276430_86608b767e8121cf835d5fc7de29af2f</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row0_col8" class="data row0 col8" >2712994768_bc57aa0c7f0f10ddd9ee6b351c299b0f</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row0_col9" class="data row0 col9" >3100557178_24fc513922187fe0105a1e32ef874729</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row0_col10" class="data row0 col10" >1921324056.0_2fcc4cb5d6c525d816803065644a2ca5</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row0_col11" class="data row0 col11" >2982714842_2a76d4f8ecc62a637e7c0f84fbb94d44</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row0_col12" class="data row0 col12" >1265841482.0_fb3e7f1914207b51752588797671f3ef</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row0_col13" class="data row0 col13" >2981192220_934e6ae591e3ddce38e361269e3f63c8</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row0_col14" class="data row0 col14" >2693217348.0_442767323cb75b9281a275bdbd2644c2</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row0_col15" class="data row0 col15" >2782465396_f67f0ef13272a49e798b39117d540213</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row0_col16" class="data row0 col16" >2189567610.0_6551d525920c7616687bfc509c5814e1</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row0_col17" class="data row0 col17" >638702598.0_71388897885796697274e1a279f1a524</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row0_col18" class="data row0 col18" >3002580168_e34b46c9bdedbcc8b8ca720fcf74d6ae</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row0_col19" class="data row0 col19" >1226599398.0_43ef1230bab7a262ea39b4b4fe7e28d3</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row0_col20" class="data row0 col20" >1072631634.0_3a17ec775f33edf2135a6f048299af75</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row0_col21" class="data row0 col21" >3043085590_586ee25f550bebe46469c7ef04051411</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row0_col22" class="data row0 col22" >2090370090.0_054cfd5afe2c3a3f84ab634ad737346f</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row0_col23" class="data row0 col23" >1848070972.0_cafea807def69598edac1f4810a65c63</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row0_col24" class="data row0 col24" >692641368.0_0efa9df860a380d093cd00c2b008387d</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row0_col25" class="data row0 col25" >1251122972.0_d47494a9381d26eb8cb4e5234fcdc3fd</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row0_col26" class="data row0 col26" >2655077536_5fc1543f9f3ecf51d5aa581ed672ff92</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row0_col27" class="data row0 col27" >705584566_6a471be9823bc60c4018c46f64cafc66</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row0_col28" class="data row0 col28" >3018254410_837623e288b391bdd6bbfdf0c05fe099</td>
            </tr>
            <tr>
                        <th id="T_211651ac_cc13_11ea_a7ba_4889e732b477level0_row1" class="row_heading level0 row1" >timestamp</th>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row1_col0" class="data row1 col0" >2018-07-30 23:57:43.674000</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row1_col1" class="data row1 col1" >2018-11-17 05:22:09.315000</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row1_col2" class="data row1 col2" >2019-07-26 04:14:14.486000</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row1_col3" class="data row1 col3" >2016-09-20 19:40:43.567000</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row1_col4" class="data row1 col4" >2019-06-26 03:25:13.067000</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row1_col5" class="data row1 col5" >2019-12-18 01:28:10.491000</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row1_col6" class="data row1 col6" >2018-05-28 15:59:03.631000</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row1_col7" class="data row1 col7" >2019-06-26 03:11:45.403000</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row1_col8" class="data row1 col8" >2019-08-04 19:13:55.677000</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row1_col9" class="data row1 col9" >2020-03-17 10:08:56.008000</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row1_col10" class="data row1 col10" >2018-08-22 20:41:17.162000</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row1_col11" class="data row1 col11" >2020-01-10 15:53:45</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row1_col12" class="data row1 col12" >2017-05-31 03:18:13.093000</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row1_col13" class="data row1 col13" >2020-01-09 20:01:45.139000</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row1_col14" class="data row1 col14" >2019-07-24 05:37:39.084000</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row1_col15" class="data row1 col15" >2019-09-13 17:26:42.811000</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row1_col16" class="data row1 col16" >2018-11-27 11:48:20.201000</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row1_col17" class="data row1 col17" >2015-02-17 14:14:00.564000</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row1_col18" class="data row1 col18" >2020-01-22 09:46:42.566000</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row1_col19" class="data row1 col19" >2017-04-23 12:49:49.814000</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row1_col20" class="data row1 col20" >2016-11-03 03:20:51.544000</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row1_col21" class="data row1 col21" >2020-02-15 03:48:33.562000</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row1_col22" class="data row1 col22" >2018-11-01 17:43:06.255000</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row1_col23" class="data row1 col23" >2018-07-10 14:28:40.935000</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row1_col24" class="data row1 col24" >2015-05-21 12:56:20.169000</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row1_col25" class="data row1 col25" >2017-05-16 19:31:45.202000</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row1_col26" class="data row1 col26" >2019-07-02 22:45:15.334000</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row1_col27" class="data row1 col27" >2015-06-13 15:03:44.228000</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row1_col28" class="data row1 col28" >2020-01-29 21:55:59.342000</td>
            </tr>
            <tr>
                        <th id="T_211651ac_cc13_11ea_a7ba_4889e732b477level0_row2" class="row_heading level0 row2" >story_id</th>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row2_col0" class="data row2 col0" >1882080860</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row2_col1" class="data row2 col1" >2152788158</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row2_col2" class="data row2 col2" >2697092730</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row2_col3" class="data row2 col3" >1031576328</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row2_col4" class="data row2 col4" >2643278798</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row2_col5" class="data row2 col5" >2945702628</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row2_col6" class="data row2 col6" >1772565236</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row2_col7" class="data row2 col7" >2643276430</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row2_col8" class="data row2 col8" >2712994768</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row2_col9" class="data row2 col9" >3100557178</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row2_col10" class="data row2 col10" >1921324056</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row2_col11" class="data row2 col11" >2982714842</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row2_col12" class="data row2 col12" >1265841482</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row2_col13" class="data row2 col13" >2981192220</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row2_col14" class="data row2 col14" >2693217348</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row2_col15" class="data row2 col15" >2782465396</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row2_col16" class="data row2 col16" >2189567610</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row2_col17" class="data row2 col17" >638702598</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row2_col18" class="data row2 col18" >3002580168</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row2_col19" class="data row2 col19" >1226599398</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row2_col20" class="data row2 col20" >1072631634</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row2_col21" class="data row2 col21" >3043085590</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row2_col22" class="data row2 col22" >2090370090</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row2_col23" class="data row2 col23" >1848070972</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row2_col24" class="data row2 col24" >692641368</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row2_col25" class="data row2 col25" >1251122972</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row2_col26" class="data row2 col26" >2655077536</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row2_col27" class="data row2 col27" >705584566</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row2_col28" class="data row2 col28" >3018254410</td>
            </tr>
            <tr>
                        <th id="T_211651ac_cc13_11ea_a7ba_4889e732b477level0_row3" class="row_heading level0 row3" >media_url</th>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row3_col0" class="data row3 col0" >timesofindia.indiatimes.com</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row3_col1" class="data row3 col1" >www.prnewswire.com</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row3_col2" class="data row3 col2" >www.newsdump.com</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row3_col3" class="data row3 col3" >www.business-standard.com</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row3_col4" class="data row3 col4" >prnewswire.co.uk</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row3_col5" class="data row3 col5" >www.wpri.com</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row3_col6" class="data row3 col6" >www.irishtimes.com</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row3_col7" class="data row3 col7" >www.devonenergy.com</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row3_col8" class="data row3 col8" >www.bignewsnetwork.com</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row3_col9" class="data row3 col9" >www.extremetech.com</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row3_col10" class="data row3 col10" >heatst.com</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row3_col11" class="data row3 col11" >www.bignewsnetwork.com</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row3_col12" class="data row3 col12" >prnewswire.co.uk</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row3_col13" class="data row3 col13" >prnewswire.co.uk</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row3_col14" class="data row3 col14" >www.marketwired.com</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row3_col15" class="data row3 col15" >www.pressdemocrat.com</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row3_col16" class="data row3 col16" >www.prnewswire.com</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row3_col17" class="data row3 col17" >thenextweb.com</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row3_col18" class="data row3 col18" >serverwatch.com</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row3_col19" class="data row3 col19" >indianexpress.com</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row3_col20" class="data row3 col20" >livemint.com</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row3_col21" class="data row3 col21" >www.marketwired.com</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row3_col22" class="data row3 col22" >www.prnewswire.com</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row3_col23" class="data row3 col23" >www.zawya.com</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row3_col24" class="data row3 col24" >betanews.com</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row3_col25" class="data row3 col25" >live.reuters.com</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row3_col26" class="data row3 col26" >www.latestnigeriannews.com</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row3_col27" class="data row3 col27" >telecompk.net</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row3_col28" class="data row3 col28" >www.wbal.com</td>
            </tr>
            <tr>
                        <th id="T_211651ac_cc13_11ea_a7ba_4889e732b477level0_row4" class="row_heading level0 row4" >parsed_company_names</th>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row4_col0" class="data row4 col0" >['MSD']</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row4_col1" class="data row4 col1" >['NVIDIA Corporation']</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row4_col2" class="data row4 col2" >['Philip Morris']</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row4_col3" class="data row4 col3" >['Microsoft']</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row4_col4" class="data row4 col4" >['Esters DuPont', 'ADM', 'Bio-based Esters DuPont']</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row4_col5" class="data row4 col5" >['Campbell']</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row4_col6" class="data row4 col6" >['iZettle', 'Busk']</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row4_col7" class="data row4 col7" >['Devon Energy establishes', 'Devon Energy']</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row4_col8" class="data row4 col8" >['NVIDIA Corporation']</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row4_col9" class="data row4 col9" >['AMD']</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row4_col10" class="data row4 col10" >['Walmart']</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row4_col11" class="data row4 col11" >['Cerner', 'University of Buffalo, Cerner']</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row4_col12" class="data row4 col12" >['Air Products and Chemicals']</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row4_col13" class="data row4 col13" >['Air Products']</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row4_col14" class="data row4 col14" >['Becton, Dickinson and Company']</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row4_col15" class="data row4 col15" >['Walmart']</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row4_col16" class="data row4 col16" >['Eli Lilly and Company', 'Boehringer Ingelheim']</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row4_col17" class="data row4 col17" >['iZettle']</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row4_col18" class="data row4 col18" >['Red Hat']</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row4_col19" class="data row4 col19" >['Mastercard']</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row4_col20" class="data row4 col20" >['Colgate-Palmolive (India) Ltd']</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row4_col21" class="data row4 col21" >['Analog Devices']</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row4_col22" class="data row4 col22" >['Zoetis']</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row4_col23" class="data row4 col23" >['Align Technology']</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row4_col24" class="data row4 col24" >['Microsoft']</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row4_col25" class="data row4 col25" >['Hewlett Packard Enterprise']</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row4_col26" class="data row4 col26" >['GIC', 'Equinix', "Singapore's GIC"]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row4_col27" class="data row4 col27" >['Zong']</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row4_col28" class="data row4 col28" >['McCormick']</td>
            </tr>
            <tr>
                        <th id="T_211651ac_cc13_11ea_a7ba_4889e732b477level0_row5" class="row_heading level0 row5" >launch_description</th>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row5_col0" class="data row5 col0" >blockbuster cancer drug Keytruda</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row5_col1" class="data row5 col1" >the DRIVE CX platform</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row5_col2" class="data row5 col2" >first Africa store</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row5_col3" class="data row5 col3" >Nokia 216 Dual SIM phone</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row5_col4" class="data row5 col4" >Breakthrough Process 6</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row5_col5" class="data row5 col5" >a line of veggie Goldfish crackers</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row5_col6" class="data row5 col6" >the feature</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row5_col7" class="data row5 col7" >Target</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row5_col8" class="data row5 col8" >the DRIVE CX platform that is a hardware module with software updates</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row5_col9" class="data row5 col9" >Ryzen Mobile 4000</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row5_col10" class="data row5 col10" >e-book catalog</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row5_col11" class="data row5 col11" >New EHR Training Program</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row5_col12" class="data row5 col12" >two new products for its anti-foaming segment</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row5_col13" class="data row5 col13" >Its New Freshline</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row5_col14" class="data row5 col14" >an automated flow cytometry sample preparation instrument with CE-IVD certification known as BD FACSDuet</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row5_col15" class="data row5 col15" >grocery delivery subscription</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row5_col16" class="data row5 col16" >Jardiance</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row5_col17" class="data row5 col17" >its first free entry-level Chip & PIN card reader</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row5_col18" class="data row5 col18" >CloudForms and OpenShift</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row5_col19" class="data row5 col19" >biometric cards</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row5_col20" class="data row5 col20" >Cibaca Vedshakti , a toothpaste made of natural ingredients</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row5_col21" class="data row5 col21" >SHARC</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row5_col22" class="data row5 col22" >the QScout MLD technology</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row5_col23" class="data row5 col23" >the iTero Element Intraoral Scanner in the Middle East</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row5_col24" class="data row5 col24" >new iPhone messaging app</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row5_col25" class="data row5 col25" >powerful computer prototype</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row5_col26" class="data row5 col26" >a $ 1 billion joint venture to build hyperscale data centers in Europe</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row5_col27" class="data row5 col27" >its 4 G Mobile Broadband Wifi in Karachi</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row5_col28" class="data row5 col28" >Old Bay Hot Sauce</td>
            </tr>
            <tr>
                        <th id="T_211651ac_cc13_11ea_a7ba_4889e732b477level0_row6" class="row_heading level0 row6" >sentiment_positivity</th>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row6_col0" class="data row6 col0" >0.0174675</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row6_col1" class="data row6 col1" >0.0510146</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row6_col2" class="data row6 col2" >0.25</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row6_col3" class="data row6 col3" >0.297619</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row6_col4" class="data row6 col4" >0.0493528</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row6_col5" class="data row6 col5" >0.155195</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row6_col6" class="data row6 col6" >0.355062</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row6_col7" class="data row6 col7" >0.173415</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row6_col8" class="data row6 col8" >0.0499339</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row6_col9" class="data row6 col9" >0.182988</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row6_col10" class="data row6 col10" >0.0994444</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row6_col11" class="data row6 col11" >0.091461</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row6_col12" class="data row6 col12" >0.095303</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row6_col13" class="data row6 col13" >0.164141</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row6_col14" class="data row6 col14" >0.00385054</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row6_col15" class="data row6 col15" >0.175777</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row6_col16" class="data row6 col16" >0.0390952</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row6_col17" class="data row6 col17" >0.181239</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row6_col18" class="data row6 col18" >0.0516311</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row6_col19" class="data row6 col19" >0.104004</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row6_col20" class="data row6 col20" >0.0670391</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row6_col21" class="data row6 col21" >0.104391</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row6_col22" class="data row6 col22" >0.204456</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row6_col23" class="data row6 col23" >-0.34</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row6_col24" class="data row6 col24" >0.174364</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row6_col25" class="data row6 col25" >0.285114</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row6_col26" class="data row6 col26" >-0.1</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row6_col27" class="data row6 col27" >0.258333</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row6_col28" class="data row6 col28" >0.175</td>
            </tr>
            <tr>
                        <th id="T_211651ac_cc13_11ea_a7ba_4889e732b477level0_row7" class="row_heading level0 row7" >sentiment_objectivity</th>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row7_col0" class="data row7 col0" >-0.442987</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row7_col1" class="data row7 col1" >-0.425555</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row7_col2" class="data row7 col2" >-0.333333</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row7_col3" class="data row7 col3" >-0.367857</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row7_col4" class="data row7 col4" >-0.404371</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row7_col5" class="data row7 col5" >-0.486364</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row7_col6" class="data row7 col6" >-0.60854</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row7_col7" class="data row7 col7" >-0.398261</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row7_col8" class="data row7 col8" >-0.452711</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row7_col9" class="data row7 col9" >-0.462789</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row7_col10" class="data row7 col10" >-0.387222</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row7_col11" class="data row7 col11" >-0.412995</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row7_col12" class="data row7 col12" >-0.522121</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row7_col13" class="data row7 col13" >-0.521444</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row7_col14" class="data row7 col14" >-0.386522</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row7_col15" class="data row7 col15" >-0.448144</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row7_col16" class="data row7 col16" >-0.42108</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row7_col17" class="data row7 col17" >-0.399554</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row7_col18" class="data row7 col18" >-0.326235</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row7_col19" class="data row7 col19" >-0.340059</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row7_col20" class="data row7 col20" >-0.407352</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row7_col21" class="data row7 col21" >-0.375827</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row7_col22" class="data row7 col22" >-0.407598</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row7_col23" class="data row7 col23" >-0.44</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row7_col24" class="data row7 col24" >-0.495212</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row7_col25" class="data row7 col25" >-0.516435</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row7_col26" class="data row7 col26" >-0.1</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row7_col27" class="data row7 col27" >-0.424479</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row7_col28" class="data row7 col28" >-0.525</td>
            </tr>
            <tr>
                        <th id="T_211651ac_cc13_11ea_a7ba_4889e732b477level0_row8" class="row_heading level0 row8" >tense_category</th>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row8_col0" class="data row8 col0" >other</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row8_col1" class="data row8 col1" >past</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row8_col2" class="data row8 col2" >other</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row8_col3" class="data row8 col3" >other</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row8_col4" class="data row8 col4" >other</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row8_col5" class="data row8 col5" >other</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row8_col6" class="data row8 col6" >recent past</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row8_col7" class="data row8 col7" >other</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row8_col8" class="data row8 col8" >past</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row8_col9" class="data row8 col9" >other</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row8_col10" class="data row8 col10" >other</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row8_col11" class="data row8 col11" >other</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row8_col12" class="data row8 col12" >past</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row8_col13" class="data row8 col13" >other</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row8_col14" class="data row8 col14" >past</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row8_col15" class="data row8 col15" >other</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row8_col16" class="data row8 col16" >other</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row8_col17" class="data row8 col17" >other</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row8_col18" class="data row8 col18" >other</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row8_col19" class="data row8 col19" >other</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row8_col20" class="data row8 col20" >past</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row8_col21" class="data row8 col21" >past</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row8_col22" class="data row8 col22" >past</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row8_col23" class="data row8 col23" >other</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row8_col24" class="data row8 col24" >future</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row8_col25" class="data row8 col25" >other</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row8_col26" class="data row8 col26" >future</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row8_col27" class="data row8 col27" >recent past</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row8_col28" class="data row8 col28" >other</td>
            </tr>
            <tr>
                        <th id="T_211651ac_cc13_11ea_a7ba_4889e732b477level0_row9" class="row_heading level0 row9" >lei_identifiers_matched</th>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row9_col0" class="data row9 col0" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row9_col1" class="data row9 col1" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row9_col2" class="data row9 col2" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row9_col3" class="data row9 col3" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row9_col4" class="data row9 col4" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row9_col5" class="data row9 col5" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row9_col6" class="data row9 col6" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row9_col7" class="data row9 col7" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row9_col8" class="data row9 col8" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row9_col9" class="data row9 col9" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row9_col10" class="data row9 col10" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row9_col11" class="data row9 col11" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row9_col12" class="data row9 col12" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row9_col13" class="data row9 col13" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row9_col14" class="data row9 col14" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row9_col15" class="data row9 col15" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row9_col16" class="data row9 col16" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row9_col17" class="data row9 col17" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row9_col18" class="data row9 col18" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row9_col19" class="data row9 col19" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row9_col20" class="data row9 col20" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row9_col21" class="data row9 col21" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row9_col22" class="data row9 col22" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row9_col23" class="data row9 col23" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row9_col24" class="data row9 col24" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row9_col25" class="data row9 col25" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row9_col26" class="data row9 col26" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row9_col27" class="data row9 col27" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row9_col28" class="data row9 col28" >[...]</td>
            </tr>
            <tr>
                        <th id="T_211651ac_cc13_11ea_a7ba_4889e732b477level0_row10" class="row_heading level0 row10" >lei_identifiers_uplinked</th>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row10_col0" class="data row10 col0" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row10_col1" class="data row10 col1" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row10_col2" class="data row10 col2" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row10_col3" class="data row10 col3" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row10_col4" class="data row10 col4" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row10_col5" class="data row10 col5" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row10_col6" class="data row10 col6" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row10_col7" class="data row10 col7" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row10_col8" class="data row10 col8" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row10_col9" class="data row10 col9" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row10_col10" class="data row10 col10" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row10_col11" class="data row10 col11" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row10_col12" class="data row10 col12" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row10_col13" class="data row10 col13" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row10_col14" class="data row10 col14" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row10_col15" class="data row10 col15" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row10_col16" class="data row10 col16" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row10_col17" class="data row10 col17" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row10_col18" class="data row10 col18" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row10_col19" class="data row10 col19" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row10_col20" class="data row10 col20" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row10_col21" class="data row10 col21" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row10_col22" class="data row10 col22" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row10_col23" class="data row10 col23" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row10_col24" class="data row10 col24" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row10_col25" class="data row10 col25" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row10_col26" class="data row10 col26" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row10_col27" class="data row10 col27" >[...]</td>
                        <td id="T_211651ac_cc13_11ea_a7ba_4889e732b477row10_col28" class="data row10 col28" >[...]</td>
            </tr>
    </tbody></table>


<br>

## Coverage

As of July 2020, the dataset covers **2.4 million product launches** parsed from English-language news. Most of the records are based on news published after 2014.

<br>

## Data access

The full dataset is hosted on the Google Cloud Platform. Please contact dennis.schuler@protonmail.com in case you are interested in using the data for research or a commercial project.



```python

```
