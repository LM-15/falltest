# Documentation for the RM title query

## Contents
* [Status](https://github.com/LM-15/falltest/blob/main/README.md#status)
* [Purpose](https://github.com/LM-15/falltest/blob/main/README.md#purpose)
* [Filters](https://github.com/LM-15/falltest/blob/main/README.md#filters)
* [Output](https://github.com/LM-15/falltest/blob/main/README.md#output)
* [To be done before the R1 2021 release? / Questions for Nancy and Axel](https://github.com/LM-15/falltest/blob/main/README.md#to-be-done-before-the-r1-2021-release) 
* [Requests not yet addressed](https://github.com/LM-15/falltest/blob/main/README.md#requests-not-yet-addressed) 


## Status
As of 11/18/20, this query has been reviewed, but it is being updated to use the relevant derived tables.

## Purpose
To provide **title** counts for **non-electronic** resources cataloged in the Inventory, by various filters.  

<details>
  <summary>Click to read more!</summary>
  
  * Provides unique title counts (i.e., only one count if more than one copy/subscription).  
  * Modify this query to suit your local needs. This query was built to include many of the measures commonly used to get overall title counts, such as those that record bibliographic format and library location information. Some parameter filters are available.  We also try to spell out which assumptions are made (some of which individual institutions may need to adjust), and requests not yet addressed. 
  * Queries to count e-resources (whether tracked through the ERM or the Inventory) are available separately. Each reporter must know where their institution’s various resources are tracked and should find the needed reports as appropriate, adding together counts if needed, and avoiding any duplication if possible.
  * Note that it is generally assumed that if you need a holdings count as of a certain date, you take it on that date; while you may be able to use processing dates to exclude resources newly added after a certain date, you cannot get back titles that were withdrawn or transferred.
  * Local and national definitions can be updated from year to year; be sure to review for needed changes.
  </details>
  
  ## Filters
  
  #### Hardcoded filters (assumptions; in the where clause):
* Excludes: e-resources; suppressed instance records, and instance records with only suppressed holdings records.  

<details>
  <summary>Click to read more!</summary>
  
  * Each instance has a holdings record.  Each holdings record has a permanent location.
  * Excludes suppressed instance records (instance discovery suppress value is “true”)
  * [When this field becomes available:] Excludes instance records that do not have at least one unsuppressed holdings record (all holdings discovery suppress values are “true”)
  * This query is intended to exclude e-resources. It excludes instance records with instance format names of “computer – online resource” or “ISNULL,”  and excludes instance records with holdings library names of “Online” or “ISNULL.” These values many need to be updated for your local needs.
  </details>
  
#### Parameter filters (at the top of the query):

* Through parameter filters, this SQL allows you to easily type in text to filter by: instance status, resource format, receipt status, language, date, location and call number.  

<details>
  <summary>Click to read more!</summary>
  
  * Instance statuses:
    * Instance statuses name (you can use this parameter to include only those titles cataloged and made ready for use; for many institutions, this would be "cataloged" and "batchloaded"; note that if your institution sets an instance status of, e.g., "pda unpurchased" you can exclude unpurchased patron driven acquisitions items if needed) (query allows up to two selected simultaneously)
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
* Language:
  * Languages (will include a value for each language used; if more than one language, the first is the primary language if there is one; use %% as wildcards; use, e.g., "%%eng%%" to get all titles that are fully or partially in english.)
* Date:
  * Cataloged date (allows you to specify start and end date)
* Location: (where housed) (institutions with a shared consortial database may need to filter with their institutional location information to verify ownership (i.e., presence of instance record alone not enough))
  * Holdings permanent location id (typically the lowest level in the location hierarchy -- the specific location within a library)
  * Holdings location name
  * Holdings campus name
  * Holdings institution name
* Call number:
  * Holdings call number types name (e.g., LC, NLM, Dewey Decimal, etc.)
  * Holdings call number (note that the call number field is a text string only (no breakouts); you may want to use truncation symbols as suggested in the filter to get at call number ranges)
  </details>
  
  #### Other fields you might want to filter on in results:
    * Instance previously held  (indicates the item was "previously held" in terms of, for example, HathiTrust digital access)
    * Super relation type name  (content within titles is sometimes analyzed (cataloged) as part of the larger, parent title; if you need to avoid including one level in your count in such cases, this and the following measure will allow you to exclude one or the other)
    * Sub relation type name (see immediately above)

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

## To be done before the R1 2021 release
<details>
  <summary>Click to read more!</summary>
  
* In the WHERE clause, update the comment from "-- filter all virtual titles (surely need more virtual indicators)" to "-- filter all virtual titles (update values as needed)."
* Add "language" from the instance JSON data. I assume it would have a parameter filter? There is more than one value if there is more than one language. If more than one language, the first is the primary language if there is one.  We would indicate to use truncation right?  Guess we would advise using, e.g., "%%eng%%", because there is not always a primary language? I don't think the source record would make it any clearer: https://www.loc.gov/marc/bibliographic/bd041.html   https://www.loc.gov/marc/bibliographic/bd008a.html 
* Add two parameter filters for instance statuses name with "Cataloged" and "Batchloaded" as examples, and remove this from the WHERE clause hardcoded filters, including the comment used because of a lack of test data.  See note above in parameters section. 
* Please remove dateofpublication from the query's MAIN TABLES WITH NEEDED COLUMNS SECTION, as it is in the query's STILL IN PROGRESS SECTION.  We will note: At this point in time, we are not bringing in the instance dataofpublication because it is not in standardized form; institutions may want to consider bringing it in if they set up parsing options to suit their needs. Will likely add date one and date two data from the source record when available (e.g., MARC  008 (places 7-10 for date 1, and 11-14 for date 2)).
* LINDA TO ASK LAURA IF THIS IS STILL CORRECT: Super relation type name / Sub relation type name: (content within titles is sometimes analyzed (cataloged) as part of the larger, parent title; if you need to avoid including one level in your count in such cases, this and the following measure will allow you to exclude one or the other.
* NANCY WILL LOOK AT WHEN QUERY REDONE: Do I have the output correct?
* LINDA WILL ASK LAURA IF MANY PEOPLE WILL USE: Do we want to add the holdings acquisition method ("purchased" is example in the folio-snapshot; MM document lists things like "gift", "deposit", "membership", "cooperative or consortial purchase", "lease" etc.) to identify items received as gifts, or is that measure too unreliable?  MM list: https://docs.google.com/spreadsheets/d/1RCZyXUA5rK47wZqfFPbiRM0xnw8WnMCcmlttT7B3VlI/edit#gid=139536469
* LINDA WILL ASK LAURA IF MANY PEOPLE WILL USE: Do we want to add inventory statistical code types?  Chicago uses?
* LINDA WILL ASK LAURA ABOUT THIS; FOR PRINT ITEMS, THE MM FILE SAYS THE primary use case is to support rights to HathiTrust content.  DOES THIS MEAN SPECIFICALLY THAT WE DIGITIZED IT AND THEREFORE HAVE ACCESS RIGHTS? OR SOMETHING ELSE ALTOGETHER?  AXEL SUGGESTED SERIES? 
  </details>
  
## Requests not yet addressed
<details>
  <summary>Click to read more!</summary>
  
  See this page for additional information recorded by the Resource Management reporters: https://wiki.folio.org/x/OA8uAg 
  * Counting separately multiple formats cataloged on the same instance record (maybe by unique instances and unique holdings formats?)
  * Information tracked possibly through holdings records notes?: precious bindings, copy notes, dedications, inscriptions, left by decedents? Use a filter with truncation. Which measures each institution uses to track this information could differ.
  * When fields available?:
    * When the holdings discover suppress field becomes available, add it to the WHERE hardcoded filters and update comment.
    * country of publication (source record)
    * date of publication (At this point in time, we are not bringing in the instance dataofpublication because it is not in standardized form; institutions may want to consider bringing it in if they set up parsing options to suit their needs. Will likely add date one and date two data from the source record when available (MARC  008 (places 7-10 for date 1, and 11-14 for date 2)).
    * geographic area code (source record)
    * is open access (source record?)
    * withdrawn in timeframe (instance suppressed with status update date in timeframe??)
    * transferred within the institution in a time period
    * has retention requirements / is an obligatory copy (have retention policy field on holdings?)
    * is government document (how this will be addressed by institutions can very greatly; statistical code, location, source record (not yet available; e.g., MARC 008, 086 for federal US/Canadian docs))
    * acquired as part of a project
    * identifying records for collections like CRL if in catalog, so can be excluded for national reporting
  </details>
