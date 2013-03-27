# Open Civic Data Division Identifiers

The goal of this project is to provide somewhat predictable and globally unique identifiers for political divisions of all kinds.

## Definitions

* **Division** - a political geography such as state, county, or congressional district.  May have multiple `Boundaries` over their lifetime.  The IDs described in this document uniquely identify these divisons.  Three (possibly more) types of divisions have been discussed, but all can be treated the same for the purpose of this document:
   * Governmental Jurisdiction - A division that a government has jurisdiction over. (e.g. North Carolina)
   * Political District - A division that elects a representative to an appropriate Governmental Jurisdiction (e.g. North Carolina Congressional District 4)
   * Service Zone - an area for which a government provides a service, such as a police or fire district.  (e.g. Washington DC Police District 105) 
* **Boundary** - an actual boundary, defined by a shapefile or sequence of address ranges.  (e.g. NC Congressional District 10 for the 113th Congress)  **This document does not attempt to assign unique IDs to boundaries.**

## ID Format

IDs are in the format `ocd-location/country:<country_code>[/<type>:<type_id>]+`

* **country_code** - [ISO-3166-1 alpha-2 code](http://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) for country
* **type** - type of boundary (e.g. `'country', 'state', 'town', 'city', 'cd', 'sldl', 'sldu'`)  
  * As work progresses a list of possible types will be curated, but type is mostly open-ended.
  * Types should be comprised entirely of lower case letters.
* **type_id** - A unique identifier for the entity at this level.  
  * Valid Characters are UTF-8 letters normalized to lower case, numerals 0-9, period (.), dash(-), underscore (\_), and tilde (~).
      * These characters match the unreserved characters in a URI [RFC 3986 section 2.3](http://www.rfc-editor.org/rfc/rfc3986.txt).
  * Spaces should be converted to underscores.
  * Special/punctuation characters (other than period and dash) should be converted to ~.
  * Leading zeros should be dropped unless doing so changes the meaning of the identifier.


## Assignment

* IDs should not attempt to capture the full hierarchy of all entities, but enough to be uniquely descriptive.  (e.g. If there are school districts at the county and city level, county & city are important disambiguators and should be included in the identifier)
* Whenever possible, all geographic ids of a given type should be defined at the same time, for example all state geographies should be defined up front.  Similarly, all cities within North Carolina should be defined at once to avoid accidentally choosing a conflicting name.
* If an set of commonly accepted identifiers for a type already exists (such as postal code for US states) it should be used.  Numeric ids (such as county FIPS codes) should not be used if textual names are clear and unambiguous, but may be appended to help resolve ambiguities.
* Judgement should be used to not to grow the set of types unnecessarily.  A list of existing types should be published and new ids making use of an undefined type_id should be appropriately justified.

## Examples

* United States
  * ocd-division/country:us
* North Carolina
  * ocd-division/country:us/state:nc
* North Carolina 2nd Congressional District
  * ocd-division/country:us/state:nc/cd:2
* North Carolina State Lower Legislative District 1 
  * ocd-division/country:us/state:nc/sldl:1
* Wake County, North Carolina
  * ocd-division/country:us/state:nc/county:wake
* Cary, North Carolina  (note that despite being within Wake County this is not indicated due to not being an identifying feature)
  * ocd-division/country:us/state:nc/town:cary
* Kildaire Farms Homeowners Association, Cary, North Carolina 
  * ocd-division/country:us/state:nc/town:cary/hoa:kildairefarms
* Washington DC, Ward 8
  * ocd-division/country:us/district:dc/ward:8 
* Washington DC, ANC 4A
  * ocd-division/country:us/district:dc/anc:4a
* Washington DC, ANC 4A, section 08  _note: this is a strict subset of the ANC for purposes of representation_
  * ocd-division/country:us/district:dc/anc:4a/section:8
* New York City, City Council District 36 (happens to be in Brooklyn- but not significant to include in id)
  * ocd-division/country:us/state:ny/city:nyc/councildistrict:36
* Canadian Federal Electoral District 13004 aka [Fundy Royal](http://en.wikipedia.org/wiki/Fundy_Royal) (known as Royal from 1914-1966, Fundy-Royal from 1966-2003, and Fundy from 2003-2004- hence the use of a numeric identifier assigned by the government)
  * ocd-division/country:ca/fed:13004
