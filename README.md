graph TD;

%% --- FREEBIES BRANCH --- %%
subgraph Freebies_Branch [FREEBIES BRANCH]
    A1[Load Freebies CSVs: sr1, sr2] --> A2
    A2[Concatenate into sr_jul22_dec23] --> A3
    A3[Merge with Product Master on SKU/EAN] --> A4
    A4[Copy & Clean Data  → sr_jul22_dec23_sales] --> A5
    A5[Add Extra Columns: period, order, etc.] --> A6
    A6[Create Freebies COGS Data  Drop 'cogs'] --> A7
    A7[Merge with df_cogs_pu  Compute cogs_pu] --> A8
    A8[Set nsv=0, type='cogs'  → sr_jul22_dec23_cogs] --> A9
    A9[Align Schema  Add Extra Columns] --> A10
    A10[Merge Freebies Paths  → sr_jul22_feb24]
end

%% --- SALES BRANCH --- %%
subgraph Sales_Branch [SALES BRANCH]
    B1[Load Sales CSVs: sr5, sr6] --> B2
    B2[Concatenate into sales_df] --> B3
    B3[Merge with Product Master on Product Code] --> B4
    B4[Clean & Structure Sales Data] --> B5
    B5[Align & Rename Columns]
end

%% --- MERGING BOTH BRANCHES --- %%
A10 -->|Merge Freebies & Sales| M1[Merge sr_jul22_feb24 with sales_df  → sr_total]
B5 --> M1

%% --- EXPORTING DATA --- %%
M1 --> M2[Final Cleaning & Standardization] --> M3
M3[Export sr_total for Power BI Dashboard]
