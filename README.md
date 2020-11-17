# Documentation for the RM title query

## Contents
* [Status](https://github.com/LM-15/falltest/blob/main/README.md#status)
* [Purpose](https://github.com/LM-15/falltest/blob/main/README.md#purpose)
* [Filters](https://github.com/LM-15/falltest/blob/main/README.md#filters)
* [Output](https://github.com/LM-15/falltest/blob/main/README.md#output)
* [To be done before the R1 2021 release? / Questions for Nancy and Axel](https://github.com/LM-15/falltest/blob/main/README.md#to-be-done-before-the-r1-2021-release--questions-for-nancy-and-axel) 
* [Requests not yet addressed](https://github.com/LM-15/falltest/blob/main/README.md#requests-not-yet-addressed) 


## Status
This query has been reviewed, but it is being updated to use the relevant derived tables.

## Purpose
To provide **title** counts for **non-electronic** resources cataloged in the Inventory.  

<details>
  <summary>Click to read more!</summary>
  
  * Provides unique title counts (i.e., only one count if more than one copy/subscription).  
  * Each institution will want to modify this query to suit their local needs. This query was built to include many of the measures commonly used to get overall title counts, such as those that record bibliographic format and library location information. Some parameter filters are available.  We also try to spell out which assumptions are made, some of which individual institutions may need to adjust. 
  * Queries to count e-resources (whether tracked through the ERM or the Inventory) are available separately. Each reporter must know where their institution’s various resources are tracked and should find the needed reports as appropriate, adding together counts if needed, and avoiding any duplication if possible.
  * Note that it is generally assumed that if you need a holdings count as of a certain date, you take it on that date; while you may be able to use processing dates to exclude resources newly added after a certain date, you cannot get back titles that were withdrawn or transferred.
  * Local and national definitions can be updated from year to year; be sure to review for needed changes.
  </details>
  
  ## Filters
  
  #### Hardcoded filters (assumptions):
* Includes only titles cataloged and made ready for use.
* Excludes: e-resources; suppressed instance records, and instance records with only suppressed holdings records.  

<details>
  <summary>Click to read more!</summary>
  
  * Each instance has a holdings record.  Each holdings record has a permanent location.
  * Excludes suppressed instance records (instance discovery suppress value is “true”)
  * [When this field becomes available:] Excludes instance records that do not have at least one unsuppressed holdings record (all holdings discovery suppress values are “true”)
  * [This hardcoded filter is currently commented out because of a lack of test data.] Includes only those titles cataloged and made ready for use (records with instance statuses names of “cataloged” or “batch loaded”).  Note that if your institution sets an instance status of, e.g., "pda unpurchased" you can exclude unpurchased patron driven acquisitions items if needed. 
  * This query is intended to exclude e-resources. It excludes instance records with instance format names of “computer – online resource” or “ISNULL,”  and excludes instance records with holdings library names of “Online” or “ISNULL.” These values many need to be updated for your local needs.
  </details>
  
#### Parameter filters:

* Throught parameter filters, this SQL allows you to easily type in text to filter by: resource format, receipt status, date, location and call number.  

<details>
  <summary>Click to read more!</summary>
  
  * Resource format: (Reporters need to know how their institution's records format information locally; it may use one of more of these commonly used fields, but not all of them.)
    * Instance types name (e.g., text, video, computer dataset, etc.)  (query allows up to three selected simultaneously)
    * Instance formats name (e.g., video – videocassette, unmediated – sheet, microform – microfilm roll, etc.)  (query allows up to three selected simultaneously)
    * Instance nature of content terms (e.g., autobiography, journal, newspaper, research report, etc.)
    * Instance statistical code name
    * Holdings statistical code name
    * Inventory modes of issuance name (e.g., serial, integrating resource, single unit, unspecified, etc.)
    * Holdings types name (e.g., physical, electronic, serial, mutli-part monograph, etc.)
* Receipt status:
  * Holdings receipt status (e.g., not currently received)
* Date:
  * Cataloged date (allows you to specify start and end date)
* Location: (where housed) (institutions with a consortial database may need to filter with their institutional location information to verify ownership (i.e., presence of instance record alone not enough))
  * Holdings permanent location id
  * Holdings location name
  * Holdings campus name
  * Holdings institution name
* Call number:
  * Holdings call number types name (e.g., LC, NLM, Dewey Decimal, etc.)
  * Holdings call number (note that the call number field is a text string only (no breakouts); you may want to use truncation symbols as suggested in the filter to get at call number ranges)
  </details>
  
  #### Other fields you might want to filter on in results:
    * Instance previously held  (to identify titles digitized owned previously in paper)
    * Super relation type name  (to identify titles that are analyzed as part of a larger title; to be able to exclude if wanted if counting only parent titles)
    * Sub relation type name

## Output
Aggregation: This query provides counts grouped by:
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

## To be done before the R1 2021 release? / Questions for Nancy and Axel
<details>
  <summary>Click to read more!</summary>
  
* Remove the query comments that say we will add more values to exclude e-resources (we added some text above to indicate institutions may need to adjust the values).
* language - should we add?  Appears to be available in the Instance JSON info. It is supposed to be repeatable. I think Laura D. said that the first language would be the primary language if there is one.  Will the source record provide language information in a more standardized way eventually?
* If the holdings discover suppress field becomes available, add it and update the query commenting.
* Un-comment-out the hardcoded filter for instance statuses which we commented out because of a lack of test data. 
* Is "date published" usable yet?  Or will the data be better from the source records?  It is listed in the query in the MAIN TABLES WITH NEEDED COLUMNS commented section of the query, but also in the STILL IN PROGRESS section of the query.
* Axel, is the note about using institutinal locations (in the readme paramter filters section) good enough on the consortial database issue?
* About filtering by call nubmer: all we can advise is using truncation for the call number fitler, right?  No changes on call number parts being separated right?
* "super relation type name"; "sub relation type name"  Can we document what we think these fields are useful for? Are we using them to identify titles that are titles analyzed from within a larger title; to be able to exclude if wanted if counting only parent titles?  I noticed that there is a "bound with" value for the inventory instance relationships types name measure, but I think earlier notes say Laura Daniels thought bound with info would be best through the holdings record (a true/false measure)?
* Do I have the output correct?
* What to call the next section if not "Requests not yet addressed."  (Some ealier suggestions:  in progress; items to take into consideration; items to keep in mind)
* Do we want to add acquistion method to identify items recieved as gifts, or is that measure too unreliable?
* Do we want to add inventory statistical code types?  Chicago uses?
* What is the difference between permanent loc and library name?
* Will folks think it's odd that we're not counting e-resources tracked in the Inventory in the same query?  Guess not maybe for items.
* * Is Instance previously held to identify titles digitized owned previously in paper?
  </details>
  
## Requests not yet addressed
<details>
  <summary>Click to read more!</summary>
  
  See this page for additional information recorded by the Resource Management reporters: https://wiki.folio.org/x/OA8uAg 
  * Counting separately multiple formats cataloged on the same instance record (maybe by unique instances and unique holdings formats?)
  * Information tracked possibly through holdings records notes?: previous bindings, copy notes, dedications, inscriptions, left by decedents?
  * When fields available?:
    * country of publication (source record)
    * geographic area code (source record)
    * is open access (source record?)
    * withdrawn in timeframe (instance supresssed with status update date in timeframe??)
    * transferred within the institution in a time period
    * has retention requirements / is an obligatory copy (have retention policy field on holdings?)
    * is government document
    * acquired as part of a project
    * identifying records for collections like CRL if in catalog, so can be excluded for national reporting
  </details>
