# Documentation of the RM title query

## Contents
* [Status](https://github.com/LM-15/falltest/blob/main/README.md#status)
* [Purpose](https://github.com/LM-15/falltest/blob/main/README.md#purpose)
* [Filters](https://github.com/LM-15/falltest/blob/main/README.md#filters)
* [Output](https://github.com/LM-15/falltest/blob/main/README.md#output)
* [In Progress](https://github.com/LM-15/falltest/blob/main/README.md#in-progress) 


## Status
This query has been reviewed for code syntax and style, approved by the community, and provided with machine test files.

## Purpose
To provide title counts for non-electronic resources cataloged in the Inventory.  

<details>
  <summary>Click to read more!</summary>
  
  * Provides unique title counts (i.e., only one count if more than one copy/subscription).  See assumptions and filters available below. Note that it is It is generally assumed that if you need a holdings count as of a certain date, you take it on that date; while you can use processing dates to exclude items newly added after a certain date, you cannot get back titles that were withdrawn or transferred through this query. Local and national definitions can be updated from year to year; be sure to review for needed changes.
  </details>
  
  ## Filters
  
  #### Harcoded filters (assumptions):
* Includes only titles cataloged and made ready for use.
* Excludes: e-resources; suppressed instance record counts, and counts of instance records with only suppressed holdings records.  

<details>
  <summary>Click to read more!</summary>
  
  * Each instance has a holdings record.  Each holdings record has a permanent location.
  * Excludes suppressed instance records (instance discovery suppress value is “true”)
  * [When this field becomes available:] Excludes instance record that do not have at least one holdings record not suppressed (all holdings discovery suppress values are “true”)
  * Includes counts of titles cataloged and made ready for use (records with instance statuses names of “cataloged” or “batch loaded”).  Note that if  your institution sets an instance statuses name for unpurchased patron driven acquisition items, they can be excluded in this way.  [This hard coded filter not currently set because of a lack of test data]
  * Excludes instance records with instance format names of “computer – online resource” or “ISNULL.”  And excludes instance records with holdings library names of “Online” or “ISNULL.”  (Online resource counts are excluded even if tracked in the Inventory; see the ERM queries for online titles held counts. Each reporter must know where her/his institution’s various resources are tracked and find the needed reports as appropriate, adding together counts if needed, and avoiding any duplication if possible.)
  </details>
  
#### Parameter filters:

SQL allows you to input text to filter by: resource format, receipt status, date, location and call number.

<details>
  <summary>Click to read more!</summary>
  
  * Resource format (Reporters need to know how their institutions records format information locally; it may use one of more of these fields, but not all of these commonly used fields listed here.  You may want to remove all references to unneeded fields throughout the query.
    * Instance types name (e.g., text, video, computer dataset, etc.)  (query allows up to three selected simultaneously)
    * Instance formats name (e.g., video – videocassette, unmediated – sheet, microform – microfilm roll, etc.)  (query allows up to three selected simultaneously)
    * Instance nature of content terms (e.g., autobiography, journal, newspaper, research report, etc.)
    * Instance statistical code name
    * Holdings statistical code name
    * Inventory modes of issuance name (e.g., serial, integrating resource, single unit, unspecified, etc.)
    * Holdings types name (e.g., physical, electronic, serial, mutli-part monograph, etc.)
* Receipt status
  * Holdings receipt status (e.g., not currently received)
* Date:
  * Cataloged date (allows you to specify start and end date)
  * [Is this usable yet?] Date published
* Location: (where housed)
  * Holdings permanent location id
  * Holdings location name
  * Holdings campus name
  * Holdings institution name
* Call number  [how do we suggest they use?]
  * Holdings call number types name (e.g., LC, NLM, Dewey Decimal, etc.)
  * Holdings call number
  </details>
  
  #### Other fields you might want to filter on in results:
* Instance previously held
* Super relation type name  (to identify titles that are titles analyzed from within a larger title; to be able to exclude if wanted if counting parent title)
* Sub relation type name

## Output
Aggregation:  this report provides counts grouped by:
* Instance types name
* Modes of issuance name
* Instance formats name
* Instance statistical code name
* Instance nature of content terms
* Holdings types name
* Holdings call number types name
* Holdings statistical code name
* Holdings receipt status
* Instance previously held
* Holdings location name
* Super relation type name  
* Sub relation type name

## In Progress
<details>
  <summary>Click to read more!</summary>
  
* Will add/address these requests when:
  * More records are available in the LDP
    * the hardcoded filter for instance statuses names of “cataloged” or “batch loaded” is not yet included because of a lack of test data.
  * We find out more about how institutions are coding or when fields are available in LDP
    * add more filters and values for virtual titles as hardcoded filter (instance type, nature of content, inventory libraries name)
  * Data is accessible in LDP
    * dateOfPublication (date of publication from MARC source more standardizable?)
    * holdings discovery suppress (not in LDP at this time)
    * instance status updated date (not in LDP at this time)
    * geographic area about
    * language
    * if open access item
    * withdrawn in timeframe
    * transferred within the institution in a time period
    * has retention requirements / is an obligatory copy
    * left by decedents
    * Received as gifts
    * Acquired as part of a project
    * Identifying records for collections like CRL if in catalog, so can be excluded for national reporting
  </details>
