# Financial Patent Data Set

## General Notes

Below, any data or aggregation done at the “year” level uses the patent application year, not the grant year.  The grant date is included as a variable in the patent-level dataset just in case it is needed, but all years  associated with other variables (VC-funding at the CSA-year level, revenue at the firm-year level, etc.) are based on the application year.  The main use of the grant date is in benchmarking forward cites at the patent-level (they are relative to average cites within the same CPC subclass and grant quarter).

For this analysis, we only consider the first inventor and first assignee when inventor/assignee information is needed.  For example, we attribute each patent’s geographic location to a Census CSA based on the county FIPS of the first inventor.  We do not attribute some fraction of the patent to the geographic location of other inventors.  Similarly, for the firm level analysis, we attribute ownership of the patent to the first assignee.  We do not attribute a fractional ownership share to other assignees.

Note we added passive funds and funds as categories later to correspond to the BEA groups. These were very modest in number in both cases. We can get you the patents falling under these categories if of interest.

This document only briefly summarizes what variables are in what files.  For more information on how the data sets were constructed, please refer to the paper or the authors.

  

## Patent Data

#### Data Versions:

The version released on April 5, 2022 is the latest data.


#### Data Description:

This sample consists of all financial patents applied for on or after January 1, 2000.  

Most of this data is taken from the October 8, 2019 Patentsview data release. The estimates of patent value are taken from the extended data (till 2020) following Kogan, L., Papanikolaou, D., Seru, A. and Stoffman, N., 2017, and Kelly, B., Papanikolaou, D., Seru, A. and Taddy, M., 2020. The CSAs are taken from an NBER crosswalk associating county FIPS with CSAs, where we use the county FIPS of the first inventor’s address as the patent’s geographic location.

We provide one data set constructed from the paper here:

- **Fin Patent Data for Posting.20220403.dta**

The variable definitions are:

- patent_id - the numeric patent number stripped of country and kind codes; can be used to join / merge with other data like financial patent data
- app_date - the application date
- grant_date - the grant date
- primary_cpc - the primary CPC code
- cpc_subclass - the 4-digit (e.g. G06Q) primary CPC subclass (used for calculating mean_cites within cpc_subclass and quarter of grant_date)
- cite_count - the number of forward citations received
- mean_cites - the mean number of cites received by all patents in this data set that have the same CPC subclass and were granted in the same quarter
- weighted_cite - cite_count / mean_cites
- assignee_type - the USPTO type of the first assignee (1 - Unassigned, 2 - US Company or Corporation, 3 - Foreign Company or Corporation, 4 - US Individual, 5 - Foreign Individual, 6 - US  Federal Government, 7 - Foreign Government, 8 - US County Government, 9 - US State Government. Note: A "1" appearing before any of these codes signifies part interest)
- kogan_etal_val - the estimated value of the patent in millions of inflation adjusted USD based on stock market response
- kelly_etal_val_10 - the estimated value of the patent based on textual analysis with a 10-year forward horizon
- kelly_etal_val_5 - the estimated value of the patent based on Kelly et al. (textual analysis with a 5-year forward horizon)
- csa_code - the numeric CSA code associated with the first inventor’s address
- csa_title - the description of the CSA region associated with csa_code
- inventor_1_country - the abbreviated country (US, etc.) of the first named inventor
- inventor_1_state - the abbreviated state (CA, etc.) of the first named inventor if in the US
- state_fips - the fips code for the state of the first named inventor if in the US
- vc_num_deals - the aggregate number of VC deals executed in the application year of the patent in the CSA associated with the patents’ first inventor
- vc_sum_equity_invested - the aggregate sum of VC money invested (in millions of nominal USD) in the application year of the patent and in the CSA associated with the patents’ first inventor
- us_ai_ranking - if the csa_code is one of 6 cities identified as AI hubs, this variable shows the CSA’s AI-hub ranking within the US (i.e. the ranking goes from 1-6, and it is not a global ranking); otherwise it is missing.
- assignee_1_country - the abbreviated country (US, etc.) of the first named assignee. In some cases, U.S. states were coded as separate country; the “division” analysis below has corrected these.
- accounting - the fractional share in accounting
- investment_banking - the fractional share in investment banking
- commercial_banking - the fractional share in commercial banking
- communications - the fractional share in communications
- payments - the fractional share in payments
- cryptocurrency - the fractional share in cryptocurrency
- currency - the fractional share in currency
- insurance - the fractional share in insurance
- real_estate - the fractional share in real estate
- retail_banking - the fractional share in retail banking
- security - the fractional share in security
- wealth_management - the fractional share in wealth management
- pae_initial_assignment - a binary variable indicating that the first assignee matched to a valid PAE name
- assignee_1_pae_name - the name of the first assignee if pae_initial_assignment == 1
- pae_reassignment - a binary variable indicating whether the patent was reassigned to a valid PAE at any point in time after Jan. 1, 2000
- reassignment_pae_name - the name of the first valid PAE (in time) to which the patent was reassigned, if pae_reassignment == 1
- pae_reassignment_date - the execution date of the first reassignment if pae_reassignment == 1

The dataset also includes a host of financial information about the first assignee taken from Capital IQ and based on the patent application year unless otherwise noted (some Capital IQ variables report prior year data, in which case we have the year before the application year).  These variables are (variables explained in further detail where not obvious):

- capiq_id - the Capital IQ identifier for the first assignee
- company_name - the name of the first assignee from Capital IQ
- revenue 
- ebitda 
- rd_expense - research and development expenses
- advertising expense
- net_income
- cash - cash in the year prior to the application year
- long_term_debt - long term debt in the year prior to the application year
- short_term_debt - short term debt in the year prior to the application year
- short_term_investments - short term investments in the year prior to the application year
- shareholder_equity - shareholder equity in the year prior to the application year
- market_cap - market capitalization as of the end (12/31) of the application year
- employment 
- year_founded
- age_of_firm
- primary_industry - industry name for the first assignee’s primary industry
- primary_industry_code - full 8-digit GICS code for the first assignee’s primary industry
- gics_sector_code - 2-digit GICS sector code for the first assignee’s primary industry
- gics_group_code - 4-digit GICS sector code for the first assignee’s primary industry
- gics_industry_code - 6-digit GICS industry code for the first assignee’s primary industry
- gics_sector - character variable with sector name corresponding gics_sector_code
- gics_industry_financials - character variable with industry name corresponding to gics_industry code only if gics_industry_code == 40 (for financials)
- revenue_group - a character vector indicating whether the firm’s revenue in the application year was less than $100 million (“Small”), more than $100 million but less than $10 billion (“Medium”), or greater than or equal to $10 billion (“Large”).  If revenue is missing, this will also be missing.
- public - a binary variable indicating whether the firm had positive, non-missing market capitalization at the end (12/31) of the application year (1); this variable will be equal to 0 in years for which the firm did patent but did not have a market capitalization at the end (12/31) of the application year; it will be missing in all other years (including years in which the firm did not patent)
- global_sifi - a binary variable indicating whether first assignee is a global SIFI
- vc_backed - a binary variable indicating whether the firm received VC-funding at any point after January 1, 2000
- division—the region of the country on which the first inventor was based.


## Contact:

Please contact Josh Lerner (jlerner@hbs.edu) or Amit Seru (aseru@stanford.edu) for any questions regarding the data.

**Please see the paper for more information on the data. If you use these data sets, please CITE this paper as the data source.**
