# EU PAS Register Data Matching
This repository contains raw data, notebooks, and script outputs.

## Legend
This is a legend for all the files in this repository:
+ `./data`
    + This folder contains raw and processed data. See `README.md` inside these folders for further help.
+ `./notebook`
    + This folder contains Jupyter notebooks for some important tasks. Read the explanations inside the notebooks for further help.
+ `./output`
    + This folder contains output data created by the custom `statistic` commands and scrapy spiders

## Data flowchart
The following flowchart illustrates the data flow for analysis. We used scraped and exported datasets, classifying the outcomes and cancelled studies manually. See [here](eupas_commands.md) for additional explanations.

```mermaid
flowchart TD
    %% Main outcome generation block
    J(["Outcome Generation"])
    J --> G["Manual Outcome Classification (True/False)"]
    J --> I["Outcome Estimation from Document Presence"]

    G --> F["download_documents_for_classification.ipynb outcomes_manual.ipynb"]
    I --> H["scrapy ema_rwd_statistics command (auto estimate outcomes)"]

    F --> K["scrapy ema_rwd_statistics command (final analysis)"]
    H --> K

    %% User input and data flow
    A[User Input] --> B[Catalogue CSV Export]
    A --> C[scrapy eupas command]

    B --> D[convert_exported_and_compare_datasets.ipynb]
    D --> E[Excel File]
    C --> E
    E --> J

    %% Cancelled study filtering
    E --> L(["Cancelled Study Filtering"])
    L --> M[Manual Read All Descriptions]

    L --> X1["Automatically classify with ChatGPT"]
    X1 --> N[classify_cancelled_gpt.ipynb]

    L --> X2["Automatically classify with Regular expressions"]
    X2 --> O[scrapy patch]

    M --> K
    N --> K
    O --> K

    %% Sponsor matching branch
    E --> S(["Sponsor matching"])
    S --> T["scrapy cluster funding_details (starting point for manual matching)"]
    T --> K

    %% Final outputs
    K -- "`funding.json studies.json`" --> V["Extra Website"]
    V --> U["npm run build"]

    K --> W["Supplement Outputs"]
    W --> P[extra_analysis notebooks]
    W --> Q[extra_tables notebooks]
    W --> R[extra_plots notebooks]

    %% Styling
    style A fill:#d9f,stroke:#333,stroke-width:2px,color:#333
    style J fill:#cfc,stroke:#333,stroke-width:2px,color:#333
    style L fill:#cfc,stroke:#333,stroke-width:2px,color:#333
    style S fill:#cfc,stroke:#333,stroke-width:2px,color:#333
    style K fill:#bbf,stroke:#333,stroke-width:2px,color:#333
```