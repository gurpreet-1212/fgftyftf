```mermaid

graph TD;

%% --- FREEBIES BRANCH --- %%
subgraph Freebies_Branch [FREEBIES BRANCH]
    A1[Load Freebies CSVs: sr1, sr2, sr3, sr4] -->|Combining multiple CSVs into a unified dataset| A2
    A2[Concatenate into sr_jul22_dec23] -->|Standardizing product identifiers by adding material codes| A3
    A3[Merge with Product Master on SKU/EAN] -->|Create a separate version for sales-related transformations| A4
    A4[Copy & Clean Data  → sr_jul22_dec23_sales] -->|Enhancing the dataset with required reporting fields| A5
    A5[Add Extra Columns: period, order, channel, etc.] -->|Removing total COGS and preparing for unit-level cost calculation| A6
    A6[Create Freebies COGS Data - Drop 'cogs'] -->|Adding per-unit COGS from external computation| A7
    A7[Merge with df_cogs_pu - Compute cogs_pu] -->|Setting transaction type for COGS entries| A8
    A8[Set nsv=0, type='cogs'  → sr_jul22_dec23_cogs] -->|Ensuring column consistency across datasets| A9
    A9[Align Schema - Add Extra Columns] -->|Combining freebies sales and COGS into a single dataset| A10
    A10[Merge sr_jul22_dec23_sales & sr_jul22_dec23_cogs  → sr_jul22_feb24]
end

%% --- SALES BRANCH --- %%
subgraph Sales_Branch [SALES BRANCH]
    B1[Load Sales CSVs: sr7, sr8] -->|Combining multiple sales files into one dataset| B2
    B2[Concatenate into sales_df] -->|Standardizing product identifiers by adding material codes| B3
    B3[Merge with Product Master on Product Code] -->|Removing inconsistencies and preparing for reporting| B4
    B4[Clean & Structure Sales Data] -->|Ensuring compatibility with freebies dataset| B5
    B5[Align & Rename Columns]
end

%% --- MERGING BOTH BRANCHES --- %%
A10 -->|Merge Freebies & Sales| M1[Merge sr_jul22_feb24 with sales_df → sr_total]
B5 --> M1

%% --- EXPORTING DATA --- %%
M1 --> M2[Final Cleaning & Standardization] --> M3
M3[Export sr_total for Power BI Dashboard]







