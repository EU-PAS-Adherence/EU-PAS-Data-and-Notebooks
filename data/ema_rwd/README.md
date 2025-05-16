# README
This is a legend for all the files in this directory:

## Reviewers
There are two reviewers (`<reviewer>`):
+ `CP`
+ `PR`

## Versions
Versions are specified like this (`<version>`):
+ `v1`, `v2`, `v3` etc.

## Legend
+ `ema_rwd_raw_export.csv`
    + This is the raw data with the original column names exported from the EMA-HMA RWD Studies register with the `.csv` export feature
    + Export date: `21-02-2024`
+ `ema_rwd.xlsx`
    + This is the exported data, converted to `xlsx` with transformed and cleaned up columns; some columns were dropped in this stage
        + See `./notebooks/database_migration/convert_exported_and_compare_datasets.ipynb`
    + Copy of `./notebooks/database_migration/converted_ema-rwd_2024-02-21.xlsx`
+ `ema_rwd_<reviewer>.xlsx`
    + $2$ Versions
    + These are `ema_rwd.xlsx` with an extra column for manually determining cancelled studies based on the `description` column
    + The evaluation was carried out by `<reviewer>` respectively
+ `ema_rwd_patched_regex_<version>.xlsx`
    + $2$ Versions
    + These are build on top of `ema_rwd.xlsx` piped through the `scrapy patch` command with the arguments `state cancel`
    + This adds some columns (updated state column and cancellation status based on regex):
        ```python
        '$UPDATED_state', # Updates state variable (no NA Values anymore) 
        '$UPDATED_state_eq_state', # Is true when state == $UPDATED_state
        '$CANCELLED', # Automatic RegExp based cancellation detection
        '$CANCELLED_extracted_word', # The matched words
        '$CANCELLED_extracted_sentence', # The matched sentence
        ```
    + `v2` was created with a bigger Regex list
+ `ema_rwd_patched_gpt.xlsx`
    + This is `ema_rwd_patched_regex_v2.xlsx` with an new `$CANCELLED_GPT` column created with the help of ChatGPT
    + There is a single line break error, which switches `\r` to `_x000D_` in Excel for one value of `additional_institutions_not_encepp` between this and the Regex versions
+ `ema_rwd_patched_manual_gpt_<version>.xlsx`
    + $3$ Versions
    + These are build on top of `ema_rwd_patched_gpt.xlsx` with manual classified cancellation status based on `ema_rwd_<reviewer>.xlsx`
    + New Column:
        ```python
        '$CANCELLED_MANUAL' # Manual cancellation detection based on CP and PR review
        ```
    + Renamed Columns:
        ```python
        # FROM
        '$CANCELLED',
        '$CANCELLED_extracted_sentence',
        '$CANCELLED_extracted_word'

        # To 
        '$CANCELLED_REGEX',
        '$CANCELLED_REGEX_extracted_sentence',
        '$CANCELLED_REGEX_extracted_word'
        ```
    + Included non intersecting cancelled studies again in `v2` based on `ema_rwd_<reviewer>.xlsx` reducing the amount from $64$ to $60$
    + Included two additional studies in `v3` after a second review of the descriptions reducing the amount from $60$ to $58$
    + Excluded two additional studies in `v4` after a review of the result documents increasing the amount from $58$ to $60$
+ `ema_rwd_p_m_gpt.xlsx`
    + This is `ema_rwd_patched_manual_gpt_v4.xlsx`, which was piped through the `scrapy patch` command with the arguments `match`
    + `p` for patched (updated state, regex cancellation and matched fundings)
    + `m`for manual (manual cancellation)
    + `gpt` for ChatGPT (GPT cancellation) 
    + These are all the added columns at this stage:
        ```python
        '$UPDATED_state', # Updates state variable (no NA Values anymore) 
        '$UPDATED_state_eq_state', # Is true when state == $UPDATED_state
        '$CANCELLED_REGEX', # Automatic RegExp based cancellation detection
        '$CANCELLED_REGEX_extracted_word', # The matched words
        '$CANCELLED_REGEX_extracted_sentence', # The matched sentence
        '$CANCELLED_MANUAL', # Manual cancellation detection based on CP and PR review
        '$CANCELLED_GPT', # Automatic ChatGPT based cancellation detection
        '$MATCHED_funding_details', # matched and harmonised funding details
        '$MATCHED' # This is the same as $MATCHED_funding_details because of legacy reasons 
        # (In the EU PAS Register matching was based on two columns which were combined here)
        ```
+ `ema_rwd_p_m_gpt_o_<version>.xlsx`
    + This is `ema_rwd_p_m_gpt.xlsx` with two new columns `has_protocol` and `has_result` based on study documents
        + See `./notebooks/study_documents/merge_classifications/outcomes_manual.ipynb`
    + `o` for outcome (manual `has_result` classification based on result study documents)
    + Changed some sponsor names in `v2`
+ `ema_rwd_p_m_gpt_o_s_<version>.xlsx`
    + This is `ema_rwd_p_m_gpt_o_v2.xlsx` with two new columns `funding_sources_grouped_override` and `multiple_funding_sources_override` which will override `funding_sources_grouped` and `multiple_funding_sources` respectivly for entries, which need manual overrides
        + See `./notebooks/extra_analysis/variables_analysis.ipynb` for `funding_sources_grouped_override`
    + `s` for sources
    + Changed some sponsor names in `v2`
    + This is the same as `ema_rwd_final.xlsx` used in the final analysis
+ `funding_manual.xlsx`
    + This is the manual match file used in `scrapy patch match` to assign the matched sponsor based on the `funding_details` field
+ `rmp1&2_documents_manual_<reviewer>.xlsx`
    + The template of this document was created in a notebook in this repository
    + This table assigns a document_type to each document for each included study with rmp category 1 or 2 at the time of extraction (see `ema_rwd.xlsx`)
    + The evaluation was carried out by `<reviewer>` respectivly
+ `rmpother_documents_manual_<reviewer>.xlsx`
    + The template of this document was created in a notebook in this repository
    + This table assigns a document_type to each document for each included study without rmp category 1 or 2 at the time of extraction (see `ema_rwd.xlsx`)
    + The evaluation was carried out by `<reviewer>` respectivly