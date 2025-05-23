# UKB Phenotype Exporter using DNAnexus Table-Exporter
This repository demonstrates how to retrieve phenotype data from the UK Biobank RAP using the `dx run table-exporter` app on the DNAnexus platform.

## Overview

There are multiple ways to export phenotypes from the UK Biobank Research Analysis Platform (RAP). This method uses the `field_names_file_txt` flag to export fields listed in a plain text file. It's especially helpful for bulk exports.

For additional flags and options, refer to:
* [Accessing phenotype data](https://dnanexus.gitbook.io/uk-biobank-rap/working-on-the-research-analysis-platform/accessing-data/accessing-phenotypic-data)
* [UKB Table-Exporter Documentation](https://documentation.dnanexus.com/developer/apps/developing-spark-apps/table-exporter-application) 

## Prerequisites

- [DNAnexus CLI (`dx`)](https://documentation.dnanexus.com/downloads)
- A valid UKB RAP project
- Access to the UK Biobank dataset

## Step 1: Create a list of field names

Use this repository to generate the list of fild names from FIDS: [grep_ifield_names](https://github.com/TheNaziaPathan/grep_ifield_names)

Save the output as `ifield_names_list.txt`.

Login via the terminal with `dx login username`.

## Step 2: Upload the field names list to your RAP project

```bash
dx upload ifield_names_list.txt --destination "/User_folders/"
```
## Step 3: Run the table-exporter
Update flags with your own information, then run the following:

```bash
dx run table-exporter \
  -idataset_or_cohort_or_dashboard="app####_2025##########.dataset" \
  -ioutput="output_base_filename" \
  -ioutput_format=TSV \
  -icoding_option=RAW \
  -iheader_style=FIELD-NAME \
  -ientity="participant" \
  -ifield_names_file_txt="ifield_names_list.txt" \
  --destination "/User_folders/"
```
## Note:

* Test with a smaller subset prior to bulk extraction
* For eid + 4 field names, the above extraction took 7 mins and £0.0075 during the creation of this repo.
