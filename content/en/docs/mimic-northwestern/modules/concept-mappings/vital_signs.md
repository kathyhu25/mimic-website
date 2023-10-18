---
title: "vital signs"
linktitle: "vital signs"
weight: 3
date: 2023-09-18
description: >
  Numeric vital sign concept mappins within respiratory, routine, general, and hemodynamic categories.
---

Vital signs data in MIMIC-Northwestern is split between two tables in the ICU module, the d_items and chartevents. The d_items table is the definition table for all concepts recorded in the events tables in the ICU module, including the chartevents table, and the chartevents table contains all the charted data available for a patient during their ICU stay, including vital signs readings. 

Data from the d_items table was used for mapping vital signs values to LOINC, including the label and the unit of measurement. Concepts in the d_items table are stratified into various categories. Most vital signs are under the category 'Routine Vital Signs'. However, some vital signs, such as respiratory rate and O2 saturation, are listed under the category 'Respiratory'. Similar to lab results, the online tool SearchLOINC was used to map vital signs to LOINC v2.72.

# Table columns

| Name            | Postgres data type  |
| --------------- | ------------------- |
| `itemid`        | INTEGER             |
| `label`         | VARCHAR(200)        |
| `abbreviation`  | VARCHAR(100)        |
| `category`      | VARCHAR(100)        |
| `unitname`      | VARCHAR(100)        |
| `loinc`         | VARCHAR(50)         |
| `loinc_label`   | VARCHAR(200)        |
| `omop_id`       | VARCHAR(50)         |
| `count`         | INTEGER             |
| `author_id`     | VARCHAR(50)         |
| `reviewer_id`   | VARCHAR(50)         |

## Detailed description

### `itemid`
A unique identifier for a single measurement type in the database.
###  `label`
The label column describes the concept which is represented by the itemid. This refers to an inhouse concept used from BIDMC or Northwestern.
### `abbreviation`
The abbreviation column lists a common abbreviation for the label. This column is harmonized to MIMIC's concepts.
### `category`
category provides some information on the type of data the itemid corresponds to. This column is harmonized to MIMIC's concepts.
### `unitname`
unitname specifies the unit of measurement used for the itemid. This column is harmonized to MIMIC's concepts.
### `loinc`
loinc contains the LOINC code associated with the given itemid. The Logical Observation Identifiers Names and Codes (LOINC) is an ontology which originally specified laboratory measurements but has since expanded to cover a wide range of clinically relevant concepts.
### `loinc_label`
loinc_label contains the LOINC Long Common Name for the loinc code.
### `omop_id`
omop_id is a unique identifier for each Concept across all domains in the Observational Medical Outcomes Partnership (OMOP) common data model.
### `count`
The count of chartevents instances for a given itemid.
### `author_id`
ORCIDs that Identifies the persons or groups responsible for asserting the mappings.
### `reviewer_id`
ORCIDs that Identifies the persons or groups that reviewed and confirmed the mapping.

## Harmonization process

Northwestern concepts were mapped to match MIMIC concepts.
To enable the harmonization to mimic's data structure, mimic concept were initially mapped to loinc codes. There were two concepts in MIMIC under the “Routine Vital Signs” category that we were unable to map because no suitable LOINC code exists for these concepts. Fortunately, these concepts account for only 0.02% of total vital signs events in MIMIC.

| Vital sign name | Mapping issue | Comments |
|-----------------|---------------|----------|
| Bladder Pressure | No LOINC code available | Bladder pressure is a technique used to monitor intra-abdominal pressure. |
| Doppler BP | No LOINC code available | Doppler is sometimes used in the process of measuring systolic blood pressure if the pulse is faint. |

Distribution of mapped concepts:

|                   | Number of concepts (N=42) | % of total Concepts | % of events (N=44,317,869) |
|-------------------|---------------------------|---------------------|---------------------------|
| Exact match       | 40                        | 95.20%              | 99.98%                    |
| Unable to map     | 2                         | 4.80%               | 0.02%                     |
| - No suitable LOINC code available | 2    | 4.80%               | 0.02%                     |
