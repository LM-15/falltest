# Documentation for the RM title query

## Contents
* [Status](https://github.com/LM-15/falltest/blob/main/README.md#status)
* [Purpose](https://github.com/LM-15/falltest/blob/main/README.md#purpose)
* [Filters](https://github.com/LM-15/falltest/blob/main/README.md#filters)
* [Output](https://github.com/LM-15/falltest/blob/main/README.md#output)
* [In Progress](https://github.com/LM-15/falltest/blob/main/README.md#in-progress) 


## Status
This query has been reviewed, but we are updating it to use relevant derived tables.

## Purpose
To provide **title** counts for **non-electronic** resources cataloged in the Inventory.  

<details>
  <summary>Click to read more!</summary>
  
  * Provides unique title counts (i.e., only one count if more than one copy/subscription).  
  * Each institution will want to modify this query to suit their local needs.  This query is built to include many of the measures commonly used to get overall title counts, such as those recording bibliographic format and library location information.  Some paramter filters are available.  We also try to spell out which assumptions are made that some institutions may need to adjust. 
  * Queries to count e-resources (whether tracked through the ERM or the Inventory) are available separately. Each reporter must know where her/his institution’s various resources are tracked and find the needed reports as appropriate, adding together counts if needed, and avoiding any duplication if possible.
  * Note that it is generally assumed that if you need a holdings count as of a certain date, you take it on that date; while you may be able to use processing dates to exclude resources newly added after a certain date, you cannot get back titles that were withdrawn or transferred.
  * Local and national definitions can be updated from year to year; be sure to review for needed changes.
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
  * Includes counts of only those titles cataloged and made ready for use (records with instance statuses names of “cataloged” or “batch loaded”).  Note that if your institution sets an instance status of, e.g., "pda unpurchased" you can exclude unpurchased patron driven acquisitions items if needed. [This hard coded filter is currently commented out because of a lack of test data]
  * This query is intended to exclude e-resources.  It excludes instance records with instance format names of “computer – online resource” or “ISNULL,”  and excludes instance records with holdings library names of “Online” or “ISNULL.” These values many need to be updated for your local needs.
  </details>
  
#### Parameter filters:

* Throught parameter filters, this SQL allows you to easily type in text to filter by: resource format, receipt status, date, location and call number.  

<details>
  <summary>Click to read more!</summary>
  
  * Resource format (Reporters need to know how their institutions records format information locally; it may use one of more of these fields, but not all of these commonly used fields listed here.)
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
* Location: (where housed) (institutions with a consortial database may to specify location information to verify ownership (e.g., instance record not enough alone))
  * Holdings permanent location id
  * Holdings location name
  * Holdings campus name
  * Holdings institution name
* Call number:   [how do we suggest they use?]
  * Holdings call number types name (e.g., LC, NLM, Dewey Decimal, etc.)
  * Holdings call number
  * Note that the call number field is a text string only (no breakouts)
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

## To be done before the R1 2021 release / Questions for Nancy and Axel
What are the "super relation type name" and the "sub relation type name"?  Are we using them to identify titles that are titles analyzed from within a larger title; to be able to exclude if wanted if counting parent title?
I noticed a "bound with" relationsi


TO BE DONE BEFORE R1 2021 PENDING ITEMS WHAT CAN WE GET DONE.  REQUIRE THE INSTITION TO BE A SPECIFIC THING

## In Progress    ITEMS TO TAKE INTO CONSIDERATION KEEP IN MIND...
<details>
  <summary>Click to read more!</summary>
  See this page for additiona info recorded by the Resource Management reporters: https://wiki.folio.org/x/OA8uAg 
  
* Will add/address these requests when:
  * More records are available in the LDP  REMOVE THIS BECAUSE WILL BE UP ABOVE
    * the hardcoded filter for instance statuses names of “cataloged” or “batch loaded” is currently commented out due to lack of test data.  Users can enable it any time they wish.
  * We find out more about how institutions are coding or when fields are available in LDP  TELLING PEOPLE THEY MAY NEED TO UPDATE IN ASSMPTIONS
    * add more filters and values for virtual titles as hardcoded filter (instance type, nature of content, inventory libraries name)
  * When have more time?
    * counting separately multiople formats attached to the same record (maybe by unique instances and unique holdings formats)
    * consortial concerns for counting
    * info tracked possibly through holdings records notes?: previous bindings, copy notes, dedications, inscriptions
    * language (but also more standardizable from source record? In instance record repeatable, but if primary would be first language)
  * Data is accessible in LDP, or accessible in a more standard way? 
    * dateOfPublication (date of publication from source record more standardizable?)
    * holdings discovery suppress (not in LDP at this time)
    * instance status updated date (not in LDP at this time)
    * country of publication (soure record)
    * geographic area code (source record)
    * language (language from source record more standardizable?)
    * if open access item (source record?)
    * withdrawn in timeframe (instance supresssed with status update date in timeframe??)
    * transferred within the institution in a time period
    * bound with (will be check box on holdings?)
    * has retention requirements / is an obligatory copy (have retention policy field on holdings)
    * government document
    * left by decedents
    * Received as gifts
    * Acquired as part of a project
    * Identifying records for collections like CRL if in catalog, so can be excluded for national reporting
  </details>
