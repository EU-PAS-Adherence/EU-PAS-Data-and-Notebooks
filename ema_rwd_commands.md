# EMA RWD Commands
These are the commands we used. Run these commands with this repository and the repository containing the Scrapy project cloned into the same directory.

First move to the Scrapy repository to get access to the custom commands:
```bash
cd path/to/scrapyproject
```

Next we need to get the data:
```bash
scrapy ema_rwd 
# also download .csv using the export functionality 
# and convert exported .csv (See README.md flowchart)
```

Copy the data to `./data/ema_rwd/ema_rwd.xlsx`

Next you will have to create the manual funding matching table (for the website and sponsor level statistics). This command will help you a little bit:
```bash
scrapy cluster funding_details -i ../../EU_PAS_Register_Data_Matching/output/ema_rwd/ema_rwd.xlsx -o ../../EU_PAS_Register_Data_Matching/output/clusters -c 0.6
```

Copy your matching file to `./data/ema_rwd/funding_manual.xlsx`

Next we will create the updated state variable, match the funding_details with the table (and try to find cancelled studies with regex; we used manual classification instead):
```bash
scrapy patch match state cancel -i ../../EU_PAS_Register_Data_Matching/ema_rwd.xlsx -o ../../EU_PAS_Register_Data_Matching/output/ema_rwd/ -mi ../../EU_PAS_Register_Data_Matching/funding_manual.xlsx -mc -ac -sc
```

Finally place your table (e.g. after manual or gpt cancellation detection, outcome classification, etc.) in `./output/ema_rwd/ema_rwd_final.xlsx` and run the statistic script (replace `-D` with your download time; we used `2024-02-21T23:20`):
```bash
scrapy ema_rwd_statistic -i ../../EU_PAS_Register_Data_Matching/output/ema_rwd/ema_rwd_final.xlsx -o ../../EU_PAS_Register_Data_Matching/output/ema_rwd/ -D 2024-02-21T23:20
```