# funding-per-protein-hackathon


*   Function COSI - Funding per Annotated Protein - ISMB 2025

## Problem Statement
Quantifying the financial investment in specific areas of biomedical research is challenging. While NIH funding is extensive, directly attributing grant dollars to individual proteins studied is non-trivial. Approximating the dollar amount of funding allocated to the proteins help us to:
*   Assess funding distribution across different biological targets.
*   Identify potentially under-funded or over-concentrated areas of protein research.
*   Track the financial trajectory of research on specific proteins over time.
*   Provide researchers and funding agencies with a clearer picture of financial resource allocation at the protein level.

## Hackathon Overview
We introduce a Hackathon project titled "Funding per Annotated Protein" with the objectives of creating a data pipeline and visualization tool that:
1.  **Connects NIH grants to resulting publications** using the NIH RePORTER database (linking grant IDs to PMIDs).
2.  **Links PMIDs to experimentally annotated proteins** using GAF (Gene Ontology Annotation Format) files, which map PMIDs to gene products and proteins that have been annotated experimentally.
3.  **Implements various funding allocation strategies** to distribute grant funding:
    *   From a grant to its associated publications (e.g., equal split is the simplest strategy).
    *   From a publication to the proteins annotated within it (e.g., equal split is the simplest strategy).
4.  **Calculates the total estimated funding** received by each protein.
5.  **Presents these findings**, including a time-series visualization of funding per protein over time on a simple web interface.

## Data Sources
1.  **NIH RePORTER:**
    *   Source: National Institutes of Health (NIH)
    *   Content: Grant information (award amounts, project periods, PIs), and associated publications (PMIDs).
    *   Access: Download [list of projects](https://reporter.nih.gov/exporter/projects) and [link tables](https://reporter.nih.gov/exporter/linktables) from NIH RePORTER and/or use [API](https://api.reporter.nih.gov/) to retrieve grant projects and associated publications.
2.  **Gene Ontology Annotation (GAF) files:**
    *   Source: Gene Ontology Consortium members (e.g., UniProt-GOA, MGI, SGD).
    *   Content: Protein annotations linking proteins (e.g., UniProt IDs) to GO terms, supported by evidence codes and PMIDs. We will focus on experimental annotations as these are always accompanied by their source PubMed articles.
    *   Access: Publicly available FTP sites hosted by EBI (`ftp.ebi.ac.uk/pub/databases/GO/goa/`) or Gene Ontology Data Archive (`https://release.geneontology.org/`)

## Methodology
1.  **Data Acquisition & Preprocessing:**
    *   Fetch grant data from NIH RePORTER "Projects" tab. Extract relevant fields: Grant ID, Fiscal Year, Award Amount, Project Start/End Dates, associated PMIDs.
    *   Download GAF files for relevant organisms (e.g., human, mouse). Parse GAF files to extract (Protein ID, PMID, Evidence Code) and filter for experimental annotations.
    *   Clean and standardize data (e.g., filter publications based on starting year, use UniProtID as primary identifier).

2.  **Linking Grants to Proteins:**
    *   **Grant-to-Publication:** Use PMIDs from NIH RePORTER "Link Tables" tab to link grants to publications.
    *   **Publication-to-Protein:** Use PMIDs from GAF files to link publications to specific proteins.
    *   Combine these to create a Grant -> Publication -> Protein linkage.

3.  **Funding Apportionment Strategies:**
    *   **Grant-to-Publication (G2P) - Equal Split**
        *   `Amount_per_Publication = Total_Grant_Award_Amount / Number_of_Publications_from_Grant`
        *  Note: Grants might span multiple years, yearly budget amount might not be available.
    *   **Publication-to-Protein (P2Prot) - Equal Split**
        *   `Amount_per_Protein_from_Publication = Amount_per_Publication / Number_of_Unique_Proteins_in_Publication`
    *   Better apportionment approaches for both G2P and P2Prot links are welcome!

4.  **Aggregation & Output:**
    *   For each protein, sum the `Amount_per_Protein_from_Publication` across all publications it is associated with. This gives the total funding per protein.
    *   For the time-series visualization, aggregate the funding per protein per publication year (from 2010 onwards). The "publication year" will be the primary temporal anchor for the visualization.

## Deliverables

1.  **Dataset of Funding per Protein:**
    *   A CSV file (or similar structured format) with columns such as:
        *   `Protein_ID` (UniProtKB ID)
        *   `Gene_Symbol` (for better recognition)
        *   `Total_Funding_Approach1` 
        *   `Total_Funding_Approach2` (if a second approach is developed)
        *   `Funding_by_Year_YYYY` (columns for each year from 2010, e.g., `Funding_2010`, `Funding_2011`, ..., `Funding_2023`)

2.  **Simple Website for Tracking Funding Over Time:**
    *   A static HTML/JS website (or a simple Python web app using Flask/Streamlit/Dash).
    *   **Features:**
        *   Input field to search for a protein (by Protein ID or Gene Symbol).
        *   Upon search, display a line chart showing the estimated funding amount attributed to that protein for each year from 2010 onwards.
        *   Option to select which funding apportionment approach's results are displayed (if multiple are fully implemented).

## Evaluation Criteria

Submissions will be evaluated on:

- **Integration of multiple data types** (Hi-C, expression, annotations, etc.)
- **Clarity**, **documentation**, and **reproducibility** of your analysis
- **Creativity** in website interface

**Bonus points for**:

- Additional funding database resources (e.g., NSF)
- Creative funding apportionment approaches
- Reusable pipeline code

---

## Repository Structure

```text
.
├── data/             # Input data: expression, Hi-C, annotations
├── notebooks/        # Jupyter or R notebooks
├── scripts/          # Python/R scripts for analysis
├── results/          # Output files, figures
├── website/          # Website files (html or Streamlis/Flask/Dash app)
├── README.md         # This readme file
└── requirements.txt  # Software dependencies
```


## Requirements

1.  **Prerequisites:**
    *   Python 3.x
    *   Git
    *   A web browser

2.  **Setup:**

3.  **Data Processing and Storage:**

4.  **Website Hosting:**


## Future Work
*   More Sophisticated Apportionment Models
*   Expanded Data Sources
*   Enhanced Visualization
*   Enhanced Data Accessibility 

