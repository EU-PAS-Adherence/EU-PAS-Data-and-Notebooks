# README
This is a legend for all the files in this directory:

## Legend
+ `eupas.xlsx`
    + This is the raw data with scraped from the EU PAS register (now replaced by the EMA-HMA RWD register) with the `scrapy eupas` command
    + Export date: `23-01-2024`
+ `centre_manual.xlsx`
    + This is the manual match file used in `scrapy patch match` to assign the matched sponsor based on the `centre_name` and `centre_name_of_investigator` fields