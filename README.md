# Documentation of the RM title query

## Contents
* [Status]
* [Purpose]
* [Hardcoded filters (assumptions)]


## Status

## Purpose
To provide title counts for non-electronic resources cataloged in the Inventory.  

<details>
  <summary>Click to read more!</summary>
  
  * Provides unique title counts (i.e., only one count if more than one copy/subscription).  See assumptions and filters available below. Note that it is It is generally assumed that if you need a holdings count as of a certain date, you take it on that date; while you can use processing dates to exclude items newly added after a certain date, you cannot get back titles that were withdrawn or transferred through this query. Local and national definitions can be updated from year to year; be sure to review for needed changes.
  </details>
  
  ## Hardcoded filters (assumptions):
*	If appropriate, has holdings records.
*	The holdings records store permanent locations.
*	Excludes suppressed instance records (instance discovery suppress value is “true”)
*	[When this field becomes available:] Excludes instance record that do not have at least one holdings record not suppressed (all holdings discovery suppress values are “true”)
*	Includes counts of titles cataloged and made ready for use (records with instance statuses names of “cataloged” or “batch loaded”).  Note that if  your institution sets an instance statuses name for unpurchased patron driven acquisition items, they can be excluded in this way.  [This hard coded filter not currently set because of a lack of test data]
*	Excludes instance records with instance format names of “computer – online resource” or “ISNULL.”  And excludes instance records with holdings library names of “Online” or “ISNULL.”  (Online resource counts are excluded even if tracked in the Inventory; see the ERM queries for online titles held counts. Each reporter must know where her/his institution’s various resources are tracked and find the needed reports as appropriate, adding together counts if needed, and avoiding any duplication if possible.)
