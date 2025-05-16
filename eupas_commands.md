# EUPAS Commands (Deprecated)
Run these commands with both repositories cloned into the same directory

```bash
cd EnceppScrapy
scrapy eupas -PDF
scrapy cluster centre_name centre_name_of_investigator -i ../../EU_PAS_Register_Data_Matching/eupas.xlsx -o ../../EU_PAS_Register_Data_Matching/output/clusters -c 0.6
scrapy substances -i ../../EU_PAS_Register_Data_Matching/eupas.xlsx -o ../../EU_PAS_Register_Data_Matching/output/clusters -c 0.6
scrapy patch match state -i ../../EU_PAS_Register_Data_Matching/eupas.xlsx -o ../../EU_PAS_Register_Data_Matching/output/eupas/ -mi ../../EU_PAS_Register_Data_Matching/centre_manual.xlsx -mc -ac --match-eupas
scrapy eupas_statistic -i ../../EU_PAS_Register_Data_Matching/output/eupas/eupas_patched.xlsx -o ../../EU_PAS_Register_Data_Matching/output/eupas/ -D 2024-01-23T22:16
```