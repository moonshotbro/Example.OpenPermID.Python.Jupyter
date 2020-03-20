# Example.OpenPermID.Python.Jupyter

## Overview

This example demonstrates how to use a Python Open PermID library. The library covers all features of Open PermID APIs including Record Matching, Entity Search, and Intelligent Tagging. 

PermID is a shortening of “Permanent Identifier” which is a machine-readable number assigned to entities, securities, organizations (companies, government agencies, universities, etc.), quotes, individuals, and more. It is specifically designed for use by machines to reference related information programmatically. Open PermID is publicly available for free at [https://permid.org/](https://permid.org/).

The Python OpenPermID is available on [pypi.org](https://pypi.org/project/OpenPermID/). It can be installed via the following **pip** command.

```
pip install OpenPermID
```
To use the Python OpenPermID, the application needs to create an OpenPermID object and set an access token to it. The access token can be retrieved after login to the [Open PermID](https://permid.org/) website.


```python
from OpenPermID import OpenPermID

opid = OpenPermID()
opid.set_access_token("<ACCESS TOKEN>")
```

## 1. Entity Search

This function is used to search an entity's PermID value from a string. 
```
serach(q, entityType='all', format="dataframe", start=1, num=5, order='rel')
```
|Parameter Name|Required|Description|
|--------------|--------|-----------|
|q|Yes|A query string to search for. It could be either just the search string value, or prefix it with "<fieldname>:" to constrain the search to a specific field, such as "**refinitiv**", "**ticker:IBM**", and "**ticker: msft AND exchange:NSM**". For a list of all available fields, please refer to the [PermID User guide](https://developers.refinitiv.com/open-permid/permid-entity-search/docs?content=4885&type=documentation_item).|
|entityType|No|The type of entity to search for. Possible values are **all**, **organization**, **instrument**, or **quote**. The default value is **all**|
|format|No|The format of the output. Possible values are **dataframe**, **json**, or **xml**. The default value is **dataframe**|
|start|No|The index of the first result returned, in the list of results ordered according to the order parameter. The index is 1-based. The default value is 1.|
|num|No|The maximum number of results returned for each entity (separately). Possible values are 5, 10, 20, 50, and 100. The default value is 5.|
|order|No|The order of the search results. Possible values are **rel** (Descending order of relevance), **az** (Ascending alphabetical order of the entity name), or **za** ( Descending alphabetical order of the entity name). The default value is **rel**.|

This function returns a tuple containing a result and error string. When the **entityType** is **all** and the **format** is **dataframe**, it returns multiple data frames indexed by the entity types (**quotes**, **organizations**, and **instruments**). For other entity types with the **dataframe** format, it returns a data frame. The result could be a data frame, JSON, or XML string depending on the **format** parameter. 

The following code calls the **search** method to search for a "Refinitiv" string with the default parameters.



```python
output,err = opid.search('Refinitiv')
```

### Display the organizations' entities


```python
output['organizations']
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>@id</th>
      <th>hasURL</th>
      <th>orgSubtype</th>
      <th>organizationName</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>https://permid.org/1-8589934184</td>
      <td>NaN</td>
      <td>Company</td>
      <td>Refinitiv UK Financial Ltd</td>
    </tr>
    <tr>
      <th>1</th>
      <td>https://permid.org/1-5000120664</td>
      <td>https://www.refinitiv.com/ja</td>
      <td>Company</td>
      <td>Refinitiv Japan KK</td>
    </tr>
    <tr>
      <th>2</th>
      <td>https://permid.org/1-4296693138</td>
      <td>NaN</td>
      <td>Company</td>
      <td>Refinitiv Asia Pte Ltd</td>
    </tr>
    <tr>
      <th>3</th>
      <td>https://permid.org/1-4295921907</td>
      <td>NaN</td>
      <td>Investment Manager/Advisor</td>
      <td>Refinitiv Global Markets Inc</td>
    </tr>
    <tr>
      <th>4</th>
      <td>https://permid.org/1-5000693632</td>
      <td>NaN</td>
      <td>Company</td>
      <td>Refinitiv de Mexico SA de CV</td>
    </tr>
  </tbody>
</table>
</div>



### Display the instruments' entities


```python
output['instruments']
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>@id</th>
      <th>assetClass</th>
      <th>hasName</th>
      <th>isIssuedBy</th>
      <th>isIssuedByName</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>https://permid.org/1-21661915727</td>
      <td>Ordinary Shares</td>
      <td>Refinitiv US Holdings Ord Shs (Unlisted)</td>
      <td>https://permid.org/1-5064690523</td>
      <td>Refinitiv US Holdings Inc</td>
    </tr>
    <tr>
      <th>1</th>
      <td>https://permid.org/1-21705613453</td>
      <td>Ordinary Shares</td>
      <td>Refinitiv US Holdings Class C Ord Shs (Unlisted)</td>
      <td>https://permid.org/1-5064690523</td>
      <td>Refinitiv US Holdings Inc</td>
    </tr>
  </tbody>
</table>
</div>



### Display the quotes' entities


```python
output['quotes']
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>@id</th>
      <th>assetClass</th>
      <th>hasName</th>
      <th>hasRIC</th>
      <th>isQuoteOf</th>
      <th>isQuoteOfInstrumentName</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>https://permid.org/1-21705613554</td>
      <td>Ordinary Shares</td>
      <td>REFINITIV C ORD</td>
      <td>RFTb.UNL</td>
      <td>https://permid.org/1-21705613453</td>
      <td>Refinitiv US Holdings Class C Ord Shs (Unlisted)</td>
    </tr>
    <tr>
      <th>1</th>
      <td>https://permid.org/1-21661916398</td>
      <td>Ordinary Shares</td>
      <td>REFINITIV ORD</td>
      <td>RFT.UNL</td>
      <td>https://permid.org/1-21661915727</td>
      <td>Refinitiv US Holdings Ord Shs (Unlisted)</td>
    </tr>
  </tbody>
</table>
</div>

## 2. Entity Lookup

If you know a PermID of an entity, you can use the **lookup** method to retrieve the entity description. 

It accepts three parameters:

|Parameter Name|Required|Description|
|--------------|--------|-----------|
|id|Yes|The PermID used to lookup e.g. 1-5064690523|
|format|No|The format of the output. Possible values are **dataframe**, **json-ld**, or **turtle**. The default value is **dataframe**|
|orient|No|The format of the returned data frame. Possible values are **row**, or **column**. The default value is **row**|

This function returns a tuple containing a result and error string. The result could be a data frame, JSON, or turtle string depending on the **format** parameter.

The following code calls the **lookup** method to retrieve the entity information of the 1-5064690523 PermID with the **column** orient parameter.


```python
output,err = opid.lookup("1-5064690523", orient="column")
output
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>1-5064690523</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>@id</th>
      <td>https://permid.org/1-5064690523</td>
    </tr>
    <tr>
      <th>@type</th>
      <td>tr-org:Organization</td>
    </tr>
    <tr>
      <th>mdaas:HeadquartersAddress</th>
      <td>3 Times Sq\n\n\nNEW YORK\nNEW YORK\n10036-6564\n</td>
    </tr>
    <tr>
      <th>mdaas:RegisteredAddress</th>
      <td>200 Bellevue Pkwy Ste 210\n\n\nWILMINGTON\nDEL...</td>
    </tr>
    <tr>
      <th>tr-common:hasPermId</th>
      <td>5064690523</td>
    </tr>
    <tr>
      <th>hasPrimaryInstrument</th>
      <td>https://permid.org/1-21661915727</td>
    </tr>
    <tr>
      <th>hasActivityStatus</th>
      <td>tr-org:statusActive</td>
    </tr>
    <tr>
      <th>tr-org:hasHeadquartersPhoneNumber</th>
      <td>16462234000</td>
    </tr>
    <tr>
      <th>tr-org:hasLEI</th>
      <td>549300NF240HXJO7N016</td>
    </tr>
    <tr>
      <th>hasLatestOrganizationFoundedDate</th>
      <td>2018-03-16T00:00:00Z</td>
    </tr>
    <tr>
      <th>hasPrimaryBusinessSector</th>
      <td>https://permid.org/1-4294952762</td>
    </tr>
    <tr>
      <th>hasPrimaryEconomicSector</th>
      <td>https://permid.org/1-4294952767</td>
    </tr>
    <tr>
      <th>hasPrimaryIndustryGroup</th>
      <td>https://permid.org/1-4294952759</td>
    </tr>
    <tr>
      <th>tr-org:hasRegisteredFaxNumber</th>
      <td>13027985841</td>
    </tr>
    <tr>
      <th>tr-org:hasRegisteredPhoneNumber</th>
      <td>13027985860</td>
    </tr>
    <tr>
      <th>isIncorporatedIn</th>
      <td>http://sws.geonames.org/6252001/</td>
    </tr>
    <tr>
      <th>isDomiciledIn</th>
      <td>http://sws.geonames.org/2635167/</td>
    </tr>
    <tr>
      <th>hasURL</th>
      <td>https://www.refinitiv.com</td>
    </tr>
    <tr>
      <th>vcard:organization-name</th>
      <td>Refinitiv US Holdings Inc</td>
    </tr>
  </tbody>
</table>
</div>


## 3. Record Matching

The PermID Record Matching API allows you to match entity Person, Organization, Instrument,
and Quote records with Refinitiv’ PermIDs. 


```Python
match(data,dataType='Organization',numberOfMatchesPerRecord=1,raw_output=False)
```
|Parameter Name|Required|Description|
|--------------|--------|-----------|
|data|Yes|A CSV string or data frame for matching. For formats of the CSV string, please refer to the [PermID User guide](https://developers.refinitiv.com/open-permid/permid-entity-search/docs?content=4885&type=documentation_item).|
|dataType|No|The type of entity to search for. Possible values are **Person**, **Organization**, **Instrument**, or **Quotation**. The default value is **Organization**.|
|numberOfMatchesPerRecord|No|A number of possible matches to output for each record in the input. The maximum number of possible matches is 5. The default value is 1.|
|raw_output|No|A boolean value set to retrieve a result as a JSON string instead of a data frame. The default value is False which returns a data frame.|

This function returns a tuple containing a result and error string. The result could be a data frame, or JSON string depending on the **raw_output** parameter.

The following code calls the **match** method to match the organization entities with a CSV string.


```python
organization="""
LocalID,Standard Identifier,Name,Country,Street,City,PostalCode,State,Website
1,,Apple,US,"Apple Campus, 1 Infinite Loop",Cupertino,95014,California,
2,,Apple,,,,,,
3,,Teva Pharmaceutical Industries Ltd,IL,,Petah Tikva,,,
4,,Tata Sky,IN,,,,,
5,RIC:IBM.N|Ticker:IBM,,,,,,,
6,Ticker:MSFT,,,,,,,
7,LEI:INR2EJN1ERAN0W5ZP974,,,,,,,
8,Ticker:FB&&Exchange:NSM,,,,,,,
9,Ticker:AAPL&&MIC:XNGS,,,,,,,
"""
output,err = opid.match(organization)
output
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Input_City</th>
      <th>Input_Country</th>
      <th>Input_LocalID</th>
      <th>Input_Name</th>
      <th>Input_PostalCode</th>
      <th>Input_Standard Identifier</th>
      <th>Input_State</th>
      <th>Input_Street</th>
      <th>Match Level</th>
      <th>Match OpenPermID</th>
      <th>Match Ordinal</th>
      <th>Match OrgName</th>
      <th>Match Score</th>
      <th>Original Row Number</th>
      <th>ProcessingStatus</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Cupertino</td>
      <td>US</td>
      <td>1</td>
      <td>Apple</td>
      <td>95014</td>
      <td>NaN</td>
      <td>California</td>
      <td>Apple Campus, 1 Infinite Loop</td>
      <td>Excellent</td>
      <td>https://permid.org/1-4295905573</td>
      <td>1</td>
      <td>Apple Inc</td>
      <td>98%</td>
      <td>2</td>
      <td>OK</td>
    </tr>
    <tr>
      <th>1</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>2</td>
      <td>Apple</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Excellent</td>
      <td>https://permid.org/1-4295905573</td>
      <td>1</td>
      <td>Apple Inc</td>
      <td>92%</td>
      <td>3</td>
      <td>OK</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Petah Tikva</td>
      <td>IL</td>
      <td>3</td>
      <td>Teva Pharmaceutical Industries Ltd</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Excellent</td>
      <td>https://permid.org/1-4295875158</td>
      <td>1</td>
      <td>Teva Pharmaceutical Industries Ltd</td>
      <td>99%</td>
      <td>4</td>
      <td>OK</td>
    </tr>
    <tr>
      <th>3</th>
      <td>NaN</td>
      <td>IN</td>
      <td>4</td>
      <td>Tata Sky</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Excellent</td>
      <td>https://permid.org/1-4297589397</td>
      <td>1</td>
      <td>Tata Sky Ltd</td>
      <td>92%</td>
      <td>5</td>
      <td>OK</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>5</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>RIC:IBM.N|Ticker:IBM</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Excellent</td>
      <td>https://permid.org/1-4295904307</td>
      <td>1</td>
      <td>International Business Machines Corp</td>
      <td>100%</td>
      <td>6</td>
      <td>OK</td>
    </tr>
    <tr>
      <th>5</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>6</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Ticker:MSFT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Excellent</td>
      <td>https://permid.org/1-4295907168</td>
      <td>1</td>
      <td>Microsoft Corp</td>
      <td>100%</td>
      <td>7</td>
      <td>OK</td>
    </tr>
    <tr>
      <th>6</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>7</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>LEI:INR2EJN1ERAN0W5ZP974</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Excellent</td>
      <td>https://permid.org/1-4295907168</td>
      <td>1</td>
      <td>Microsoft Corp</td>
      <td>100%</td>
      <td>8</td>
      <td>OK</td>
    </tr>
    <tr>
      <th>7</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Ticker:FB&amp;&amp;Exchange:NSM</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Excellent</td>
      <td>https://permid.org/1-4297297477</td>
      <td>1</td>
      <td>Facebook Inc</td>
      <td>100%</td>
      <td>9</td>
      <td>OK</td>
    </tr>
    <tr>
      <th>8</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Ticker:AAPL&amp;&amp;MIC:XNGS</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Excellent</td>
      <td>https://permid.org/1-4295905573</td>
      <td>1</td>
      <td>Apple Inc</td>
      <td>100%</td>
      <td>10</td>
      <td>OK</td>
    </tr>
  </tbody>
</table>
</div>



The following code calls the **match** method to match the person entities with a data frame.


```python
import pandas as pd
person = pd.DataFrame(columns = ['LocalID',
                                 'FirstName',
                                 'MiddleName',
                                 'PreferredName',
                                 'LastName',
                                 'CompanyPermID',
                                 'CompanyName',
                                 'NamePrefix',
                                 'NameSuffix']) 
person = person.append(pd.Series(['1','Satya','','','Nadella','','Microsoft Corp','',''], 
                                 index=person.columns),ignore_index=True)
person = person.append(pd.Series(['2','Satya','','','Nadella','4295907168','','',''], 
                                 index=person.columns),ignore_index=True)
person = person.append(pd.Series(['3','Martin','','','Jetter','','International Business Machines Corp','',''], 
                                 index=person.columns),ignore_index=True)
person = person.append(pd.Series(['4','Bill','','','Gates','','Microsoft Corp','',''], 
                                 index=person.columns),ignore_index=True)
output,err = opid.match(person, dataType='Person')
output
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Input_First Name</th>
      <th>Input_Last Name</th>
      <th>Input_LocalID</th>
      <th>Input_OrgName</th>
      <th>Input_OrgOpenPermID</th>
      <th>Match First Name</th>
      <th>Match Last Name</th>
      <th>Match Level</th>
      <th>Match OpenPermID</th>
      <th>Match Ordinal</th>
      <th>Match OrgName</th>
      <th>Match OrgOpenPermID</th>
      <th>Match Score</th>
      <th>Original Row Number</th>
      <th>ProcessingStatus</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Satya</td>
      <td>Nadella</td>
      <td>1</td>
      <td>Microsoft Corp</td>
      <td>NaN</td>
      <td>Satya</td>
      <td>Nadella</td>
      <td>Good</td>
      <td>https://permid.org/1-34413262612</td>
      <td>1</td>
      <td>MICROSOFT CORPORATION</td>
      <td>https://permid.org/1-4295907168</td>
      <td>0.75</td>
      <td>2</td>
      <td>OK</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Satya</td>
      <td>Nadella</td>
      <td>2</td>
      <td>NaN</td>
      <td>https://permid.org/1-4295907168</td>
      <td>Satya</td>
      <td>Nadella</td>
      <td>Excellent</td>
      <td>https://permid.org/1-34413262612</td>
      <td>1</td>
      <td>MICROSOFT CORPORATION</td>
      <td>https://permid.org/1-4295907168</td>
      <td>0.95</td>
      <td>3</td>
      <td>OK</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Martin</td>
      <td>Jetter</td>
      <td>3</td>
      <td>International Business Machines Corp</td>
      <td>NaN</td>
      <td>Martin</td>
      <td>Jetter</td>
      <td>Excellent</td>
      <td>https://permid.org/1-34418338814</td>
      <td>1</td>
      <td>INTERNATIONAL BUSINESS MACHINES CORPORATION</td>
      <td>https://permid.org/1-4295904307</td>
      <td>0.83</td>
      <td>4</td>
      <td>OK</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Bill</td>
      <td>Gates</td>
      <td>4</td>
      <td>Microsoft Corp</td>
      <td>NaN</td>
      <td>William</td>
      <td>Gates</td>
      <td>Good</td>
      <td>https://permid.org/1-34413157709</td>
      <td>1</td>
      <td>MICROSOFT CORPORATION</td>
      <td>https://permid.org/1-4295907168</td>
      <td>0.71</td>
      <td>5</td>
      <td>OK</td>
    </tr>
  </tbody>
</table>
</div>



## 4. Record Matching File

This method is similar to the above **match** method. It is used to match the entity Person, Organization, Instrument, and Quote records with Refinitiv’s PermIDs. However, instead of passing a string or data frame, it accepts a file name that contains records to be matched.

```Python
matchFile(filename,dataType='Organization',numberOfMatchesPerRecord=1,raw_output=False)
```

|Parameter Name|Required|Description|
|--------------|--------|-----------|
|filename|Yes|A filename of the CSV file containing records to be matched. Templates for the CSV files can be downloaded at the [Record Matching](https://permid.org/match) website.|
|dataType|No|The type of entity to search for. Possible values are **Person**, **Organization**, **Instrument**, or **Quotation**. The default value is **Organization**.|
|numberOfMatchesPerRecord|No|A number of possible matches to output for each record in the input. The maximum number of possible matches is 5. The default value is 1.|
|raw_output|No|A boolean value set to retrieve a result as a JSON string instead of a data frame. The default value is False which returns a data frame.|

This function returns a tuple containing a result and error string. The result could be a data frame or JSON string depending on the **raw_output** parameter.

The following code calls the **matchFile** method to match records in an organization CSV file.


```python
output,err = opid.matchFile("Organization_input.csv")
output
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Input_City</th>
      <th>Input_Country</th>
      <th>Input_LocalID</th>
      <th>Input_Name</th>
      <th>Input_PostalCode</th>
      <th>Input_Standard Identifier</th>
      <th>Input_State</th>
      <th>Input_Street</th>
      <th>Match Level</th>
      <th>Match OpenPermID</th>
      <th>Match Ordinal</th>
      <th>Match OrgName</th>
      <th>Match Score</th>
      <th>Original Row Number</th>
      <th>ProcessingStatus</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Cupertino</td>
      <td>US</td>
      <td>1</td>
      <td>Apple</td>
      <td>95014</td>
      <td>NaN</td>
      <td>California</td>
      <td>Apple Campus, 1 Infinite Loop</td>
      <td>Excellent</td>
      <td>https://permid.org/1-4295905573</td>
      <td>1</td>
      <td>Apple Inc</td>
      <td>98%</td>
      <td>2</td>
      <td>OK</td>
    </tr>
    <tr>
      <th>1</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>2</td>
      <td>Apple</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Excellent</td>
      <td>https://permid.org/1-4295905573</td>
      <td>1</td>
      <td>Apple Inc</td>
      <td>92%</td>
      <td>3</td>
      <td>OK</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Petah Tikva</td>
      <td>IL</td>
      <td>3</td>
      <td>Teva Pharmaceutical Industries Ltd</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Excellent</td>
      <td>https://permid.org/1-4295875158</td>
      <td>1</td>
      <td>Teva Pharmaceutical Industries Ltd</td>
      <td>99%</td>
      <td>4</td>
      <td>OK</td>
    </tr>
    <tr>
      <th>3</th>
      <td>NaN</td>
      <td>IN</td>
      <td>4</td>
      <td>Tata Sky</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Excellent</td>
      <td>https://permid.org/1-4297589397</td>
      <td>1</td>
      <td>Tata Sky Ltd</td>
      <td>92%</td>
      <td>5</td>
      <td>OK</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>5</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>RIC:IBM.N|Ticker:IBM</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Excellent</td>
      <td>https://permid.org/1-4295904307</td>
      <td>1</td>
      <td>International Business Machines Corp</td>
      <td>100%</td>
      <td>6</td>
      <td>OK</td>
    </tr>
    <tr>
      <th>5</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>6</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Ticker:MSFT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Excellent</td>
      <td>https://permid.org/1-4295907168</td>
      <td>1</td>
      <td>Microsoft Corp</td>
      <td>100%</td>
      <td>7</td>
      <td>OK</td>
    </tr>
    <tr>
      <th>6</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>7</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>LEI:INR2EJN1ERAN0W5ZP974</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Excellent</td>
      <td>https://permid.org/1-4295907168</td>
      <td>1</td>
      <td>Microsoft Corp</td>
      <td>100%</td>
      <td>8</td>
      <td>OK</td>
    </tr>
    <tr>
      <th>7</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Ticker:FB&amp;&amp;Exchange:NSM</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Excellent</td>
      <td>https://permid.org/1-4297297477</td>
      <td>1</td>
      <td>Facebook Inc</td>
      <td>100%</td>
      <td>9</td>
      <td>OK</td>
    </tr>
    <tr>
      <th>8</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Ticker:AAPL&amp;&amp;MIC:XNGS</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Excellent</td>
      <td>https://permid.org/1-4295905573</td>
      <td>1</td>
      <td>Apple Inc</td>
      <td>100%</td>
      <td>10</td>
      <td>OK</td>
    </tr>
  </tbody>
</table>
</div>



## 5. Intelligent Tagging

This method allows you to tag free-text documents with rich semantic metadata, by identifying and tagging entities, events, and topics.
```
calais(text, language='English', contentType='raw', outputFormat='json')
```
|Parameter Name|Required|Description|
|--------------|--------|-----------|
|text|Yes|Content to be tagged. It could be raw text, html, xml, or pdf|
|language|No|Indicates the language of the input text. Currently, possible values are **English**, **Chinese**, **French**, **German**, **Japanese**, or **Spanish**. The default value is **English**.|
|contentType|No|Indicates the content type of the input text. Possible values are **raw**, **html**, **xml**, or **pdf**. The default value is **raw**.|
|outputFormat|No|Defines the output response format. Possible values are **json**, **rdf**, or **n3**. The default value is **json**.|

This function returns a tuple containing a result and error string. The result could be a JSON, RDF or N-Triples string depending on the **outputFormat** parameter.

The following code calls the **calais** method to tag the raw text.


```python
raw_text ="""
TOKYO (Reuters) - Financial markets reeled on Thursday as stocks dived and oil slumped after U.S. President Donald Trump took the dramatic step of banning travel from Europe to stem the spread of coronavirus, threatening more disruptions to trade and the world economy.

With the pandemic wreaking havoc on daily life of millions worldwide, investors were also disappointed by the lack of broad measures in Trump's plan to fight the pathogen, prompting traders to bet of further aggressive easing by the Federal Reserve.

Euro Stoxx 50 futures STXEc1 plunged 8.3% to their lowest levels since mid-2016. They were last down 6.9% while investors rushed to safe-haven assets from bonds to gold to the yen and the Swiss franc.

U.S. S&P 500 futures ESc1 plummeted as much as 4.9% in Asia and last traded down 3.6%, a day after the S&P 500 .SPX lost 4.89%, leaving the index on the brink of entering bear market territory, defined as a 20% fall from a recent top.

MSCI's broadest gauge of world shares, ACWI .MIWD00000PUS, could follow suit, having fallen 19.2% so far from its record peak hit only a month ago.
"""
output,err = opid.calais(raw_text)
print(output)
```

    {"doc":{"info":{"calaisRequestID":"ff5b9a7f-db7b-5d79-170e-2e595f247f47","id":"http:\/\/id.opencalais.com\/XE-dhoMh83H3a*0o3GJE7A","ontology":"http:\/\/mdaas-virtual-onecalais.int.thomsonreuters.com\/owlschema\/13.0.rc2\/onecalais.owl.allmetadata.xml","docId":"http:\/\/d.opencalais.com\/dochash-1\/f341bd22-d2f1-384c-8d9d-e187ac9897cd","document":"\nTOKYO (Reuters) - Financial markets reeled on Thursday as stocks dived and oil slumped after U.S. President Donald Trump took the dramatic step of banning travel from Europe to stem the spread of coronavirus, threatening more disruptions to trade and the world economy.\n\nWith the pandemic wreaking havoc on daily life of millions worldwide, investors were also disappointed by the lack of broad measures in Trump's plan to fight the pathogen, prompting traders to bet of further aggressive easing by the Federal Reserve.\n\nEuro Stoxx 50 futures STXEc1 plunged 8.3% to their lowest levels since mid-2016. They were last down 6.9% while investors rushed to safe-haven assets from bonds to gold to the yen and the Swiss franc.\n\nU.S. S&P 500 futures ESc1 plummeted as much as 4.9% in Asia and last traded down 3.6%, a day after the S&P 500 .SPX lost 4.89%, leaving the index on the brink of entering bear market territory, defined as a 20% fall from a recent top.\n\nMSCI's broadest gauge of world shares, ACWI .MIWD00000PUS, could follow suit, having fallen 19.2% so far from its record peak hit only a month ago.\n","docTitle":"","docDate":"2020-03-16 10:31:02.218"},"meta":{"contentType":"text\/raw","processingVer":"AllMetadata","serverVersion":"13.0.310:310","stagsVer":"defaultVersion","submissionDate":"2020-03-16 10:31:01.873","submitterCode":"0ca6a864-5659-789d-5f32-f365f695e757","signature":"digestalg-1|meGrr+82Hh4UzVtCvBx06p5itmw=|jNGV5U\/f8fbL1pUdo1rmLH5B8MEu7KENdjHKWdnhXpZJ9U0amimkkQ==","language":"English"}},"http:\/\/d.opencalais.com\/dochash-1\/f341bd22-d2f1-384c-8d9d-e187ac9897cd\/cat\/1":{"_typeGroup":"topics","forenduserdisplay":"false","score":1,"name":"Business_Finance"},"http:\/\/d.opencalais.com\/dochash-1\/f341bd22-d2f1-384c-8d9d-e187ac9897cd\/lid\/DefaultLangId":{"_typeGroup":"language","language":"http:\/\/d.opencalais.com\/lid\/DefaultLangId\/English","forenduserdisplay":"false","permid":"505062"},"http:\/\/d.opencalais.com\/dochash-1\/f341bd22-d2f1-384c-8d9d-e187ac9897cd\/SocialTag\/1":{"_typeGroup":"socialTag","id":"http:\/\/d.opencalais.com\/dochash-1\/f341bd22-d2f1-384c-8d9d-e187ac9897cd\/SocialTag\/1","socialTag":"http:\/\/d.opencalais.com\/genericHasher-1\/88bb4bda-bf2b-36d1-90a7-091347309cb5","forenduserdisplay":"true","name":"Financial markets","importance":"1","originalValue":"Financial markets"},"http:\/\/d.opencalais.com\/dochash-1\/f341bd22-d2f1-384c-8d9d-e187ac9897cd\/SocialTag\/2":{"_typeGroup":"socialTag","id":"http:\/\/d.opencalais.com\/dochash-1\/f341bd22-d2f1-384c-8d9d-e187ac9897cd\/SocialTag\/2","socialTag":"http:\/\/d.opencalais.com\/genericHasher-1\/c68bf480-adf5-319d-b152-dd8a6c45c01b","forenduserdisplay":"true","name":"Coronaviridae","importance":"1","originalValue":"Coronaviridae"},"http:\/\/d.opencalais.com\/dochash-1\/f341bd22-d2f1-384c-8d9d-e187ac9897cd\/SocialTag\/3":{"_typeGroup":"socialTag","id":"http:\/\/d.opencalais.com\/dochash-1\/f341bd22-d2f1-384c-8d9d-e187ac9897cd\/SocialTag\/3","socialTag":"http:\/\/d.opencalais.com\/genericHasher-1\/3179050a-0d51-324c-8d1b-8c88055cc503","forenduserdisplay":"true","name":"Deutsche Börse","importance":"1","originalValue":"Deutsche Börse"},"http:\/\/d.opencalais.com\/dochash-1\/f341bd22-d2f1-384c-8d9d-e187ac9897cd\/SocialTag\/4":{"_typeGroup":"socialTag","id":"http:\/\/d.opencalais.com\/dochash-1\/f341bd22-d2f1-384c-8d9d-e187ac9897cd\/SocialTag\/4","socialTag":"http:\/\/d.opencalais.com\/genericHasher-1\/16cb4002-bfac-34c6-bfbb-28618b682862","forenduserdisplay":"true","name":"STOXX","importance":"2","originalValue":"STOXX"},"http:\/\/d.opencalais.com\/dochash-1\/f341bd22-d2f1-384c-8d9d-e187ac9897cd\/SocialTag\/5":{"_typeGroup":"socialTag","id":"http:\/\/d.opencalais.com\/dochash-1\/f341bd22-d2f1-384c-8d9d-e187ac9897cd\/SocialTag\/5","socialTag":"http:\/\/d.opencalais.com\/genericHasher-1\/b022d2f3-3bf0-3ff3-a5bd-e18e7560cc5a","forenduserdisplay":"true","name":"Coronavirus","importance":"2","originalValue":"Coronavirus"},"http:\/\/d.opencalais.com\/dochash-1\/f341bd22-d2f1-384c-8d9d-e187ac9897cd\/SocialTag\/6":{"_typeGroup":"socialTag","id":"http:\/\/d.opencalais.com\/dochash-1\/f341bd22-d2f1-384c-8d9d-e187ac9897cd\/SocialTag\/6","socialTag":"http:\/\/d.opencalais.com\/genericHasher-1\/6ff8d37f-c1b6-3f0b-be24-0e5bc80c6a1c","forenduserdisplay":"true","name":"Futures contract","importance":"2","originalValue":"Futures contract"},"http:\/\/d.opencalais.com\/dochash-1\/f341bd22-d2f1-384c-8d9d-e187ac9897cd\/SocialTag\/7":{"_typeGroup":"socialTag","id":"http:\/\/d.opencalais.com\/dochash-1\/f341bd22-d2f1-384c-8d9d-e187ac9897cd\/SocialTag\/7","socialTag":"http:\/\/d.opencalais.com\/genericHasher-1\/d3bbac84-e6d7-33a7-8afa-de79ea809156","forenduserdisplay":"true","name":"Swiss franc","importance":"2","originalValue":"Swiss franc"},"http:\/\/d.opencalais.com\/dochash-1\/f341bd22-d2f1-384c-8d9d-e187ac9897cd\/SocialTag\/8":{"_typeGroup":"socialTag","id":"http:\/\/d.opencalais.com\/dochash-1\/f341bd22-d2f1-384c-8d9d-e187ac9897cd\/SocialTag\/8","socialTag":"http:\/\/d.opencalais.com\/genericHasher-1\/aec539f0-27cf-3a0f-9672-b5109d842e67","forenduserdisplay":"true","name":"SPX","importance":"2","originalValue":"SPX"},"http:\/\/d.opencalais.com\/dochash-1\/f341bd22-d2f1-384c-8d9d-e187ac9897cd\/SocialTag\/9":{"_typeGroup":"socialTag","id":"http:\/\/d.opencalais.com\/dochash-1\/f341bd22-d2f1-384c-8d9d-e187ac9897cd\/SocialTag\/9","socialTag":"http:\/\/d.opencalais.com\/genericHasher-1\/0df0257c-f6c4-3a42-888f-488f5677e510","forenduserdisplay":"true","name":"S&P 500 Index","importance":"2","originalValue":"S&P 500 Index"},"http:\/\/d.opencalais.com\/dochash-1\/f341bd22-d2f1-384c-8d9d-e187ac9897cd\/SocialTag\/10":{"_typeGroup":"socialTag","id":"http:\/\/d.opencalais.com\/dochash-1\/f341bd22-d2f1-384c-8d9d-e187ac9897cd\/SocialTag\/10","socialTag":"http:\/\/d.opencalais.com\/genericHasher-1\/5cf7d5dd-2310-3a18-b0c5-cd3325e20de1","forenduserdisplay":"true","name":"Euro Stoxx 50","importance":"2","originalValue":"Euro Stoxx 50"},"http:\/\/d.opencalais.com\/dochash-1\/f341bd22-d2f1-384c-8d9d-e187ac9897cd\/SocialTag\/11":{"_typeGroup":"socialTag","id":"http:\/\/d.opencalais.com\/dochash-1\/f341bd22-d2f1-384c-8d9d-e187ac9897cd\/SocialTag\/11","socialTag":"http:\/\/d.opencalais.com\/genericHasher-1\/412d2c87-2ead-391a-ae44-6add965d8ada","forenduserdisplay":"true","name":"Economy","importance":"2","originalValue":"Economy"},"http:\/\/d.opencalais.com\/dochash-1\/f341bd22-d2f1-384c-8d9d-e187ac9897cd\/ComponentVersions":{"_typeGroup":"versions","version":["DIY-Categorization-fnr-TRCS-prcng_3:20171117095241:20171117095241","DIY-Categorization-news_discovery-TRCS-t1cr_tier_1_capital_ratio_____T1CR-News:20190709091033:20190709091033","DIY-Categorization-record_tagging-TRCS-smuggling_wc_christ___SMGLNG-News:20200219093707:20200219093707","DIY-Categorization-forest_spirit-TRCS-policy_forced_labor_pvm___M_262-News:20200114164912:20200114164912","DIY-Categorization-news_discovery-TRCS-uscru___USCRU-News:20190211131330:20190211131330","DIY-Categorization-fnr-TRCS-curint-News:20180221133342:20180221133342","DIY-Categorization-fnr-TRCS-capx:20170627191152:20170627191152","DIY-Categorization-fnr-TRCS-job_cuts__rsh_nithish1___LAYOFS-Research:20180703093637:20180703093637","DIY-Categorization-news_discovery-TRCS-syria3___SY-News:20191125141249:20191125141249","DIY-Categorization-fnr-TRCS-industry_consolidation_sai_rsh___INDMRG-Research:20180703113905:20180703113905","DIY-Categorization-news_discovery-TRCS-au_pritha___AU-News:20191127143728:20191127143728","DIY-Categorization-news_discovery-TRCS-ec_ecuador___EC-News:20190809110954:20190809110954","DIY-Categorization-fnr-TRCS-abs-News:20180321093043:20180321093043","DIY-Categorization-news_discovery-TRCS-meast_kairav___MEAST-News:20200225154127:20200225154127","DIY-Categorization-forest_spirit-TRCS-stakeholder_engagement_prashant___M_23H-News:20191227054607:20191227054607","DIY-Categorization-risk-TRCS-enforcement_jithin___ENFORC-News:20191017130800:20191017130800","DIY-Categorization-news_discovery-TRCS-ky1___KY-News:20190603132706:20190603132706","DIY-Categorization-news_discovery-TRCS-latcru___LATCRU-News:20190208140753:20190208140753","DIY-Categorization-news_discovery-TRCS-sigpol___SIGPOL-News:20190925135800:20190925135800","DIY-Categorization-news_discovery-TRCS-gambia___GM-News:20190807095237:20190807095237","DIY-Categorization-fnr-TRCS-meal___MEAL-News:20180716080238:20180716080238","DIY-Categorization-news_discovery-TRCS-un1___UN1-News:20190315052632:20190315052632","DIY-Categorization-forest_spirit-TRCS-policy_executive_retention___M_22W-News:20191230061950:20191230061950","DIY-Categorization-record_tagging-TRCS-violent_crimes_2___VCRIM1-News:20200219093851:20200219093851","DIY-Categorization-fnr-TRCS-cstcut_new-News:20180301140156:20180301140156","DIY-Categorization-fnr-TRCS-margnu:20171117095324:20171117095324","DIY-Categorization-news_discovery-TRCS-cbru___CBRU-News:20190117150804:20190117150804","DIY-Categorization-news_discovery-TRCS-south_africa___ZA-News:20191104123949:20191104123949","DIY-Categorization-fnr-TRCS-ta1___TA1-News:20180716090345:20180716090345","DIY-Categorization-news_discovery-TRCS-ilei___ILEI-News:20190527144111:20190527144111","DIY-Categorization-fnr-TRCS-pgm___PGM-News:20180508065449:20180508065449","DIY-Categorization-news_discovery-TRCS-llr_kairav___LLR-News:20190710112025:20190710112025","DIY-Categorization-fnr-TRCS-board1___BOARD1-News:20180622140843:20180622140843","DIY-Categorization-news_discovery-TRCS-fomc3___FOMC-News:20190508103144:20190508103144","DIY-Categorization-research-TRCS-financial_crime_rsch_avadhut___SCAM1-Research:20181221074756:20181221074756","DIY-Categorization-news_discovery-TRCS-strtgy___STRTGY-News:20181226124912:20181226124912","DIY-Categorization-news_discovery-TRCS-aaesin___AAESIN-News:20190423093017:20190423093017","DIY-Categorization-news_discovery-TRCS-md__moldova___MD-News:20191015092457:20191015092457","DIY-Categorization-news_discovery-TRCS-arti___ARTI-News:20181221144725:20181221144725","DIY-Categorization-research-TRCS-target_price_increase_rsch___AATPIN-Research:20181019144530:20181019144530","DIY-Categorization-forest_spirit-TRCS-policy_child_labor_pvm___M_261-News:20200114164324:20200114164324","Deals Index:202003160939:202003160939","DIY-Categorization-esg-TRCS-intellectual_property___M_21S-News:20191119075828:20191119075828","DIY-Categorization-fnr-TRCS-risk_exonerated___EXNRT-News:20180816152710:20180816152710","DIY-Categorization-news_discovery-TRCS-trnspt___TRNSPT-News:20181109113853:20181109113853","DIY-Categorization-forest_spirit-TRCS-iso_9000___M_26N-News:20200108083540:20200108083540","DIY-Categorization-news_discovery-TRCS-bru___BRU-News:20181206082606:20181206082606","DIY-Categorization-risk-TRCS-sanctions_and_restrictions___SNCTN1-News:20180921123345:20180921123345","DIY-Categorization-fnr-TRCS-cttl___CTTL-News:20180424161004:20180424161004","DIY-Categorization-fnr-TRCS-anti_competitive_behavior_rsch_2___ACB-Research:20190503023148:20190503023148","DIY-Categorization-news_discovery-TRCS-wrm___WRM-News:20190515113813:20190515113813","DIY-Categorization-risk-TRCS-product_and_services_jithin___PANS-News:20190527154529:20190527154529","DIY-Categorization-fnr-TRCS-indx_stsale:20170323110805:20170323110805","DIY-Categorization-forest_spirit-TRCS-integrated_strategy_in_md_a___M_23G-News:20191227173156:20191227173156","DIY-Categorization-research-TRCS-antitrust_regulation_rsch_avadhu___MONOP-Research:20190417124739:20190417124739","DIY-Categorization-news_discovery-TRCS-mg_pritha___MG-News:20191017073123:20191017073123","DIY-Categorization-news_discovery-TRCS-cbbr___CBBR-News:20190301062835:20190301062835","DIY-Categorization-news_discovery-TRCS-myanmar___MM-News:20190724075745:20190724075745","DIY-Categorization-news_discovery-TRCS-3dpr___3DPR-News:20200309120111:20200309120111","DIY-Categorization-news_discovery-TRCS-oils___OILS-News:20181115145002:20181115145002","DIY-Categorization-news_discovery-TRCS-volgro___VOLGRO-News:20190709090605:20190709090605","DIY-Categorization-news_discovery-TRCS-sigcom___SIGCOM-News:20190306104121:20190306104121","DIY-Categorization-news_discovery-TRCS-unhcr_pritha___UNHCR-News:20200123143439:20200123143439","DIY-Categorization-news_discovery-TRCS-cbie___CBIE-News:20190903104626:20190903104626","DIY-Categorization-fnr-TRCS-margin_pressure_rsch_joel___MARGND-Research:20180518140643:20180518140643","DIY-Categorization-forest_spirit-TRCS-employees_community_work___M_25B-News:20200106075232:20200106075232","DIY-Categorization-fnr-TRCS-comptn:20170621141739:20170621141739","DIY-Categorization-risk-TRCS-theft_and_embezzlement___THFT-News:20191108114421:20191108114421","DIY-Categorization-news_discovery-TRCS-sste___SSTE-News:20181107102633:20181107102633","DIY-Categorization-fnr-TRCS-childexploitation___KIDEXP-News:20180827070834:20180827070834","DIY-Categorization-fnr-TRCS-pricing_power_rsch_joel_new___PRCNG-Research:20180730092938:20180730092938","DIY-Categorization-news_discovery-TRCS-mo_kairav___MO-News:20200114113724:20200114113724","DIY-Categorization-risk-TRCS-crime___CRIM2-News:20190311142934:20190311142934","DIY-Categorization-news_discovery-TRCS-ee_estonia_pritha___EE-News:20190524094417:20190524094417","DIY-Categorization-record_tagging-TRCS-cease_and_desist_2___CEDEO-News:20200225080708:20200225080708","DIY-Categorization-news_discovery-TRCS-osce___OSCE-News:20190204122523:20190204122523","DIY-Categorization-fnr-TRCS-class_actions_rsch_joel___CLASS-Research:20180711122108:20180711122108","DIY-Categorization-forest_spirit-TRCS-policy_customer_health_safety___M_26S-News:20200128173600:20200128173600","DIY-Categorization-fnr-TRCS-plcy___PLCY-News:20180416161629:20180416161629","DIY-Categorization-fnr-TRCS-mog___MOG-News:20180816064905:20180816064905","DIY-Categorization-fnr-TRCS-financial_mismanagement3___FINMI-News:20180706115709:20180706115709","DIY-Categorization-news_discovery-TRCS-rghtis___RGHTIS-News:20190111151955:20190111151955","DIY-Categorization-news_discovery-TRCS-crim___crime-News:20181214094457:20181214094457","DIY-Categorization-news_discovery-TRCS-bmxo___BMXO-News:20190209125942:20190209125942","DIY-Categorization-forest_spirit-TRCS-voting_cap_deepa___M_238-News:20191222104411:20191222104411","DIY-Categorization-forest_spirit-TRCS-gambling___M_26K-News:20200208042129:20200208042129","DIY-Categorization-risk-TRCS-it_governance___ITGOV-News:20181207151943:20181207151943","DIY-Categorization-news_discovery-TRCS-net_earned_premium___NEP-News:20190704124020:20190704124020","DIY-Categorization-news_discovery-TRCS-woo___WOO-News:20180904124827:20180904124827","DIY-Categorization-fnr-TRCS-regs_buyb_new:20170509091609:20170509091609","DIY-Categorization-record_tagging-TRCS-pharmaceutical_trafficking___TRAPHA-News:20200225081707:20200225081707","DIY-Categorization-fnr-TRCS-int___INT-News:20180326113056:20180326113056","DIY-Categorization-news_discovery-TRCS-nr__nauru___NR-News:20190912073738:20190912073738","DIY-Categorization-record_tagging-TRCS-insider_trading___INTRD1-News:20200225081305:20200225081305","DIY-Categorization-forest_spirit-TRCS-gmo_products_pvm___M_26L-News:20200212164654:20200212164654","DIY-Categorization-fnr-TRCS-risk_infring_ola___M_1TC-News:20180605101139:20180605101139","DIY-Categorization-fnr-TRCS-pwrl___PWRL-News:20180716080357:20180716080357","config-physicalAssets-powerStations:994:994","DIY-Categorization-news_discovery-TRCS-cprod___CPROD-News:20181026094426:20181026094426","DIY-Categorization-news_discovery-TRCS-bioeth___BIOETH-News:20190118102008:20190118102008","DIY-Categorization-news_discovery-TRCS-eafr_pritha___EAFR-News:20200225125359:20200225125359","DIY-Categorization-news_discovery-TRCS-adr___ADR-News:20190311115422:20190311115422","DIY-Categorization-news_discovery-TRCS-ko_pritha___KO-News:20190807140921:20190807140921","DIY-Categorization-news_discovery-TRCS-rtlap___RTLAP-News:20200103144602:20200103144602","DIY-Categorization-news_discovery-TRCS-gabon___GA-News:20190909110311:20190909110311","DIY-Categorization-risk-TRCS-brand_infringement___BRINF-News:20180924064629:20180924064629","DIY-Categorization-news_discovery-TRCS-antigua_and_barbuda___AG-News:20191107094622:20191107094622","DIY-Categorization-news_discovery-TRCS-vietnam___VN-News:20190604145001:20190604145001","DIY-Categorization-news_discovery-TRCS-cancru___CANCRU-News:20190318092300:20190318092300","DIY-Categorization-forest_spirit-TRCS-management_training___M_274-News:20200107075812:20200107075812","DIY-Categorization-news_discovery-TRCS-in_kairav___IN-News:20200106120234:20200106120234","DIY-Categorization-fnr-TRCS-invtr___INVTR-News:20181016084911:20181016084911","DIY-Categorization-fnr-TRCS-same_store_sales_sai_rch___STSALE-Research:20180508132425:20180508132425","DIY-Categorization-news_discovery-TRCS-bahrain___BH-News:20190923080055:20190923080055","DIY-Categorization-record_tagging-TRCS-extortion_olivia_hess___XTRTN-News:20200225080201:20200225080201","DIY-Categorization-fnr-TRCS-revenue_drivers_ram_rch___REVDRV-Research:20180406095446:20180406095446","DIY-Categorization-news_discovery-TRCS-chile___CL-News:20191126091720:20191126091720","NextTags:2.1.27-legacy-trit-release-master:27","DIY-Categorization-news_discovery-TRCS-drv___DRV-News:20181024124551:20181024124551","DIY-Categorization-news_discovery-TRCS-cbeg___CBEG-News:20190603094901:20190603094901","DIY-Categorization-record_tagging-TRCS-cybercrime_2___HACK1-News:20200225080322:20200225080322","DIY-Categorization-forest_spirit-TRCS-fundamental_human_rights_ilo_un___M_260-News:20200129131246:20200129131246","DIY-Categorization-fnr-TRCS-governance_rsh_nithishgowda___ESGGOV-Research:20180713103456:20180713103456","DIY-Categorization-news_discovery-TRCS-kmove___KMOVE-News:20181226150143:20181226150143","DIY-Categorization-news_discovery-TRCS-trf___TRF-News:20181220141428:20181220141428","DIY-Categorization-forest_spirit-TRCS-minimum_number_of_shares_to_vote___M_230-News:20191222113715:20191222113715","DIY-Categorization-news_discovery-TRCS-ask___ASK-News:20190301112518:20190301112518","DIY-Categorization-fnr-TRCS-sl1___SL1-News:20180719103649:20180719103649","DIY-Categorization-record_tagging-TRCS-illegal_immigration___ILLIM1-News:20200225081646:20200225081646","DIY-Categorization-fnr-TRCS-accounting_issues_rsch_joel___ACCI-Research:20180405085125:20180405085125","DIY-Categorization-news_discovery-TRCS-wnd___WND-News:20180911104725:20180911104725","DIY-Categorization-fnr-TRCS-pltry___PLTRY-News:20180502073135:20180502073135","DIY-Categorization-fnr-TRCS-infl-News:20180206095852:20180206095852","DIY-Categorization-news_discovery-TRCS-suk___SUK-News:20180816065714:20180816065714","DIY-Categorization-news_discovery-TRCS-asean1___ASEAN1-News:20190425095129:20190425095129","DIY-Categorization-fnr-TRCS-res_seasonality___SEASNS-Research:20180613094823:20180613094823","DIY-Categorization-news_discovery-TRCS-usc___USC-News:20181109152756:20181109152756","DIY-Categorization-forest_spirit-TRCS-staff_transport_impact_reduction___M_23Q-News:20200316070550:20200316070550","DIY-Categorization-news_discovery-TRCS-ndfw___NDFW-News:20180904085433:20180904085433","DIY-Categorization-news_discovery-TRCS-albania___AL-News:20190916100517:20190916100517","DIY-Categorization-research-TRCS-target_price_changes_rsch_avadhu___AATPCH-Research:20181029101517:20181029101517","DIY-Categorization-news_discovery-TRCS-bioms___BIOMS-News:20190118112336:20190118112336","DIY-Categorization-fnr-TRCS-newjbs:20180124163802:20180124163802","DIY-Categorization-news_discovery-TRCS-aatpin___AATPIN-News:20190422092106:20190422092106","DIY-Categorization-news_discovery-TRCS-ivory_coast___CI-News:20191004082922:20191004082922","DIY-Categorization-risk-TRCS-charged___CHRG-News:20181024124212:20181024124212","DIY-Categorization-news_discovery-TRCS-denmark___DK-News:20190805112614:20190805112614","DIY-Categorization-record_tagging-TRCS-deported_or_exiled___DEPORT-News:20200225081442:20200225081442","DIY-Categorization-news_discovery-TRCS-livestock___U_D-News:20180920124531:20180920124531","DIY-Categorization-news_discovery-TRCS-brly___BRLY-News:20180904124737:20180904124737","DIY-Categorization-news_discovery-TRCS-rdft___RDFT-News:20180928131757:20180928131757","DIY-Categorization-fnr-TRCS-intellectual_property_sai_rsh___IPROP-Research:20180723092831:20180723092831","DIY-Categorization-risk-TRCS-terror_related_matters___TERRO-News:20190625122840:20190625122840","DIY-Categorization-news_discovery-TRCS-bq1___BQ1-News:20181003112150:20181003112150","DIY-Categorization-news_discovery-TRCS-greece___GR-News:20190712095815:20190712095815","DIY-Categorization-news_discovery-TRCS-dass_kairav___DASS-News:20200218122557:20200218122557","DIY-Categorization-forest_spirit-TRCS-animal_testing_reduction_initiat___M_26E-News:20200213141128:20200213141128","DIY-Categorization-fnr-TRCS-dis:20170418132337:20170418132337","DIY-Categorization-news_discovery-TRCS-eg_kairav___EG-News:20190603074327:20190603074327","DIY-Categorization-news_discovery-TRCS-paxm___PAXM-News:20190826094407:20190826094407","DIY-Categorization-fnr-TRCS-revdrv2-News:20180202125843:20180202125843","DIY-Categorization-news_discovery-TRCS-vote___VOTE-News:20181231122633:20181231122633","DIY-Categorization-fnr-TRCS-scalability_rsch_joel___SCALE-Research:20180716095204:20180716095204","DIY-Categorization-record_tagging-TRCS-crimes_against_the_state___STCRIM-News:20200225081337:20200225081337","DIY-Categorization-news_discovery-TRCS-antarctica___AQ-News:20200203151159:20200203151159","DIY-Categorization-fnr-TRCS-splitb_r1:20170329082516:20170329082516","DIY-Categorization-research-TRCS-shares_split_avadhut___SPLITB-Research:20180919121636:20180919121636","DIY-Categorization-news_discovery-TRCS-gwp___GWP-News:20190305151803:20190305151803","DIY-Categorization-tms-TRCS-dummy___C_3U-News:20190606073736:20190606073736","DIY-Categorization-forest_spirit-TRCS-flexible_working_schemes___M_25L-News:20200108094452:20200108094452","DIY-Categorization-forest_spirit-TRCS-equal_voting_rights___M_231-News:20191216114415:20191216114415","DIY-Categorization-news_discovery-TRCS-lesotho___LS-News:20190716075755:20190716075755","DIY-Categorization-fnr-TRCS-alu-News:20180314130406:20180314130406","DIY-Categorization-news_discovery-TRCS-cv_pritha___CV-News:20191018070622:20191018070622","DIY-Categorization-news_discovery-TRCS-law___LAW-News:20181129113417:20181129113417","DIY-Categorization-news_discovery-TRCS-cont___CONT-News:20180914094654:20180914094654","DIY-Categorization-news_discovery-TRCS-lu_kairav_final___LU-News:20191118092729:20191118092729","DIY-Categorization-forest_spirit-TRCS-veto_power_or_golden_share_prash___M_237-News:20191222112426:20191222112426","DIY-Categorization-fnr-TRCS-revs___REVS-News:20180801145429:20180801145429","DIY-Categorization-esg-TRCS-management_departures___M_223-News:20191210055550:20191210055550","DIY-Categorization-fnr-TRCS-ils___ILS-News:20180419135358:20180419135358","DIY-Categorization-news_discovery-TRCS-nepal___NP-News:20190624094524:20190624094524","DIY-Categorization-risk-TRCS-child_exploitation_adela___KIDEXP-News:20190502082115:20190502082115","DIY-Categorization-news_discovery-TRCS-brxt___BRXT-News:20181213114302:20181213114302","DIY-Categorization-news_discovery-TRCS-ca_pritha___CA-News:20190712115306:20190712115306","DIY-Categorization-fnr-TRCS-vio___VIO-News:20180718135331:20180718135331","DIY-Categorization-fnr-TRCS-esgenv_2-News:20180202131300:20180202131300","DIY-Categorization-fnr-TRCS-loa___LOA-News:20180409071645:20180409071645","DIY-Categorization-record_tagging-TRCS-tax_and_customs_violation_final___TAXCUV-News:20200219093725:20200219093725","DIY-Categorization-news_discovery-TRCS-inboe_kairav___INBOE-News:20190906132536:20190906132536","DIY-Categorization-news_discovery-TRCS-iq_iraq___IQ-News:20190527071226:20190527071226","DIY-Categorization-fnr-TRCS-margnd_2:20171117095121:20171117095121","DIY-Categorization-news_discovery-TRCS-es1___ES-News:20190624112328:20190624112328","DIY-Categorization-fnr-TRCS-soy1___SOY1-News:20180521141509:20180521141509","DIY-Categorization-fnr-TRCS-wea___WEA-News:20180612095409:20180612095409","DIY-Categorization-news_discovery-TRCS-cbke___CBKE-News:20190610080154:20190610080154","DIY-Categorization-news_discovery-TRCS-laos___LA-News:20191023142821:20191023142821","DIY-Categorization-forest_spirit-TRCS-policy_emissions_reduction___M_23P-News:20200311093136:20200311093136","DIY-Categorization-record_tagging-TRCS-narcotics_trafficking___NARTRA-News:20200225075727:20200225075727","DIY-Categorization-news_discovery-TRCS-slnm___U_43-News:20190430071259:20190430071259","DIY-Categorization-news_discovery-TRCS-auscru___AUSCRU-News:20190401102902:20190401102902","DIY-Categorization-record_tagging-TRCS-counterfeiting_2___FAKED-News:20200225080220:20200225080220","DIY-Categorization-news_discovery-TRCS-nvship___NVSHIP-News:20190328152758:20190328152758","DIY-Categorization-fnr-TRCS-conspiracy_and_collusion_clint___CRIM-News:20180802134019:20180802134019","DIY-Categorization-news_discovery-TRCS-norbk___NORBK-News:20190205074045:20190205074045","DIY-Categorization-fnr-TRCS-change_of_cfo_joel_res-Research:20180315092841:20180315092841","DIY-Categorization-news_discovery-TRCS-trdemb___TRDEMB-News:20190718100549:20190718100549","DIY-Categorization-news_discovery-TRCS-porpro___PORPRO-News:20190109081113:20190109081113","DIY-Categorization-forest_spirit-TRCS-supply_chain_health_safety_train___M_25Y-News:20200227115028:20200227115028","config-sca-DataPackage:50:50","DIY-Categorization-record_tagging-TRCS-absconder_or_fugitive_new___ABFU-News:20200225080629:20200225080629","DIY-Categorization-forest_spirit-TRCS-policy_energy_efficiency___M_24S-News:20200304153503:20200304153503","DIY-Categorization-forest_spirit-TRCS-policy_career_development_p___M_275-News:20200108165428:20200108165428","DIY-Categorization-fnr-TRCS-tracc___TRACC-News:20180409095318:20180409095318","DIY-Categorization-forest_spirit-TRCS-minimum_voting_rights___M_230-News:20191216101447:20191216101447","DIY-Categorization-news_discovery-TRCS-ethiopia___ET-News:20190806122305:20190806122305","DIY-Categorization-news_discovery-TRCS-kp_north_korea_preethi___KP-News:20190524071524:20190524071524","DIY-Categorization-fnr-TRCS-mergers___acquisitions__joel_rch-Research:20180322100020:20180322100020","DIY-Categorization-esg-TRCS-wages_workingcondition_contro___M_225-News:20191217110217:20191217110217","DIY-Categorization-news_discovery-TRCS-norway_kairav___NO-News:20190514065724:20190514065724","DIY-Categorization-fnr-TRCS-fraud_ola___DCPTN-News:20180612125416:20180612125416","config-companyNe:1190:1190","DIY-Categorization-news_discovery-TRCS-imm___IMM-News:20190103090746:20190103090746","DIY-Categorization-news_discovery-TRCS-votp___VOTP-News:20181031133856:20181031133856","DIY-Categorization-fnr-TRCS-acb:20170502102348:20170502102348","DIY-Categorization-news_discovery-TRCS-potus___POTUS-News:20181024142603:20181024142603","DIY-Categorization-news_discovery-TRCS-guadeloupe___GP-News:20190606133411:20190606133411","DIY-Categorization-forest_spirit-TRCS-policy_fair_competition___M_25H-News:20200122140234:20200122140234","DIY-Categorization-news_discovery-TRCS-rel___REL-News:20190115070946:20190115070946","DIY-Categorization-news_discovery-TRCS-peer_to_peer_economy___P2PP-News:20200225095057:20200225095057","DIY-Categorization-forest_spirit-TRCS-nuclear_5_revenues_pvm___M_26Q-News:20200214133108:20200214133108","DIY-Categorization-fnr-TRCS-lpg1___LPG1-News:20180718112244:20180718112244","DIY-Categorization-news_discovery-TRCS-imf___IMF-News:20190423080226:20190423080226","DIY-Categorization-news_discovery-TRCS-me_kairav___ME-News:20190617125013:20190617125013","DIY-Categorization-news_discovery-TRCS-drone_pritha___DRONE-News:20191227094232:20191227094232","DIY-Categorization-news_discovery-TRCS-pwr___PWR-News:20180912090738:20180912090738","DIY-Categorization-research-TRCS-estimates_decrease_rsch_avadhut___AAESDE-Research:20181128073554:20181128073554","DIY-Categorization-news_discovery-TRCS-boe___BOE-News:20190318091704:20190318091704","DIY-Categorization-research-TRCS-secondary_share_offerings_nithis___SISU-Research:20180917102822:20180917102822","DIY-Categorization-news_discovery-TRCS-resr___RESR-News:20190423095518:20190423095518","DIY-Categorization-fnr-TRCS-stsale_new-News:20180302131833:20180302131833","DIY-Categorization-news_discovery-TRCS-ps1___PS1-News:20181211075602:20181211075602","DIY-Categorization-news_discovery-TRCS-cerem___CEREM-News:20190130104229:20190130104229","DIY-Categorization-news_discovery-TRCS-spac___SPAC-News:20190305070151:20190305070151","DIY-Categorization-news_discovery-TRCS-biofuels___BIOF-News:20180813100403:20180813100403","DIY-Categorization-news_discovery-TRCS-hinduism___HIND-News:20181122143018:20181122143018","DIY-Categorization-news_discovery-TRCS-cd__congo__drc____CD-News:20190909073527:20190909073527","DIY-Categorization-news_discovery-TRCS-gd_pritha___GD-News:20190805134049:20190805134049","DIY-Categorization-news_discovery-TRCS-kenya___KE-News:20190606093619:20190606093619","DIY-Categorization-fnr-TRCS-scrp___SCRP-News:20180514114027:20180514114027","DIY-Categorization-news_discovery-TRCS-tajikistan___TJ-News:20190726112031:20190726112031","DIY-Categorization-news_discovery-TRCS-gu_kairav___GU-News:20190920073306:20190920073306","DIY-Categorization-news_discovery-TRCS-boisl___BOISL-News:20190514090930:20190514090930","DIY-Categorization-news_discovery-TRCS-rigrat___RIGRAT-News:20190820122106:20190820122106","DIY-Categorization-research-TRCS-rating_changes___AARCHG-Research:20181001105833:20181001105833","DIY-Categorization-fnr-TRCS-natural_gas___NGS-News:20190517095824:20190517095824","DIY-Categorization-forest_spirit-TRCS-succession_plan_for_executives_p___M_22J-News:20191216103807:20191216103807","DIY-Categorization-news_discovery-TRCS-biodsl___BIODSL-News:20190107143613:20190107143613","DIY-Categorization-news_discovery-TRCS-rba___RBA-News:20190306101113:20190306101113","DIY-Categorization-forest_spirit-TRCS-employees_health_safety_team_pvm___M_25Q-News:20200113143630:20200113143630","DIY-Categorization-fnr-TRCS-gilesriskaccused___ACCU-News:20180706100250:20180706100250","DIY-Categorization-news_discovery-TRCS-hnbk___HNBK-News:20190308121103:20190308121103","DIY-Categorization-fnr-TRCS-hosal_jayasimha-News:20180320092027:20180320092027","DIY-Categorization-fnr-TRCS-levrge:20170621122109:20170621122109","DIY-Categorization-fnr-TRCS-significance_commodities_new___C_3X-News:20190313134116:20190313134116","DIY-Categorization-news_discovery-TRCS-arpu___ARPU-News:20191203093005:20191203093005","DIY-Categorization-news_discovery-TRCS-central_eastern_europe___CEEU-News:20200312144619:20200312144619","DIY-Categorization-news_discovery-TRCS-rtm___RTM-News:20190122123012:20190122123012","DIY-Categorization-esg-TRCS-fda_warning_letter___M_227-News:20191217032931:20191217032931","DIY-Categorization-news_discovery-TRCS-gy_kairav___GY-News:20190802121536:20190802121536","DIY-Categorization-news_discovery-TRCS-jordan___JO-News:20191023132355:20191023132355","DIY-Categorization-record_tagging-TRCS-human_trafficking_wc_renata_1___TRAHU-News:20200219092812:20200219092812","DIY-Categorization-news_discovery-TRCS-burkina_faso___BF-News:20191111131537:20191111131537","DIY-Categorization-fnr-TRCS-nkl___NKL-News:20180605132818:20180605132818","DIY-Categorization-news_discovery-TRCS-aarchg___AARCHG-News:20190426134933:20190426134933","DIY-Categorization-fnr-TRCS-hedge_list1:20170625000000:20170625000000","DIY-Categorization-fnr-TRCS-nucpwr___NUCPWR-News:20180627115243:20180627115243","DIY-Categorization-fnr-TRCS-orj___ORJ-News:20180716104900:20180716104900","DIY-Categorization-news_discovery-TRCS-seychelles___SC-News:20190905091236:20190905091236","DIY-Categorization-research-TRCS-results_beat_rsch_avadhu___RSBEAT-Research:20190131112119:20190131112119","DIY-Categorization-news_discovery-TRCS-cblt___CBLT-News:20190201104329:20190201104329","DIY-Categorization-news_discovery-TRCS-supd___SUPD-News:20180904072615:20180904072615","DIY-Categorization-news_discovery-TRCS-npa___NPA-News:20190703062853:20190703062853","DIY-Categorization-news_discovery-TRCS-stat___STAT-News:20190423080607:20190423080607","DIY-Categorization-fnr-TRCS-corporate_inventory_rama_rch___INVTRY-Research:20180329065542:20180329065542","DIY-Categorization-fnr-TRCS-risk_trafficking___TRAF-News:20180725141319:20180725141319","DIY-Categorization-news_discovery-TRCS-mexico___MX-News:20190802110710:20190802110710","DIY-Categorization-risk-TRCS-country_risk_2___CTRYR-News:20181126100355:20181126100355","DIY-Categorization-fnr-TRCS-corporate_taxation_rsch_joel_new___CRPTAX-Research:20180621102341:20180621102341","DIY-Categorization-fnr-TRCS-environmental_degradation___ENDEG1-News:20180705142320:20180705142320","DIY-Categorization-news_discovery-TRCS-mifid2___MIFID2-News:20181105112700:20181105112700","DIY-Categorization-news_discovery-TRCS-pressr___PRESSR-News:20190220122738:20190220122738","DIY-Categorization-news_discovery-TRCS-aardw___AARDW-News:20190430113353:20190430113353","DIY-Categorization-news_discovery-TRCS-hrep___HREP-News:20181226134140:20181226134140","DIY-Categorization-news_discovery-TRCS-belize___BZ-News:20190906095632:20190906095632","DIY-Categorization-risk-TRCS-abuse_of_power5___ABUPO-News:20180912071332:20180912071332","DIY-Categorization-record_tagging-TRCS-exploitation_of_children___KIDEX1-News:20200225080820:20200225080820","DIY-Categorization-record_tagging-TRCS-licence_revocation___LICRE-News:20200225081550:20200225081550","DIY-Categorization-news_discovery-TRCS-bt_kairav___BT-News:20190906102205:20190906102205","DIY-Categorization-forest_spirit-TRCS-majority_requirements___M_22N-News:20191222111349:20191222111349","DIY-Categorization-news_discovery-TRCS-rasm___RASM-News:20190311150437:20190311150437","DIY-Categorization-news_discovery-TRCS-nvprod___NVPROD-News:20190327132846:20190327132846","DIY-Categorization-fnr-TRCS-ng1___NG1-News:20180716090408:20180716090408","DIY-Categorization-news_discovery-TRCS-angola___AO-News:20190918070533:20190918070533","DIY-Categorization-news_discovery-TRCS-seacru___SEACRU-News:20190402125306:20190402125306","DIY-Categorization-news_discovery-TRCS-incfe_pritha___INCFE-News:20190723074242:20190723074242","DIY-Categorization-news_discovery-TRCS-fed___FED-News:20190204115224:20190204115224","DIY-Categorization-news_discovery-TRCS-prod___PROD-News:20181026150152:20181026150152","DIY-Categorization-fnr-TRCS-soil___SOIL-News:20180601140445:20180601140445","DIY-Categorization-news_discovery-TRCS-oman___OM-News:20190805102056:20190805102056","DIY-Categorization-fnr-TRCS-esggov-News:20180301091656:20180301091656","DIY-Categorization-news_discovery-TRCS-automotive_technology___AMVTEC-News:20200117142154:20200117142154","DIY-Categorization-news_discovery-TRCS-cannab___CANNAB-News:20190117103813:20190117103813","DIY-Categorization-fnr-TRCS-kndp___KDNP-News:20180704141821:20180704141821","DIY-Categorization-news_discovery-TRCS-cdv___CDV-News:20180925091850:20180925091850","DIY-Categorization-fnr-TRCS-sales1:20171219091829:20171219091829","DIY-Categorization-news_discovery-TRCS-cbbd___CBBD-News:20190514143100:20190514143100","DIY-Categorization-fnr-TRCS-lng___LNG-News:20180716080158:20180716080158","DIY-Categorization-news_discovery-TRCS-emacru___EMACRU-News:20190423100138:20190423100138","DIY-Categorization-risk-TRCS-governance_risk___GOVR-News:20190905144614:20190905144614","DIY-Categorization-news_discovery-TRCS-hydpwr_pritha___HYDPWR-News:20200123100106:20200123100106","DIY-Categorization-news_discovery-TRCS-wbnk___WBNK-News:20190425132535:20190425132535","DIY-Categorization-news_discovery-TRCS-rnw___RNW-News:20181121101236:20181121101236","DIY-Categorization-news_discovery-TRCS-bokr___BOKR-News:20190204125316:20190204125316","DIY-Categorization-news_discovery-TRCS-boj___BOJ-News:20190301084547:20190301084547","DIY-Categorization-fnr-TRCS-product_launches_rsch_joel___PRDLCH-Research:20180604142910:20180604142910","DIY-Categorization-fnr-TRCS-gaspwr___GASPWR-News:20180713124347:20180713124347","DIY-Categorization-fnr-TRCS-bun___BUN-News:20180612073220:20180612073220","config-refreshableDIY:13:13","DIY-Categorization-news_discovery-TRCS-lux_kairav___LUX-News:20191106135531:20191106135531","DIY-Categorization-news_discovery-TRCS-cleb___CLEB-News:20181221135352:20181221135352","DIY-Categorization-news_discovery-TRCS-petc___PETC-News:20180924104022:20180924104022","DIY-Categorization-fnr-TRCS-crycur:20171121113558:20171121113558","DIY-Categorization-news_discovery-TRCS-dummy___C_3U-News:20180805090719:20180805090719","DIY-Categorization-news_discovery-TRCS-amcru___AMCRU-News:20190211075324:20190211075324","DIY-Categorization-news_discovery-TRCS-defbuy___DEFBUY-News:20181129132759:20181129132759","DIY-Categorization-fnr-TRCS-competition_sai_kiran_rch___COMPTN-Research:20180412104501:20180412104501","DIY-Categorization-fnr-TRCS-rmrmrg:20171204112525:20171204112525","DIY-Categorization-fnr-TRCS-capital_expenditure_rama_rch2-Research:20180305132538:20180305132538","DIY-Categorization-fnr-TRCS-money_laundering_trial___M_1SH-News:20180425122705:20180425122705","DIY-Categorization-news_discovery-TRCS-5gnet___5GNET-News:20200212083421:20200212083421","DIY-Categorization-fnr-TRCS-hjk_sajith___HJK-News:20180427070940:20180427070940","DIY-Categorization-news_discovery-TRCS-mongolia___MN-News:20190924120327:20190924120327","DIY-Categorization-forest_spirit-TRCS-whistleblower_protection___M_25I-News:20191231071911:20191231071911","DIY-Categorization-fnr-TRCS-frxrte:20171229123536:20171229123536","DIY-Categorization-fnr-TRCS-war___WAR-News:20180419081608:20180419081608","DIY-Categorization-fnr-TRCS-esgsoc_2-News:20180315093602:20180315093602","DIY-Categorization-news_discovery-TRCS-na__namibia___NA-News:20191204094254:20191204094254","DIY-Categorization-news_discovery-TRCS-mmt___MMT-News:20181221151050:20181221151050","DIY-Categorization-news_discovery-TRCS-iaea___IAEA-News:20190205145903:20190205145903","DIY-Categorization-news_discovery-TRCS-jersey___JE-News:20191125103337:20191125103337","DIY-Categorization-news_discovery-TRCS-smed___SMED-News:20181116102853:20181116102853","People Index:202003160759:202003160759","DIY-Categorization-news_discovery-TRCS-g20___G20-News:20190604131357:20190604131357","DIY-Categorization-fnr-TRCS-risk_transportation_crime___TCRIM-News:20180717143922:20180717143922","DIY-Categorization-news_discovery-TRCS-italy___IT-News:20191127092312:20191127092312","DIY-Categorization-news_discovery-TRCS-europe___EUROP-News:20200313115735:20200313115735","DIY-Categorization-news_discovery-TRCS-sen___SEN-News:20181218060606:20181218060606","DIY-Categorization-risk-TRCS-cybercriminal_activitiy007___HACK2-News:20181126144420:20181126144420","DIY-Categorization-news_discovery-TRCS-nicaragua___NI-News:20191128152256:20191128152256","DIY-Categorization-fnr-TRCS-mons___MONS-News:20180404103759:20180404103759","DIY-Categorization-fnr-TRCS-pall___PALL-News:20180601131312:20180601131312","DIY-Categorization-news_discovery-TRCS-vu_pritha___VU-News:20190905143231:20190905143231","DIY-Categorization-risk-TRCS-accused_revisit___ACCU-News:20190703121456:20190703121456","DIY-Categorization-news_discovery-TRCS-lv_kairav_new___LV-News:20190806124122:20190806124122","DIY-Categorization-news_discovery-TRCS-aaa___AAA-News:20181203123326:20181203123326","DIY-Categorization-fnr-TRCS-wine1___WINE1-News:20180522120541:20180522120541","DIY-Categorization-news_discovery-TRCS-cy_cyprus___CY-News:20190607102635:20190607102635","DIY-Categorization-fnr-TRCS-ocrim___OCRIM-News:20180417114410:20180417114410","DIY-Categorization-research-TRCS-earnings_surprise___ERNSPS-Research:20190404125405:20190404125405","DIY-Categorization-research-TRCS-estimates_changes_rsch_avadhut___AAESCH-Research:20181113121724:20181113121724","DIY-Categorization-news_discovery-TRCS-ecb___ECB-News:20181221111457:20181221111457","DIY-Categorization-fnr-TRCS-ceo1_new___CEO1-News:20180515143641:20180515143641","DIY-Categorization-fnr-TRCS-gol_new___GOL-News:20180511091509:20180511091509","DIY-Categorization-research-TRCS-results_meet_rsch_avadhut_1___RSMEET-Research:20190411093527:20190411093527","DIY-Categorization-news_discovery-TRCS-kr_pritha___KR-News:20191226140043:20191226140043","DIY-Categorization-news_discovery-TRCS-philippines___PH-News:20190619123836:20190619123836","DIY-Categorization-forest_spirit-TRCS-different_share_classes___M_22Z-News:20191222110432:20191222110432","DIY-Categorization-news_discovery-TRCS-icc___ICC-News:20190225114207:20190225114207","DIY-Categorization-news_discovery-TRCS-tnc___TNC-News:20180809105244:20180809105244","DIY-Categorization-fnr-TRCS-awlq:20170331124441:20170331124441","DIY-Categorization-news_discovery-TRCS-votg___VOTG-News:20181016114921:20181016114921","DIY-Categorization-fnr-TRCS-segment_revenue_rsh_sai_2___SEGREV-Research:20180406153631:20180406153631","DIY-Categorization-fnr-TRCS-forgery_and_couterfeiting___FAKING-News:20180625122007:20180625122007","DIY-Categorization-news_discovery-TRCS-indonesia___ID-News:20191011132404:20191011132404","DIY-Categorization-fnr-TRCS-corporate_valuation_rama_rch___CVALU-Research:20180403102035:20180403102035","DIY-Categorization-news_discovery-TRCS-kh_cambodia___KH-News:20190523094206:20190523094206","DIY-Categorization-fnr-TRCS-ipo___IPO-News:20180803131910:20180803131910","DIY-Categorization-news_discovery-TRCS-cm1___CM1-News:20181011144947:20181011144947","DIY-Categorization-news_discovery-TRCS-cn_kaiirav___CN-News:20191216061230:20191216061230","DIY-Categorization-fnr-TRCS-market_share_rsch_joel_new___MKTSHR-Research:20180403070617:20180403070617","DIY-Categorization-forest_spirit-TRCS-hiv_aids_program___M_25T-News:20200203151409:20200203151409","DIY-Categorization-news_discovery-TRCS-kg_kyrgyzstan_pritha___KG-News:20190607140422:20190607140422","DIY-Categorization-fnr-TRCS-murd_sajith___MURD-News:20180418122332:20180418122332","DIY-Categorization-news_discovery-TRCS-tunisia___TN-News:20190805135959:20190805135959","DIY-Categorization-news_discovery-TRCS-mtnot___MTNOT-News:20190529095843:20190529095843","DIY-Categorization-forest_spirit-TRCS-gri_report_guidelines_prashant___M_23F-News:20191216103350:20191216103350","config-negativeSignature:994:994","DIY-Categorization-news_discovery-TRCS-ie_ravi___IE-News:20190726151005:20190726151005","DIY-Categorization-fnr-TRCS-poil___POIL-News:20180524145900:20180524145900","DIY-Categorization-forest_spirit-TRCS-product_access_low_price___M_26X-News:20200218080420:20200218080420","DIY-Categorization-news_discovery-TRCS-bud___BUD-News:20190102123211:20190102123211","DIY-Categorization-fnr-TRCS-fake1_monop_bnkcap_new:20170405133257:20170405133257","DIY-Categorization-news_discovery-TRCS-mlk___MLK-News:20190128082041:20190128082041","DIY-Categorization-news_discovery-TRCS-united_kingdom___GB-News:20200205074934:20200205074934","DIY-Categorization-news_discovery-TRCS-coll___COLL-News:20181123121657:20181123121657","DIY-Categorization-news_discovery-TRCS-prxf___PRXF-News:20181126112452:20181126112452","DIY-Categorization-fnr-TRCS-nrg___NRG-News:20180515122734:20180515122734","DIY-Categorization-news_discovery-TRCS-bkth___BKTH-News:20190306075204:20190306075204","DIY-Categorization-news_discovery-TRCS-iceland___IS-News:20190718145547:20190718145547","Dial4J:OneCalais_12.1-RELEASE:90","DIY-Categorization-news_discovery-TRCS-rigcnt___RIGCNT-News:20190821112009:20190821112009","DIY-Categorization-news_discovery-TRCS-zimbabwe___ZW-News:20190527124402:20190527124402","DIY-Categorization-risk-TRCS-anticompetitive_behavior___ACB1-News:20181210180256:20181210180256","DIY-Categorization-news_discovery-TRCS-sasia___SASIA-News:20200312085004:20200312085004","DIY-Categorization-news_discovery-TRCS-lebanon___LB-News:20190618113539:20190618113539","DIY-Categorization-news_discovery-TRCS-thtr___THTR-News:20190226162348:20190226162348","DIY-Categorization-news_discovery-TRCS-bo_pritha___BO-News:20191014131654:20191014131654","DIY-Categorization-fnr-TRCS-share_buybacks_repurchase_ram_rh-Research:20180322095215:20180322095215","DIY-Categorization-news_discovery-TRCS-licbed___LICBED-News:20200102121024:20200102121024","DIY-Categorization-news_discovery-TRCS-ukraine___UA-News:20190703080254:20190703080254","DIY-Categorization-forest_spirit-TRCS-human_rights_breaches_suppliers___M_266-News:20200206134836:20200206134836","DIY-Categorization-forest_spirit-TRCS-policy_community_involvement___M_25G-News:20200107144401:20200107144401","DIY-Categorization-fnr-TRCS-nd_cru___CRU-News:20180801093329:20180801093329","DIY-Categorization-fnr-TRCS-animal_welfare_adela___ANWE-News:20180817072819:20180817072819","DIY-Categorization-record_tagging-TRCS-healthcare_fraud___HCFD-News:20200225080429:20200225080429","DIY-Categorization-news_discovery-TRCS-dz_algeria___DZ-News:20190522104859:20190522104859","DIY-Categorization-news_discovery-TRCS-nbpl___NBPL-News:20190201103404:20190201103404","DIY-Categorization-fnr-TRCS-corporate_controversy_rsh_sai2___CVRSY-Research:20180606105315:20180606105315","DIY-Categorization-fnr-TRCS-div___DIV-News:20180727111741:20180727111741","DIY-Categorization-news_discovery-TRCS-oms___OMS-News:20190312133935:20190312133935","DIY-Categorization-record_tagging-TRCS-arm_trafficking___TRAGU-News:20200225080218:20200225080218","DIY-Categorization-fnr-TRCS-dbtr_new:20170315123740:20170315123740","DIY-Categorization-fnr-TRCS-cot___COT-News:20180625140348:20180625140348","DIY-Categorization-fnr-TRCS-risk_illegal_immigration___ILLIM-News:20180621144454:20180621144454","DIY-Categorization-news_discovery-TRCS-rws___RWS-News:20181024152520:20181024152520","DIY-Categorization-risk-TRCS-employment_practices_adela___EMPLP-News:20180830131150:20180830131150","DIY-Categorization-fnr-TRCS-auct___AUCT-News:20180425084455:20180425084455","DIY-Categorization-news_discovery-TRCS-ref___REF-News:20181130084549:20181130084549","DIY-Categorization-forest_spirit-TRCS-biodiversity_impact_reduction___M_23Z-News:20200312060523:20200312060523","DIY-Categorization-news_discovery-TRCS-balt_pritha___BALT-News:20200124141313:20200124141313","DIY-Categorization-news_discovery-TRCS-advo2___ADVO-News:20190710124320:20190710124320","DIY-Categorization-research-TRCS-estimate_increase_rsch_avadhut___AAESIN-Research:20181121120506:20181121120506","DIY-Categorization-news_discovery-TRCS-eswatini___SZ-News:20191004113339:20191004113339","DIY-Categorization-news_discovery-TRCS-pt__portugal___PT-News:20191213113643:20191213113643","DIY-Categorization-news_discovery-TRCS-benin___BJ-News:20191106085344:20191106085344","DIY-Categorization-forest_spirit-TRCS-internal_audit_reporting___M_22I-News:20200307161254:20200307161254","DIY-Categorization-fnr-TRCS-scale_2:20171130072915:20171130072915","DIY-Categorization-fnr-TRCS-dbulk___DBULK-News:20180403110302:20180403110302","DIY-Categorization-fnr-TRCS-gdp-News:20180321130548:20180321130548","DIY-Categorization-news_discovery-TRCS-tt_pritha___TT-News:20190808122703:20190808122703","OA Override:1190:1190","DIY-Categorization-fnr-TRCS-civ___CIV-News:20180607121523:20180607121523","DIY-Categorization-news_discovery-TRCS-secur___SECUR-News:20190118134526:20190118134526","DIY-Categorization-fnr-TRCS-tin1___U_2J-News:20180702121127:20180702121127","DIY-Categorization-fnr-TRCS-hrgt:20170427081548:20170427081548","DIY-Categorization-fnr-TRCS-xpand:20170323144646:20170323144646","DIY-Categorization-fnr-TRCS-timber___TMBR-News:20180712114239:20180712114239","DIY-Categorization-news_discovery-TRCS-uran_updated___URAN-News:20181018135740:20181018135740","DIY-Categorization-fnr-TRCS-intellectual_property_crime_sai____FAKE1-Research:20180705094713:20180705094713","DIY-Categorization-news_discovery-TRCS-tmp___TMP-News:20181003134537:20181003134537","DIY-Categorization-fnr-TRCS-kycterror-News:20180223164257:20180223164257","DIY-Categorization-record_tagging-TRCS-trafficking_stolen_good_2___TRAGD-News:20200225081011:20200225081011","DIY-Categorization-fnr-TRCS-leverage_ratios_rama_rch-Research:20180305132725:20180305132725","DIY-Categorization-fnr-TRCS-innovation___r_d_rsh_sai___INNVTN-Research:20180618153249:20180618153249","DIY-Categorization-news_discovery-TRCS-mt_kairav_new___MT-News:20190807094147:20190807094147","DIY-Categorization-news_discovery-TRCS-rnwpwr___RNWPWR-News:20181008130546:20181008130546","DIY-Categorization-news_discovery-TRCS-de_kairav___DE-News:20191010144805:20191010144805","DIY-Categorization-fnr-TRCS-same_store_sales_sai_rch-Research:20180320084305:20180320084305","DIY-Categorization-news_discovery-TRCS-nscru___NSCRU-News:20190423152943:20190423152943","DIY-Categorization-news_discovery-TRCS-bvi___VG-News:20190523101201:20190523101201","DIY-Categorization-news_discovery-TRCS-dop___DOP-News:20190111151948:20190111151948","DIY-Categorization-news_discovery-TRCS-h2o___H2O-News:20181017151052:20181017151052","DIY-Categorization-news_discovery-TRCS-southern_africa___SAFR-News:20200227125540:20200227125540","DIY-Categorization-fnr-TRCS-rmbse___RMBSE-News:20180725183840:20180725183840","DIY-Categorization-news_discovery-TRCS-pfin___PFIN-News:20190315095259:20190315095259","DIY-Categorization-news_discovery-TRCS-seasia_kairav___SEASIA-News:20200225123838:20200225123838","DIY-Categorization-fnr-TRCS-bonus_share_issues_sai_rch-Research:20180305132618:20180305132618","DIY-Categorization-fnr-TRCS-resass-News:20180208130324:20180208130324","DIY-Categorization-news_discovery-TRCS-european_union___EU-News:20181026171522:20181026171522","DIY-Categorization-fnr-TRCS-allcemrgdvst2:20191105142831:20191105142831","DIY-Categorization-fnr-TRCS-environmental_rsh_nithi___ESGENV-Research:20180706131309:20180706131309","DIY-Categorization-news_discovery-TRCS-dfts___DFTS-News:20181011061404:20181011061404","DIY-Categorization-fnr-TRCS-lead1-News:20180321121644:20180321121644","DIY-Categorization-news_discovery-TRCS-bermuda___BM-News:20190925081500:20190925081500","DIY-Categorization-fnr-TRCS-plat___PLAT-News:20180513085820:20180513085820","DIY-Categorization-news_discovery-TRCS-tblts___TBLTS-News:20200205145258:20200205145258","DIY-Categorization-fnr-TRCS-dip___DIP-News:20180719120345:20180719120345","DIY-Categorization-news_discovery-TRCS-premtl___PREMTL-News:20181026112900:20181026112900","DIY-Categorization-news_discovery-TRCS-calls1___CALLS1-News:20190424071400:20190424071400","DIY-Categorization-news_discovery-TRCS-ghana___GH-News:20190529120707:20190529120707","DIY-Categorization-forest_spirit-TRCS-policy_employee_health_safety_p___M_25U-News:20200108165954:20200108165954","DIY-Categorization-record_tagging-TRCS-bribery_corruption___CRRPT1-News:20200225074834:20200225074834","DIY-Categorization-news_discovery-TRCS-wales___GBW-News:20190827103737:20190827103737","DIY-Categorization-news_discovery-TRCS-kw_kuwait___KW-News:20190805074905:20190805074905","DIY-Categorization-fnr-TRCS-social_rsh_nithish___ESGSOC-Research:20180709125852:20180709125852","DIY-Categorization-news_discovery-TRCS-pipelines___PPL-News:20180904143716:20180904143716","DIY-Categorization-risk-TRCS-convicted___CNVT-News:20181001195847:20181001195847","DIY-Categorization-news_discovery-TRCS-sfts___SFTS-News:20180820104538:20180820104538","DIY-Categorization-fnr-TRCS-ordr:20170313151239:20170313151239","DIY-Categorization-fnr-TRCS-eqb___EQB-News:20180510082724:20180510082724","DIY-Categorization-fnr-TRCS-coc___COC-News:20180423125041:20180423125041","DIY-Categorization-news_discovery-TRCS-fert___FERT-News:20180829081440:20180829081440","DIY-Categorization-fnr-TRCS-coa___COA-News:20180524073631:20180524073631","DIY-Categorization-fnr-TRCS-non_recurring_item_rsch_joel___EXCPTS-Research:20180625122444:20180625122444","DIY-Categorization-news_discovery-TRCS-bnmy___BNMY-News:20190129090707:20190129090707","People Override:994:994","DIY-Categorization-news_discovery-TRCS-mc__monaco___MC-News:20190920125850:20190920125850","DIY-Categorization-forest_spirit-TRCS-policy_freedom_of_association___M_263-News:20200109052220:20200109052220","DIY-Categorization-record_tagging-TRCS-money_laundering_new___MOLND1-News:20200225075716:20200225075716","DIY-Categorization-fnr-TRCS-lith___LITH-News:20180705082554:20180705082554","DIY-Categorization-fnr-TRCS-seasns_kamila-News:20180320120211:20180320120211","DIY-Categorization-fnr-TRCS-change_of_ceo_ram_rch___CEO1-Research:20180403103028:20180403103028","DIY-Categorization-fnr-TRCS-insurg___M_NX-News:20180801124439:20180801124439","DIY-Categorization-fnr-TRCS-aid___AID-News:20180713152341:20180713152341","DIY-Categorization-news_discovery-TRCS-slovenia___SQ-News:20190905114944:20190905114944","index-sluglines:202003031909:202003031909","DIY-Categorization-fnr-TRCS-hrgt_recll:20170522120000:20170522120000","DIY-Categorization-fnr-TRCS-fraud1___FRAUD1-News:20180608135213:20180608135213","index-company-oa3:202003160439:202003160439","DIY-Categorization-risk-TRCS-finance_related_matters___FREM-News:20180817122426:20180817122426","DIY-Categorization-fnr-TRCS-hoil___HOIL-News:20180515122702:20180515122702","DIY-Categorization-news_discovery-TRCS-wash___WASH-News:20181211090229:20181211090229","DIY-Categorization-news_discovery-TRCS-palestine___PS-News:20200302102325:20200302102325","DIY-Categorization-risk-TRCS-macroeconomics___MIECO-News:20181121075630:20181121075630","DIY-Categorization-news_discovery-TRCS-fedspk___FEDSPK-News:20190515101547:20190515101547","DIY-Categorization-fnr-TRCS-rawm___RAWM-News:20180403083541:20180403083541","DIY-Categorization-news_discovery-TRCS-ferr___FERR-News:20181029124609:20181029124609","DIY-Categorization-news_discovery-TRCS-corat___CORAT-News:20190703105333:20190703105333","DIY-Categorization-esg-TRCS-strikes___M_224-News:20191210045013:20191210045013","DIY-Categorization-news_discovery-TRCS-rikbk___RIKBK-News:20190204074901:20190204074901","DIY-Categorization-fnr-TRCS-grndbt___GRNDBT-News:20180727063314:20180727063314","DIY-Categorization-news_discovery-TRCS-rsmeet___RSMEET-News:20181220143551:20181220143551","DIY-Categorization-news_discovery-TRCS-aarup___AARUP-News:20190426070612:20190426070612","index-vessels:202003141834:202003141834","DIY-Categorization-news_discovery-TRCS-wfire___WFIRE-News:20181018075517:20181018075517","DIY-Categorization-news_discovery-TRCS-ccre___CCRE-News:20181221022416:20181221022416","DIY-Categorization-news_discovery-TRCS-npl___NPL-News:20190708081855:20190708081855","DIY-Categorization-fnr-TRCS-brib_r1:20170329090629:20170329090629","DIY-Categorization-news_discovery-TRCS-central_asia___CASIA-News:20200310071349:20200310071349","DIY-Categorization-risk-TRCS-accused___ACCU-News:20181004091009:20181004091009","DIY-Categorization-news_discovery-TRCS-nz_kairav___NZ-News:20191010102821:20191010102821","DIY-Categorization-news_discovery-TRCS-poth___U_4A-News:20190423093052:20190423093052","DIY-Categorization-news_discovery-TRCS-gn__guinea__new___GN-News:20191105140551:20191105140551","DIY-Categorization-research-TRCS-target_price_change_rsch_avadhut___AATPCH-Research:20181019142510:20181019142510","DIY-Categorization-news_discovery-TRCS-nato___NATO-News:20190111033335:20190111033335","DIY-Categorization-news_discovery-TRCS-france_mnemonic_fixed___FR-News:20190528110824:20190528110824","DIY-Categorization-fnr-TRCS-mplt___MPLT-News:20180327111716:20180327111716","DIY-Categorization-news_discovery-TRCS-czech_republic___CZ-News:20191204134208:20191204134208","DIY-Categorization-news_discovery-TRCS-sigind2___SIGIND-News:20200214131301:20200214131301","DIY-Categorization-news_discovery-TRCS-cldtec__cloud_computing___CLDTEC-News:20200303092459:20200303092459","DIY-Categorization-news_discovery-TRCS-chri___CHRI-News:20181127093618:20181127093618","DIY-Categorization-news_discovery-TRCS-nntec_kairav___NNTEC-News:20200103112454:20200103112454","DIY-Categorization-fnr-TRCS-basmtl___BASMTL-News:20180725135819:20180725135819","DIY-Categorization-news_discovery-TRCS-sigdbt___SIGDBT-News:20190605073246:20190605073246","DIY-Categorization-news_discovery-TRCS-sbpk___SBPK-News:20190522121537:20190522121537","DIY-Categorization-news_discovery-TRCS-panama1___PA-News:20190827135127:20190827135127","DIY-Categorization-risk-TRCS-other_security_breach_adela2___SECBR1-News:20180920102426:20180920102426","DIY-Categorization-news_discovery-TRCS-am_kairav___AM-News:20191014121404:20191014121404","DIY-Categorization-fnr-TRCS-stk_new___STK-News:20180705113023:20180705113023","DIY-Categorization-news_discovery-TRCS-lgbt___LGBT-News:20190424101359:20190424101359","DIY-Categorization-news_discovery-TRCS-www___WWW-News:20180920094617:20180920094617","DIY-Categorization-news_discovery-TRCS-open_source_software___OPSRC-News:20200214124939:20200214124939","DIY-Categorization-fnr-TRCS-shareholder_activism_rama_rch-Research:20180320142730:20180320142730","DIY-Categorization-news_discovery-TRCS-art___ART-News:20181224135158:20181224135158","DIY-Categorization-news_discovery-TRCS-jamaica___JM-News:20190812100203:20190812100203","DIY-Categorization-news_discovery-TRCS-france___A_10A-News:20190517102538:20190517102538","DIY-Categorization-news_discovery-TRCS-nascru___NASCRU-News:20190213093304:20190213093304","DIY-Categorization-news_discovery-TRCS-sigstx___SIGSTX-News:20190423135435:20190423135435","DIY-Categorization-news_discovery-TRCS-georgia__country___GE-News:20200122114640:20200122114640","DIY-Categorization-fnr-TRCS-cvalu:20171212133524:20171212133524","DIY-Categorization-fnr-TRCS-performan_result_earning_ram_rch___RES-Research:20180516103928:20180516103928","DIY-Categorization-news_discovery-TRCS-incen___INCEN-News:20190215070918:20190215070918","DIY-Categorization-news_discovery-TRCS-do_pritha___DO-News:20191106151041:20191106151041","DIY-Categorization-news_discovery-TRCS-bangladesh___BD-News:20191202065924:20191202065924","DIY-Categorization-news_discovery-TRCS-mv__maldives___MV-News:20191011093226:20191011093226","DIY-Categorization-news_discovery-TRCS-ruscru___RUSCRU-News:20190222141328:20190222141328","DIY-Categorization-risk-TRCS-enforcement_adela3___ENFORC-News:20181001140710:20181001140710","DIY-Categorization-forest_spirit-TRCS-shareholders_vote_on_executive_p___M_235-News:20191216102330:20191216102330","DIY-Categorization-fnr-TRCS-csent-News:20180314134456:20180314134456","DIY-Categorization-forest_spirit-TRCS-policy_board_diversity___M_22P-News:20200106121706:20200106121706","DIY-Categorization-news_discovery-TRCS-aatpde_new___AATPDE-News:20190422092415:20190422092415","DIY-Categorization-news_discovery-TRCS-opec___OPEC-News:20190509113635:20190509113635","DIY-Categorization-news_discovery-TRCS-tsub___TSUB-News:20200128133447:20200128133447","DIY-Categorization-news_discovery-TRCS-malawi___MW-News:20190823131714:20190823131714","DIY-Categorization-record_tagging-TRCS-antitrust_violations___ANTRV-News:20200225081733:20200225081733","DIY-Categorization-news_discovery-TRCS-pol___POL-News:20181218114130:20181218114130","DIY-Categorization-news_discovery-TRCS-isle_of_man___IM-News:20191021133928:20191021133928","DIY-Categorization-fnr-TRCS-disasters___accidents_rsch_joel___DIS-Research:20180412092858:20180412092858","DIY-Categorization-news_discovery-TRCS-qntc___QNTC-News:20191206123620:20191206123620","DIY-Categorization-news_discovery-TRCS-aainit___AAINIT-News:20190430064624:20190430064624","DIY-Categorization-news_discovery-TRCS-shfv___SHFV-News:20180924111046:20180924111046","DIY-Categorization-risk-TRCS-organized_crime00___OCRIM-News:20181023100724:20181023100724","DIY-Categorization-fnr-TRCS-kycaas_bribery_and_corruption___M_1S5-News:20180524131445:20180524131445","DIY-Categorization-news_discovery-TRCS-covb___COVB-News:20180926082309:20180926082309","DIY-Categorization-research-TRCS-rating_downgrade_rsch_avadhut___AARDW-Research:20181010094454:20181010094454","DIY-Categorization-forest_spirit-TRCS-water_efficiency_policy___M_253-News:20200307161737:20200307161737","DIY-Categorization-news_discovery-TRCS-pntoil___PNTOIL-News:20180907093208:20180907093208","DIY-Categorization-news_discovery-TRCS-at_pritha___AT-News:20190920073458:20190920073458","DIY-Categorization-forest_spirit-TRCS-policy_board_independence_pvm___M_22R-News:20191223024551:20191223024551","DIY-Categorization-news_discovery-TRCS-icj___ICJ-News:20190207135504:20190207135504","DIY-Categorization-record_tagging-TRCS-fraud_1___DCPTN1-News:20200219092812:20200219092812","DIY-Categorization-record_tagging-TRCS-dissolved_company_2___DISCOM-News:20200225080248:20200225080248","DIY-Categorization-news_discovery-TRCS-equatorial_guinea___GQ-News:20191216091751:20191216091751","DIY-Categorization-fnr-TRCS-company_cost_reduction_ram_rch2-Research:20180320152742:20180320152742","DIY-Categorization-news_discovery-TRCS-biosen__biosensors___BIOSEN-News:20200128091221:20200128091221","DIY-Categorization-news_discovery-TRCS-internet_of_things___IOFT-News:20200127120413:20200127120413","DIY-Categorization-record_tagging-TRCS-consoiracy_and_collusion___CNSPR1-News:20200225075147:20200225075147","DIY-Categorization-news_discovery-TRCS-eritrea___ER-News:20190906141317:20190906141317","DIY-Categorization-fnr-TRCS-brge___BRGE-News:20180719153716:20180719153716","DIY-Categorization-news_discovery-TRCS-afrcru___AFRCRU-News:20190329092650:20190329092650","config-forEndUserDisplay:3:3","config-refineries:994:994","DIY-Categorization-news_discovery-TRCS-appp___APPP-News:20190313140641:20190313140641","DIY-Categorization-fnr-TRCS-nap___NAP-News:20180816064933:20180816064933","DIY-Categorization-news_discovery-TRCS-venezuela___VE-News:20190610085246:20190610085246","DIY-Categorization-fnr-TRCS-priv1___PRIV-News:20180716102809:20180716102809","DIY-Categorization-news_discovery-TRCS-minmtl___MINMTL-News:20180829102751:20180829102751","DIY-Categorization-news_discovery-TRCS-geopwr_kairav___GEOPWR-News:20200122115324:20200122115324","DIY-Categorization-fnr-TRCS-dgood___DGOOD-News:20180820123228:20180820123228","DIY-Categorization-news_discovery-TRCS-smrtph__smartphones___SMRTPH-News:20200124133334:20200124133334","DIY-Categorization-news_discovery-TRCS-cttlb___CTTLB-News:20190208143734:20190208143734","DIY-Categorization-fnr-TRCS-cdo2-News:20180315084530:20180315084530","DIY-Categorization-fnr-TRCS-prdlch-News:20180201133354:20180201133354","DIY-Categorization-news_discovery-TRCS-rsmiss___RSMISS-News:20181212144721:20181212144721","DIY-Categorization-record_tagging-TRCS-hate_crimes___HCRIM-News:20200225081224:20200225081224","DIY-Categorization-news_discovery-TRCS-cof___COF-News:20180820110203:20180820110203","DIY-Categorization-news_discovery-TRCS-qatar___QA-News:20190801133301:20190801133301","DIY-Categorization-news_discovery-TRCS-cbar___CBAR-News:20190612110325:20190612110325","DIY-Categorization-news_discovery-TRCS-asm___ASM-News:20190304085508:20190304085508","DIY-Categorization-fnr-TRCS-strategic_combinations_joel_rch-Research:20180315092813:20180315092813","DIY-Categorization-fnr-TRCS-slcn___SLCN-News:20180717075150:20180717075150","DIY-Categorization-fnr-TRCS-ipr-News:20180319134217:20180319134217","DIY-Categorization-news_discovery-TRCS-aiea___AIEA-News:20190715075712:20190715075712","DIY-Categorization-fnr-TRCS-fapm:20170713144316:20170713144316","DIY-Categorization-news_discovery-TRCS-mu_pritha___MU-News:20190910121949:20190910121949","DIY-Categorization-fnr-TRCS-class:20170315123207:20170315123207","DIY-Categorization-forest_spirit-TRCS-votingcap_percentage_jingjing___M_239-News:20191222111007:20191222111007","DIY-Categorization-news_discovery-TRCS-hk_kairav___HK-News:20190808122556:20190808122556","DIY-Categorization-fnr-TRCS-distll___DISTLL-News:20180719130631:20180719130631","DIY-Categorization-forest_spirit-TRCS-public_availability_corporate_lj___M_233-News:20191216100333:20191216100333","DIY-Categorization-research-TRCS-computer_crime_rsch_anusha2___HACK-Research:20181116091317:20181116091317","DIY-Categorization-fnr-TRCS-childexploitation4___KIDEXP-News:20180830131046:20180830131046","DIY-Categorization-news_discovery-TRCS-gra___GRA-News:20180904124713:20180904124713","DIY-Categorization-news_discovery-TRCS-mldm_2___MLDM-News:20190319115709:20190319115709","DIY-Categorization-fnr-TRCS-r_and_d_exp_rsch_avadhut___RDEXP-Research:20180706091400:20180706091400","DIY-Categorization-news_discovery-TRCS-flm2___FLM-News:20190104150654:20190104150654","DIY-Categorization-forest_spirit-TRCS-policy_performance_oriented_d___M_22X-News:20191231105540:20191231105540","DIY-Categorization-forest_spirit-TRCS-pornography_pvm___M_26W-News:20200307161626:20200307161626","DIY-Categorization-news_discovery-TRCS-bri___BRI-News:20181004060629:20181004060629","DIY-Categorization-news_discovery-TRCS-jud___JUD-News:20181109134149:20181109134149","DIY-Categorization-news_discovery-TRCS-marsx__mars_exploration___MARSX-News:20191227105705:20191227105705","config-cse:1188:1188","DIY-Categorization-news_discovery-TRCS-ostr___OSTR-News:20190430153004:20190430153004","DIY-Categorization-news_discovery-TRCS-east_timor___TL-News:20191217111344:20191217111344","DIY-Categorization-news_discovery-TRCS-uupp___UUPP-News:20200212091538:20200212091538","DIY-Categorization-news_discovery-TRCS-solv___SOLV-News:20190425123128:20190425123128","DIY-Categorization-fnr-TRCS-hiring_rsh_nithish1___NEWJBS-Research:20180613082728:20180613082728","DIY-Categorization-record_tagging-TRCS-illegal_gambling___ILLGA-News:20200225080447:20200225080447","DIY-Categorization-news_discovery-TRCS-colombia___CO-News:20190718110557:20190718110557","DIY-Categorization-fnr-TRCS-workers_pay_sai_rsh_2___WPAY-Research:20180731115839:20180731115839","DIY-Categorization-news_discovery-TRCS-lr_kishlay___LR-News:20190523073523:20190523073523","DIY-Categorization-news_discovery-TRCS-cbir___CBIR-News:20190517113645:20190517113645","DIY-Categorization-news_discovery-TRCS-poland___PL-News:20190603142341:20190603142341","DIY-Categorization-news_discovery-TRCS-olef___OLEF-News:20190107151638:20190107151638","DIY-Categorization-news_discovery-TRCS-vrlty_kairav___VRLTY-News:20191224102017:20191224102017","DIY-Categorization-fnr-TRCS-isu_dat_r:20170315124144:20170315124144","DIY-Categorization-forest_spirit-TRCS-shareholders_approval_of_stock___M_234-News:20191230081643:20191230081643","DIY-Categorization-fnr-TRCS-cnsl_sisu:20170321144134:20170321144134","DIY-Categorization-fnr-TRCS-customer_loyalty_sai_rch___CLYLTY-Research:20180409122355:20180409122355","DIY-Categorization-fnr-TRCS-narc_kairav___NARC-News:20180510113450:20180510113450","DIY-Categorization-news_discovery-TRCS-cbng___CBNG-News:20190209130111:20190209130111","DIY-Categorization-fnr-TRCS-war_crime_clint___WRCRIM-News:20180718112943:20180718112943","DIY-Categorization-news_discovery-TRCS-rbnz___RBNZ-News:20190510122351:20190510122351","DIY-Categorization-news_discovery-TRCS-bs__bahamas___BS-News:20191028064133:20191028064133","DIY-Categorization-news_discovery-TRCS-indmrg___INDMRG-News:20190122113813:20190122113813","DIY-Categorization-news_discovery-TRCS-corpd___CORPD-News:20181210072051:20181210072051","DIY-Categorization-news_discovery-TRCS-rice1___RICE1-News:20181016075520:20181016075520","DIY-Categorization-news_discovery-TRCS-burundi___BI-News:20190813065140:20190813065140","DIY-Categorization-fnr-TRCS-managment_pay_rsch_avadhu___MPAY-Research:20180628090155:20180628090155","DIY-Categorization-news_discovery-TRCS-rbin___RBIN-News:20190205074504:20190205074504","DIY-Categorization-news_discovery-TRCS-cbph___CBPH-News:20190213140314:20190213140314","DIY-Categorization-fnr-TRCS-rfo___RFO-News:20180703112522:20180703112522","DIY-Categorization-forest_spirit-TRCS-policy_diversity_and_opportunity___M_25M-News:20191231144605:20191231144605","DIY-Categorization-forest_spirit-TRCS-policy_board_size_prashant___M_22S-News:20191216102818:20191216102818","DIY-Categorization-news_discovery-TRCS-bsri___BSRI-News:20190206074608:20190206074608","DIY-Categorization-esg-TRCS-product_recall_controversies___M_21R-News:20191113095324:20191113095324","DIY-Categorization-risk-TRCS-abuse_of_power2___ABUPO-News:20180904121736:20180904121736","DIY-Categorization-fnr-TRCS-newjbs-News:20180123114120:20180123114120","DIY-Categorization-tms-TRCS-dummy_plano_qa___C_3U-News:20190520090913:20190520090913","DIY-Categorization-fnr-TRCS-steel___U_53-News:20180809121727:20180809121727","DIY-Categorization-fnr-TRCS-shract_r1:20170412111808:20170412111808","DIY-Categorization-news_discovery-TRCS-smrtw___SMRTW-News:20200207082446:20200207082446","DIY-Categorization-fnr-TRCS-demand_rsch_joel2___SALES1-Research:20180427132317:20180427132317","DIY-Categorization-news_discovery-TRCS-agn___AGN-News:20181126111010:20181126111010","DIY-Categorization-news_discovery-TRCS-vots2___VOTS-News:20181220104544:20181220104544","DIY-Categorization-news_discovery-TRCS-unicef_2___UNICEF-News:20190321130757:20190321130757","DIY-Categorization-risk-TRCS-other_controversies___CNTRV1-News:20181113140901:20181113140901","DIY-Categorization-fnr-TRCS-dummy___C_3U-News:20190617112745:20190617112745","DIY-Categorization-news_discovery-TRCS-singapore___SG-News:20190610144031:20190610144031","DIY-Categorization-news_discovery-TRCS-gunctr___GUNCTR-News:20181023112120:20181023112120","DIY-Categorization-news_discovery-TRCS-shpp___SHPP-News:20181008055544:20181008055544","DIY-Categorization-news_discovery-TRCS-gt_guatemala_pritha___GT-News:20190610142913:20190610142913","DIY-Categorization-esg-TRCS-announced_layoffs_controversies___M_222-News:20191203122531:20191203122531","DIY-Categorization-news_discovery-TRCS-paraguay___PY-News:20190611102316:20190611102316","config-vessels:994:994","DIY-Categorization-fnr-TRCS-iron_ore___IRN-News:20180627135950:20180627135950","DIY-Categorization-news_discovery-TRCS-nvsale___NVSALE-News:20190514140917:20190514140917","DIY-Categorization-news_discovery-TRCS-tr_pritha___TR-News:20190910102720:20190910102720","DIY-Categorization-news_discovery-TRCS-cocoil___COCOIL-News:20180905071801:20180905071801","DIY-Categorization-news_discovery-TRCS-cens___CENS-News:20181228174757:20181228174757","DIY-Categorization-news_discovery-TRCS-gbe___GBE-News:20191004083456:20191004083456","DIY-Categorization-news_discovery-TRCS-cm_kairav___CM-News:20191104075230:20191104075230","DIY-Categorization-news_discovery-TRCS-ml_kairav___ML-News:20190906084932:20190906084932","DIY-Categorization-fnr-TRCS-product_shipments_rashmi_rch2-Research:20180313094806:20180313094806","DIY-Categorization-news_discovery-TRCS-forex_sig___SIGFRX-News:20190320110454:20190320110454","DIY-Categorization-fnr-TRCS-risk_crime___CRIM2-News:20180802162728:20180802162728","DIY-Categorization-fnr-TRCS-recap1___RECAP1-News:20180625111339:20180625111339","DIY-Categorization-news_discovery-TRCS-usda1___USDA-News:20190123142330:20190123142330","DIY-Categorization-fnr-TRCS-ngl___NGL-News:20180720104952:20180720104952","DIY-Categorization-news_discovery-TRCS-scotus___SCOTUS-News:20190130121422:20190130121422","DIY-Categorization-news_discovery-TRCS-russia___RU-News:20191217102748:20191217102748","DIY-Categorization-news_discovery-TRCS-antarc___ANTARC-News:20191226144604:20191226144604","DIY-Categorization-news_discovery-TRCS-il_kairav___IL-News:20191105091718:20191105091718","DIY-Categorization-forest_spirit-TRCS-crisis_management_systems_pvm___M_259-News:20200113144318:20200113144318","DIY-Categorization-news_discovery-TRCS-th_kairav___TH-News:20190603083204:20190603083204","OneCalais:13.0.310:310","DIY-Categorization-news_discovery-TRCS-ascru___ASCRU-News:20190207111057:20190207111057","DIY-Categorization-fnr-TRCS-invtry_09_25_2017:20171120093416:20171120093416","DIY-Categorization-news_discovery-TRCS-wireless_technology2___WLEST-News:20200124071936:20200124071936","DIY-Categorization-news_discovery-TRCS-afru___AFRU-News:20190206052557:20190206052557","DIY-Categorization-news_discovery-TRCS-northern_ireland___GBNI-News:20191216115355:20191216115355","DIY-Categorization-news_discovery-TRCS-irdm___IRDM-News:20180907110709:20180907110709","DIY-Categorization-fnr-TRCS-organizational_rest_rsch_joel___REORG-Research:20180508125004:20180508125004","DIY-Categorization-news_discovery-TRCS-stx___STX-News:20190211124219:20190211124219","DIY-Categorization-fnr-TRCS-co2___CO2-News:20180626081018:20180626081018","DIY-Categorization-fnr-TRCS-igd___IGD-News:20180509090349:20180509090349","DIY-Categorization-news_discovery-TRCS-ngo___NGO-News:20190304133314:20190304133314","DIY-Categorization-news_discovery-TRCS-ir__iran___IR-News:20191126092357:20191126092357","DIY-Categorization-news_discovery-TRCS-fld___FLD-News:20181203123932:20181203123932","DIY-Categorization-news_discovery-TRCS-wto___WTO-News:20190220075821:20190220075821","DIY-Categorization-news_discovery-TRCS-cet1cr___CET1CR-News:20190715080407:20190715080407","DIY-Categorization-news_discovery-TRCS-gbr___GBR-News:20190522100114:20190522100114","DIY-Categorization-fnr-TRCS-risk_violent_crime___VCRIM-News:20180607141731:20180607141731","DIY-Categorization-fnr-TRCS-terror_and_related_matters___TERRO-News:20180704090745:20180704090745","DIY-Categorization-fnr-TRCS-mktshr2:20170705120412:20170705120412","DIY-Categorization-risk-TRCS-conspiracy_and_collusion_clint_3___CNSPRC-News:20180829070325:20180829070325","DIY-Categorization-news_discovery-TRCS-fiji_kairav___FJ-News:20190515065719:20190515065719","DIY-Categorization-news_discovery-TRCS-robot_kairav___ROBOT-News:20191224085324:20191224085324","DIY-Categorization-news_discovery-TRCS-stmcel__stem_cell_therapy___STMCEL-News:20200123122108:20200123122108","DIY-Categorization-news_discovery-TRCS-carreg___CARREG-News:20180903095952:20180903095952","DIY-Categorization-news_discovery-TRCS-quak___M_1MB-News:20180828115807:20180828115807","DIY-Categorization-news_discovery-TRCS-hn__honduras___HN-News:20191009075343:20191009075343","DIY-Categorization-news_discovery-TRCS-rpm___RPM-News:20190304104014:20190304104014","DIY-RCS-fnr-TRCS-cavs:6:6","DIY-Categorization-news_discovery-TRCS-french_guiana___GF-News:20191122122029:20191122122029","DIY-Categorization-fnr-TRCS-key_performance_ind_rsch_joel___KPIS-Research:20180719120953:20180719120953","DIY-Categorization-news_discovery-TRCS-belarus___BY-News:20191003070113:20191003070113","DIY-Categorization-forest_spirit-TRCS-policy_data_privacy___M_26T-News:20191229120222:20191229120222","DIY-Categorization-forest_spirit-TRCS-embryonic_stem_cell_research_pvm___M_26J-News:20200226103055:20200226103055","DIY-Categorization-fnr-TRCS-wom___WOM-News:20180508045954:20180508045954","DIY-Categorization-risk-TRCS-human_rights_violation_adela___HRIGV-News:20190315143548:20190315143548","DIY-Categorization-news_discovery-TRCS-drwn___DRWN-News:20181116065331:20181116065331","DIY-Categorization-news_discovery-TRCS-chs___CHS-News:20190109073527:20190109073527","DIY-Categorization-fnr-TRCS-divestitures_spin_offs_rash_rch-Research:20180305132659:20180305132659","DIY-Categorization-fnr-TRCS-jet___JET-News:20180604151907:20180604151907","DIY-Categorization-news_discovery-TRCS-bks___BKS-News:20181001124556:20181001124556","DIY-Categorization-news_discovery-TRCS-wafr___WAFR-News:20200218083221:20200218083221","DIY-Categorization-news_discovery-TRCS-bsent___BSENT-News:20190318072953:20190318072953","DIY-Categorization-fnr-TRCS-mtg___MTG-News:20180423093530:20180423093530","DIY-Categorization-fnr-TRCS-margin_expansion_rsch_joel___MARGNU-Research:20180706132335:20180706132335","DIY-Categorization-news_discovery-TRCS-nafta___NAFTA-News:20190207123747:20190207123747","DIY-Categorization-news_discovery-TRCS-gibraltar___GI-News:20191016094138:20191016094138","DIY-Categorization-news_discovery-TRCS-corn___COR-News:20180817132653:20180817132653","DIY-Categorization-news_discovery-TRCS-lithuania___LT-News:20191009095516:20191009095516","DIY-Categorization-fnr-TRCS-eub___EUB-News:20180327093517:20180327093517","DIY-Categorization-news_discovery-TRCS-ctraf___CTRAF-News:20191024061016:20191024061016","DIY-Categorization-news_discovery-TRCS-nplr___NPLR-News:20190717072649:20190717072649","DIY-Categorization-news_discovery-TRCS-uruguay___UY-News:20190805091851:20190805091851","DIY-Categorization-fnr-TRCS-blktrd_r1:20171116075645:20171116075645","DIY-Categorization-news_discovery-TRCS-cmbs___CMBS-News:20180918144819:20180918144819","DIY-Categorization-news_discovery-TRCS-be_pritha___BE-News:20191114141854:20191114141854","DIY-Categorization-news_discovery-TRCS-lsci___LSCI-News:20200207154126:20200207154126","DIY-Categorization-record_tagging-TRCS-insolvency__liquidation____bankr___INLIBK-News:20200225080914:20200225080914","DIY-Categorization-fnr-TRCS-znc___ZNC-News:20180412130505:20180412130505","DIY-Categorization-record_tagging-TRCS-environmental_crime_wynanda___EVCRIM-News:20200225080742:20200225080742","DIY-Categorization-news_discovery-TRCS-rpk___RPK-News:20190304165532:20190304165532","DIY-Categorization-forest_spirit-TRCS-board_member_re_election_years___M_22M-News:20200106145937:20200106145937","DIY-Categorization-news_discovery-TRCS-nucl___NUCL-News:20181213104105:20181213104105","DIY-Categorization-fnr-TRCS-trd_tax:20170309132522:20170309132522","DIY-Categorization-news_discovery-TRCS-kazakhstan___KZ-News:20190606151502:20190606151502","DIY-Categorization-news_discovery-TRCS-slovakia___SK-News:20190806130248:20190806130248","DIY-Categorization-news_discovery-TRCS-sigdrv___SIGDRV-News:20190625073905:20190625073905","DIY-Categorization-news_discovery-TRCS-nigeria___NG-News:20190717070356:20190717070356","DIY-Categorization-fnr-TRCS-reorg_new___REORG-News:20180712123210:20180712123210","DIY-Categorization-news_discovery-TRCS-vgame___VGAME-News:20181009133717:20181009133717","DIY-Categorization-fnr-TRCS-product_recalls_rsh_sai2___RECLL-Research:20180425121704:20180425121704","DIY-Categorization-news_discovery-TRCS-libya___LY-News:20190710094642:20190710094642","DIY-Categorization-news_discovery-TRCS-sa_pritha___SA-News:20190703111552:20190703111552","DIY-Categorization-fnr-TRCS-expansions_new_markets_sai_rsh2___XPAND-Research:20180605143141:20180605143141","DIY-Categorization-news_discovery-TRCS-resrvs__reserves___RESRVS-News:20191121100040:20191121100040","DIY-Categorization-news_discovery-TRCS-tw__taiwan___TW-News:20190715074239:20190715074239","DIY-Categorization-news_discovery-TRCS-camer__pritha___CAMER-News:20200218115811:20200218115811","DIY-Categorization-news_discovery-TRCS-senegal___SN-News:20190812063659:20190812063659","DIY-Categorization-news_discovery-TRCS-croatia___HR-News:20190530142431:20190530142431","DIY-Categorization-forest_spirit-TRCS-health_safety_management_syste___M_25R-News:20200212103754:20200212103754","DIY-Categorization-forest_spirit-TRCS-waste_reduction_initiatives___M_241-News:20200303061845:20200303061845","DIY-Categorization-fnr-TRCS-wheat___WHT-News:20180612114051:20180612114051","DIY-Categorization-news_discovery-TRCS-greenland___GL-News:20191001123800:20191001123800","DIY-Categorization-news_discovery-TRCS-cargm2___CARGM-News:20190905111833:20190905111833","DIY-Categorization-news_discovery-TRCS-upp___UPP-News:20191224151821:20191224151821","DIY-Categorization-fnr-TRCS-sov_2:20170522130956:20170522130956","DIY-Categorization-news_discovery-TRCS-ma_pritha___MA-News:20190619124047:20190619124047","DIY-Categorization-fnr-TRCS-risk_information_leaked___LKD-News:20180611080047:20180611080047","DIY-Categorization-news_discovery-TRCS-crsh___CRSH-News:20180926152016:20180926152016","DIY-Categorization-risk-TRCS-data_breach___DATBR-News:20180924094703:20180924094703","DIY-Categorization-fnr-TRCS-bons:20170406103713:20170406103713","DIY-Categorization-fnr-TRCS-env___ENV-News:20180628080033:20180628080033","DIY-Categorization-news_discovery-TRCS-yemen___YE-News:20191007152936:20191007152936","DIY-Categorization-news_discovery-TRCS-mauritania___MR-News:20191007083600:20191007083600","DIY-Categorization-news_discovery-TRCS-nwp___NWP-News:20190704091624:20190704091624","DIY-Categorization-fnr-TRCS-fake1___FAKE1-News:20180516150141:20180516150141","DIY-Categorization-fnr-TRCS-gilesriskconvicetd___CNVT-News:20180706115730:20180706115730","DIY-Categorization-forest_spirit-TRCS-lijian_environment_training___M_24X-News:20200312080836:20200312080836","DIY-Categorization-fnr-TRCS-antitrust_competitive_behaviour2___MONOP-Research:20180430152713:20180430152713","DIY-Categorization-news_discovery-TRCS-tgsn___U_44-News:20190423093005:20190423093005","DIY-Categorization-fnr-TRCS-racr___RACR-News:20180404134359:20180404134359","DIY-Categorization-esg-TRCS-anti_competition_controversy___M_21W-News:20191202110839:20191202110839","DIY-Categorization-fnr-TRCS-corruption___bribery___joel_rch-Research:20180319102344:20180319102344","DIY-Categorization-fnr-TRCS-humanrightsviol2___M_1W7-News:20180621141751:20180621141751","DIY-Categorization-fnr-TRCS-commercial_contract_wins_sai_rch___ORDR-Research:20180413122334:20180413122334","DIY-Categorization-news_discovery-TRCS-oecd___OECD-News:20190201111651:20190201111651","DIY-Categorization-news_discovery-TRCS-muni___MUNI-News:20181218104939:20181218104939","DIY-Categorization-forest_spirit-TRCS-oecd_guidelines_multinational___M_25D-News:20200204102334:20200204102334","DIY-Categorization-fnr-TRCS-change_of_chairperson_rsch_joel___CHAIR1-Research:20180430061611:20180430061611","DIY-Categorization-fnr-TRCS-exchange_rate_impact_rama_rsh-Research:20180312091507:20180312091507","DIY-Categorization-fnr-TRCS-bankruptcy_insolvency_rsch_joel-Research:20180315095938:20180315095938","DIY-Categorization-fnr-TRCS-pep_exposure___PEXP-News:20180727101113:20180727101113","DIY-Categorization-fnr-TRCS-blkchn_4-News:20180123105513:20180123105513","DIY-Categorization-fnr-TRCS-bomb___BOMB-News:20180509143515:20180509143515","DIY-Categorization-news_discovery-TRCS-moonx_pritha___MOONX-News:20200316065926:20200316065926","DIY-Categorization-fnr-TRCS-research_regulatory_issues___REGS-Research:20180516142505:20180516142505","DIY-Categorization-news_discovery-TRCS-digpay___DIGPAY-News:20200109080750:20200109080750","DIY-Categorization-fnr-TRCS-labour_disputes_sai_rsh___DISP-Research:20180713115603:20180713115603","DIY-Categorization-fnr-TRCS-segrev:20171117133125:20171117133125","DIY-Categorization-research-TRCS-target_price_decrease_rsch___AATPDE-Research:20181029092516:20181029092516","DIY-Categorization-forest_spirit-TRCS-supplier_esg_training___M_278-News:20200306124830:20200306124830","DIY-Categorization-news_discovery-TRCS-twave___TWAVE-News:20180925084150:20180925084150","DIY-Categorization-news_discovery-TRCS-comarb___COMARB-News:20180809100505:20180809100505","DIY-Categorization-news_discovery-TRCS-group7___G7-News:20190423101721:20190423101721","DIY-Categorization-news_discovery-TRCS-lif___LIF-News:20190607124347:20190607124347","DIY-Categorization-record_tagging-TRCS-illegal_restraint__kidnapping___RESKID-News:20200225080543:20200225080543","DIY-Categorization-news_discovery-TRCS-asp_average_selling_price___ASP-News:20190806100827:20190806100827","DIY-Categorization-fnr-TRCS-dvst___DVST-News:20180716074612:20180716074612","DIY-Categorization-fnr-TRCS-kyc_embezzlement___M_1SE-News:20180430061642:20180430061642","DIY-Categorization-fnr-TRCS-hrgt_namec:20170329090731:20170329090731","DIY-Categorization-risk-TRCS-supply_disruption___SUPDI-News:20181030110609:20181030110609","DIY-Categorization-record_tagging-TRCS-sexual_exploitation_wc_renata___SEXEX-News:20200225081811:20200225081811","DIY-Categorization-risk-TRCS-cyber_security___CBRS-News:20181001113416:20181001113416","DIY-Categorization-risk-TRCS-accidents_and_negligence7___ACNEG-News:20181001085547:20181001085547","DIY-Categorization-news_discovery-TRCS-pighog___PIGHOG-News:20181016130631:20181016130631","DIY-Categorization-news_discovery-TRCS-pboc___PBOC-News:20190201085925:20190201085925","DIY-Categorization-research-TRCS-initiations_of_coverage_sai_rsh___AAINIT-Research:20181005105114:20181005105114","DIY-Categorization-forest_spirit-TRCS-policy_shareholder_engagement_p___M_232-News:20191222113240:20191222113240","DIY-Categorization-news_discovery-TRCS-voth_updated___VOTH-News:20181213071522:20181213071522","DIY-Categorization-news_discovery-TRCS-td_kairav___TD-News:20191104095000:20191104095000","DIY-Categorization-risk-TRCS-convicted3___CNVT-News:20181003071315:20181003071315","DIY-Categorization-news_discovery-TRCS-afghanistan___AF-News:20190606110337:20190606110337","DIY-Categorization-news_discovery-TRCS-steel___STE-News:20190612092241:20190612092241","DIY-Categorization-news_discovery-TRCS-solomon_islands___SB-News:20190909115904:20190909115904","DIY-Categorization-news_discovery-TRCS-tnkr___TNKR-News:20180910104426:20180910104426","DIY-Categorization-fnr-TRCS-change_of_president_joel_rch___PRES1-Research:20180402085923:20180402085923","DIY-Categorization-news_discovery-TRCS-cznb___CZNB-News:20190722113353:20190722113353","DIY-Categorization-news_discovery-TRCS-lossr___LOSSR-News:20190319152636:20190319152636","DIY-RCS-fnr-TRCS-nd:3:3","DIY-Categorization-news_discovery-TRCS-bosnia___BA-News:20191121122727:20191121122727","DIY-Categorization-fnr-TRCS-stx:20170530123138:20170530123138","DIY-Categorization-news_discovery-TRCS-bb__barbados___BB-News:20190910102118:20190910102118","DIY-Categorization-fnr-TRCS-edu___EDU-News:20180717083248:20180717083248","DIY-Categorization-fnr-TRCS-pmi-News:20180214153232:20180214153232","DIY-Categorization-news_discovery-TRCS-netherlands___NL-News:20191023095157:20191023095157","DIY-Categorization-news_discovery-TRCS-nbua___NBUA-News:20190514142222:20190514142222","DIY-Categorization-news_discovery-TRCS-sv_kairav___SV-News:20190827104259:20190827104259","config-physicalAssets-mines:994:994","DIY-Categorization-news_discovery-TRCS-boc___BOC-News:20190408073659:20190408073659","DIY-Categorization-news_discovery-TRCS-rack___RACK-News:20190110091330:20190110091330","DIY-Categorization-news_discovery-TRCS-snb___SNB-News:20190204114529:20190204114529","DIY-Categorization-news_discovery-TRCS-bw__botswana___BW-News:20191111132416:20191111132416","DIY-Categorization-news_discovery-TRCS-aaesde___AAESDE-News:20190423093211:20190423093211","DIY-Categorization-news_discovery-TRCS-apec___APEC-News:20190424104009:20190424104009","DIY-Categorization-news_discovery-TRCS-unesco___UNESCO-News:20190219083435:20190219083435","DIY-Categorization-news_discovery-TRCS-sorg___SORG-News:20181017100425:20181017100425","DIY-Categorization-news_discovery-TRCS-pplmov___PPLMOV-News:20181105164402:20181105164402","DIY-Categorization-news_discovery-TRCS-intag__international_agencies_____INTAG-News:20200218112456:20200218112456","DIY-Categorization-news_discovery-TRCS-msic2___MSIC-News:20181217170549:20181217170549","DIY-Categorization-news_discovery-TRCS-solpwr__solar_power_stations___SOLPWR-News:20200127122052:20200127122052","DIY-Categorization-news_discovery-TRCS-sigrea___SIGREA-News:20190909121325:20190909121325","DIY-Categorization-news_discovery-TRCS-def___DEF-News:20181214130044:20181214130044","DIY-Categorization-news_discovery-TRCS-scrim___SCRIM-News:20181228143829:20181228143829","DIY-Categorization-research-TRCS-results_miss_rsch_avadhut_1___RSMISS-Research:20190410100411:20190410100411","DIY-Categorization-forest_spirit-TRCS-policy_supply_chain_health_safet___M_25X-News:20200124161503:20200124161503","BlackList:1188:1188","DIY-Categorization-news_discovery-TRCS-haiti___HT-News:20190916110926:20190916110926","DIY-Categorization-news_discovery-TRCS-smrth_kairav___SMRTH-News:20191217104433:20191217104433","DIY-Categorization-news_discovery-TRCS-mk__north_macedonia___MK-News:20191227120401:20191227120401","DIY-Categorization-tms-TRCS-publish_dummy___C_3U-News:20190217151324:20190217151324","DIY-Categorization-esg-TRCS-management_comp_controversies___M_21E-News:20191217074314:20191217074314","DIY-Categorization-news_discovery-TRCS-masg___MASG-News:20190311110839:20190311110839","DIY-Categorization-fnr-TRCS-mgs___MGS-News:20180529091421:20180529091421","DIY-Categorization-fnr-TRCS-rlft___RLFT-News:20180802124812:20180802124812","DIY-Categorization-forest_spirit-TRCS-animal_testing___M_26C-News:20200203083522:20200203083522","DIY-Categorization-forest_spirit-TRCS-policy_skills_training_pvm___M_276-News:20200108164808:20200108164808","DIY-Categorization-fnr-TRCS-shpmnt_new2:20171116121809:20171116121809","DIY-Categorization-record_tagging-TRCS-arms_and_ammunition_possession_1___UNGUN1-News:20200225074841:20200225074841","DIY-Categorization-risk-TRCS-industry_risk___INDR-News:20181218073238:20181218073238","DIY-Categorization-news_discovery-TRCS-orggro___ORGGRO-News:20190708111920:20190708111920","DIY-Categorization-fnr-TRCS-gol-News:20180319131232:20180319131232","DIY-Categorization-news_discovery-TRCS-huma___HUMA-News:20190610105157:20190610105157","DIY-Categorization-news_discovery-TRCS-lk_kairav___LK-News:20191028114705:20191028114705","DIY-Categorization-fnr-TRCS-shl___SHL-News:20180420142214:20180420142214","DIY-Categorization-news_discovery-TRCS-diam1___DIAM-News:20181016084719:20181016084719","DIY-Categorization-news_discovery-TRCS-mecru___MECRU-News:20190403074655:20190403074655","DIY-Categorization-news_discovery-TRCS-dair___DAIR-News:20181011123944:20181011123944","DIY-Categorization-forest_spirit-TRCS-csr_sustainability_committee___M_23C-News:20200203051937:20200203051937","DIY-Categorization-news_discovery-TRCS-pakistan___PK-News:20190626075306:20190626075306","DIY-Categorization-risk-TRCS-unlawful_market_conduct___UNMCO-News:20180830103801:20180830103801","DIY-Categorization-fnr-TRCS-crptax:20171117095403:20171117095403","DIY-Categorization-forest_spirit-TRCS-corporate_responsibility_awards___M_258-News:20200206062918:20200206062918","DIY-Categorization-risk-TRCS-forced_slave_labor___SLAVE-News:20190422140256:20190422140256","DIY-Categorization-risk-TRCS-animal_welfare____ANWE-News:20181107095613:20181107095613","DIY-Categorization-fnr-TRCS-caput___CAPUT-News:20180814082905:20180814082905","DIY-Categorization-news_discovery-TRCS-mozambique___MZ-News:20191118092856:20191118092856","DIY-Categorization-risk-TRCS-financial_difficulty_clint___FINDIF-News:20180830092829:20180830092829","DIY-Categorization-fnr-TRCS-cppr-News:20180315144023:20180315144023","DIY-Categorization-fnr-TRCS-risk_charged___CHRG-News:20180706134206:20180706134206","index-ports:202003142009:202003142009","DIY-Categorization-forest_spirit-TRCS-human_rights_suppliers_nug___M_267-News:20200117121721:20200117121721","DIY-Categorization-news_discovery-TRCS-national_government_debt___GVD-News:20190123103411:20190123103411","DIY-Categorization-news_discovery-TRCS-cuba___CU-News:20190702092747:20190702092747","DIY-Categorization-news_discovery-TRCS-romania___RO-News:20190809121347:20190809121347","DIY-Categorization-forest_spirit-TRCS-renewable_energy_use___M_24T-News:20200312075013:20200312075013","DIY-Categorization-news_discovery-TRCS-hkmn___HKMN-News:20190125104038:20190125104038","DIY-Categorization-news_discovery-TRCS-islm___ISLM-News:20190522094333:20190522094333","config-physicalAssets-ports:994:994","DIY-Categorization-news_discovery-TRCS-prcp___PRCP-News:20181126121507:20181126121507","DIY-Categorization-fnr-TRCS-opt___OPT-News:20180706121423:20180706121423","DIY-Categorization-news_discovery-TRCS-aum___AUM-News:20190723131201:20190723131201","DIY-Categorization-fnr-TRCS-ream:20170529065740:20170529065740","DIY-Categorization-forest_spirit-TRCS-armaments___M_26G-News:20200204024856:20200204024856","DIY-Categorization-news_discovery-TRCS-azerbaijan___AZ-News:20190626065421:20190626065421","DIY-Categorization-fnr-TRCS-rub___RUB-News:20180510113353:20180510113353","DIY-Categorization-news_discovery-TRCS-hu_kairav_new___HU-News:20190719114812:20190719114812","DIY-Categorization-fnr-TRCS-chld:20170524084254:20170524084254","DIY-Categorization-news_discovery-TRCS-seeu___SEEU-News:20200310093010:20200310093010","DIY-Categorization-forest_spirit-TRCS-policy_bribery_and_corruption___M_25E-News:20191231030547:20191231030547","DIY-Categorization-fnr-TRCS-raw_material_cost_rsch_joel___RAWM-Research:20180607121525:20180607121525","DIY-Categorization-news_discovery-TRCS-central_african_republic___CF-News:20190722075226:20190722075226","DIY-Categorization-fnr-TRCS-results_forecasts_warnings_sai_r___RESF-Research:20180710145413:20180710145413","DIY-Categorization-fnr-TRCS-risk_unlawful_possession_weapons___UNGUN-News:20180703113037:20180703113037","DIY-Categorization-news_discovery-TRCS-agri___AGRI-News:20181025062058:20181025062058","DIY-Categorization-news_discovery-TRCS-brunei___BN-News:20191015115048:20191015115048","DIY-Categorization-news_discovery-TRCS-coapwr___COAPWR-News:20180911105149:20180911105149","DIY-Categorization-news_discovery-TRCS-sigmce___SIGMCE-News:20190726143355:20190726143355","DIY-Categorization-forest_spirit-TRCS-environment_supply_chain_selecti___M_24M-News:20200307161431:20200307161431","DIY-Categorization-news_discovery-TRCS-djibouti___DJ-News:20190814093902:20190814093902","DIY-Categorization-news_discovery-TRCS-plf___PLF-News:20190225100937:20190225100937","DIY-Categorization-news_discovery-TRCS-bulgaria___BG-News:20191014075341:20191014075341","DIY-Categorization-record_tagging-TRCS-new_forgery___M_1S0-News:20200225075223:20200225075223","DIY-Categorization-fnr-TRCS-allce:20170323145654:20170323145654","DIY-Categorization-news_discovery-TRCS-ar_kairav___AR-News:20191202094222:20191202094222","DIY-Categorization-esg-TRCS-shareholder_rights_controversies___M_21F-News:20190930103950:20190930103950","DIY-Categorization-forest_spirit-TRCS-lijian_external_consultant___M_22H-News:20191217023745:20191217023745","DIY-Categorization-news_discovery-TRCS-congo__rc____CG-News:20191127151215:20191127151215","DIY-Categorization-fnr-TRCS-medreg:20170508120210:20170508120210","DIY-Categorization-fnr-TRCS-equity_stakes_rama_rch___STK-Research:20180329064332:20180329064332","DIY-Categorization-record_tagging-TRCS-frozen_seized_assets_wc_renata___FRAS-News:20200225081354:20200225081354","DIY-Categorization-forest_spirit-TRCS-lijian_policy_board_experience___M_22Q-News:20191222111742:20191222111742","DIY-Categorization-research-TRCS-rating_upgrade_avadhut___AARUP-Research:20181005115413:20181005115413","DIY-Categorization-news_discovery-TRCS-aumflo___AUMFLO-News:20190909113822:20190909113822","DIY-Categorization-fnr-TRCS-medical_regulatory_issues_rsch___MEDREG-Research:20180716104824:20180716104824","DIY-Categorization-news_discovery-TRCS-finreg___FINREG-News:20190513092205:20190513092205","DIY-Categorization-news_discovery-TRCS-jp_kairav___JP-News:20190604113143:20190604113143","DIY-Categorization-news_discovery-TRCS-united_arab_emirates___AE-News:20200113142202:20200113142202","DIY-Categorization-forest_spirit-TRCS-agrochemical_products_pvm___M_26A-News:20200226102308:20200226102308","DIY-Categorization-news_discovery-TRCS-sds___SDS-News:20190208103811:20190208103811","DIY-Categorization-news_discovery-TRCS-rsbeat___RSBEAT-News:20181207120341:20181207120341","config-drugs:994:994","DIY-Categorization-fnr-TRCS-modernslavery1___M_1W2-News:20180719120126:20180719120126","DIY-Categorization-forest_spirit-TRCS-state_owned_enterprise_prashant___M_236-News:20191222101100:20191222101100","DIY-Categorization-news_discovery-TRCS-eci___ECI-News:20181116124304:20181116124304","DIY-Categorization-record_tagging-TRCS-theft_and_embezzlement___THFT1-News:20200219093808:20200219093808","DIY-Categorization-news_discovery-TRCS-fi_finland___FI-News:20190522112057:20190522112057","DIY-Categorization-news_discovery-TRCS-leveraged_loans___LEVLOA-News:20200110104158:20200110104158","DIY-Categorization-news_discovery-TRCS-gos___GOS-News:20180927132842:20180927132842","DIY-Categorization-fnr-TRCS-hyd___HYD-News:20180418120003:20180418120003","DIY-Categorization-news_discovery-TRCS-cofara___COFARA-News:20190107112822:20190107112822","DIY-Categorization-news_discovery-TRCS-natu___NATU-News:20181011122145:20181011122145","DIY-Categorization-risk-TRCS-significant_company_restructure___SIGCOR-News:20181025081439:20181025081439","DIY-Categorization-news_discovery-TRCS-config___CONFIG-News:20181126101910:20181126101910","DIY-Categorization-news_discovery-TRCS-sbvn1___SBVN-News:20190522081010:20190522081010","DIY-Categorization-fnr-TRCS-publish_dummy_topic___C_3U-News:20190114110225:20190114110225","DIY-Categorization-news_discovery-TRCS-gfin___GFIN-News:20181119100414:20181119100414","DIY-Categorization-forest_spirit-TRCS-policy_business_ethics___M_25F-News:20200214173120:20200214173120","DIY-Categorization-news_discovery-TRCS-nim___NIM-News:20190705115033:20190705115033","DIY-Categorization-risk-TRCS-exonerated_jithin___EXNRT-News:20200305144937:20200305144937","DIY-Categorization-fnr-TRCS-risk_tax_evasion___TAXNC-News:20180710140943:20180710140943","DIY-Categorization-fnr-TRCS-sug___SUG-News:20180809150330:20180809150330","DIY-Categorization-record_tagging-TRCS-wildlife_crime___WICRIM-News:20200225080933:20200225080933","DIY-Categorization-news_discovery-TRCS-pe_kairav___PE-News:20190904112731:20190904112731","DIY-Categorization-news_discovery-TRCS-plas___PLAS-News:20190215065422:20190215065422","DIY-Categorization-news_discovery-TRCS-autonomous_vehicles___CAVS-News:20200108134630:20200108134630","DIY-Categorization-news_discovery-TRCS-cofrob___COFROB-News:20190219122006:20190219122006","DIY-Categorization-fnr-TRCS-mrg_new___MRG-News:20180615115823:20180615115823","DIY-Categorization-fnr-TRCS-exrsks:20171129184952:20171129184952","DIY-Categorization-news_discovery-TRCS-gbs___GBS-News:20200123152121:20200123152121","DIY-Categorization-news_discovery-TRCS-costa_rica___CR-News:20190925132431:20190925132431","DIY-Categorization-news_discovery-TRCS-biocel___BIOCEL-News:20190314143248:20190314143248","DIY-Categorization-news_discovery-TRCS-switzerland___CH-News:20190821113625:20190821113625","DIY-Categorization-fnr-TRCS-silver___SLVR-News:20180711114536:20180711114536"]},"http:\/\/d.opencalais.com\/comphash-1\/40cd4901-3bf1-3417-a016-b9cba7e8f6e8":{"_typeGroup":"entities","_type":"Company","forenduserdisplay":"false","name":"STXEc1","nationality":"N\/A","score":3,"confidencelevel":"0.196","frompersoncareer":"0","__label":"Processing section","__value":"STXEc1","recognizedas":"name","_typeReference":"http:\/\/s.opencalais.com\/1\/type\/em\/e\/Company","instances":[{"detection":"[by the Federal Reserve.\n\nEuro Stoxx 50 futures ]STXEc1[ plunged 8.3% to their lowest levels since]","prefix":"by the Federal Reserve.\n\nEuro Stoxx 50 futures ","exact":"STXEc1","suffix":" plunged 8.3% to their lowest levels since","offset":545,"length":6}],"relevance":0.2,"confidence":{"statisticalfeature":"0.489","dblookup":"0.0","resolution":"0.0","aggregate":"0.196"}},"http:\/\/d.opencalais.com\/genericHasher-1\/0d87e8a5-fb41-33c9-a1fb-bedf134b384e":{"_typeGroup":"entities","_type":"Position","forenduserdisplay":"false","name":"President","__label":"1","__value":"President","_typeReference":"http:\/\/s.opencalais.com\/1\/type\/em\/e\/Position","instances":[{"detection":"[as stocks dived and oil slumped after U.S. ]President[ Donald Trump took the dramatic step of banning]","prefix":"as stocks dived and oil slumped after U.S. ","exact":"President","suffix":" Donald Trump took the dramatic step of banning","offset":99,"length":9}],"relevance":0.2},"http:\/\/d.opencalais.com\/genericHasher-1\/31ec6404-85fe-33f5-af02-cb37fec6b170":{"_typeGroup":"entities","_type":"IndustryTerm","forenduserdisplay":"false","name":"oil","__label":"18","__value":"oil","_typeReference":"http:\/\/s.opencalais.com\/1\/type\/em\/e\/IndustryTerm","instances":[{"detection":"[markets reeled on Thursday as stocks dived and ]oil[ slumped after U.S. President Donald Trump took]","prefix":"markets reeled on Thursday as stocks dived and ","exact":"oil","suffix":" slumped after U.S. President Donald Trump took","offset":76,"length":3}],"relevance":0.2},"http:\/\/d.opencalais.com\/genericHasher-1\/8d32ccfd-7214-30b7-bc38-6fff5e3bc25b":{"_typeGroup":"entities","_type":"MarketIndex","forenduserdisplay":"true","name":"S&P 500 INDEX - CBOE","ric":".SPX","confidencelevel":"0.97072035","_typeReference":"http:\/\/s.opencalais.com\/1\/type\/em\/e\/MarketIndex","permid":"https:\/\/permid.org\/1-404018","instances":[{"detection":"[Asia and last traded down 3.6%, a day after the ]S&P 500[ .SPX lost 4.89%, leaving the index on the brink]","prefix":"Asia and last traded down 3.6%, a day after the ","exact":"S&P 500","suffix":" .SPX lost 4.89%, leaving the index on the brink","offset":828,"length":7},{"detection":"[last traded down 3.6%, a day after the S&P 500 ].SPX[ lost 4.89%, leaving the index on the brink of]","prefix":"last traded down 3.6%, a day after the S&P 500 ","exact":".SPX","suffix":" lost 4.89%, leaving the index on the brink of","offset":836,"length":4}],"relevance":0.8},"http:\/\/d.opencalais.com\/genericHasher-1\/a54ac18b-a956-3b5c-b235-0609ca899305":{"_typeGroup":"entities","_type":"Country","forenduserdisplay":"true","name":"United States","tempscore":"4","__label":"1","__value":"U.S.","resolutions":[{"name":"United States","shortname":"United States","latitude":"40.4230003233","longitude":"-98.7372244786","rcscode":"G:6J","permid":"100319"}],"_typeReference":"http:\/\/s.opencalais.com\/1\/type\/em\/e\/Country","instances":[{"detection":"[Thursday as stocks dived and oil slumped after ]U.S.[ President Donald Trump took the dramatic step of]","prefix":"Thursday as stocks dived and oil slumped after ","exact":"U.S.","suffix":" President Donald Trump took the dramatic step of","offset":94,"length":4}],"relevance":0.2},"http:\/\/d.opencalais.com\/genericHasher-1\/a5359054-758f-3c20-ac97-d90fbc6a170e":{"_typeGroup":"entities","_type":"Continent","forenduserdisplay":"false","name":"Asia","tempscore":"5","__label":"1","__value":"Asia","_typeReference":"http:\/\/s.opencalais.com\/1\/type\/em\/e\/Continent","instances":[{"detection":"[500 futures ESc1 plummeted as much as 4.9% in ]Asia[ and last traded down 3.6%, a day after the S&P]","prefix":"500 futures ESc1 plummeted as much as 4.9% in ","exact":"Asia","suffix":" and last traded down 3.6%, a day after the S&P","offset":780,"length":4}],"relevance":0.2},"http:\/\/d.opencalais.com\/pershash-1\/3cb6735d-ad40-304e-b561-6fae64c71c32":{"_typeGroup":"entities","_type":"Person","forenduserdisplay":"true","persontype":"N\/A","confidencelevel":"0.828","nationality":"N\/A","lastname":"Trump","name":"Donald Trump","firstname":"Donald","commonname":"Donald Trump","confidence":{"statisticalfeature":"0.509","dblookup":"0.95","resolution":"0.9062719","aggregate":"0.828"},"_typeReference":"http:\/\/s.opencalais.com\/1\/type\/em\/e\/Person","permid":"https:\/\/permid.org\/1-404011","instances":[{"detection":"[dived and oil slumped after U.S. President ]Donald Trump[ took the dramatic step of banning travel from]","prefix":"dived and oil slumped after U.S. President ","exact":"Donald Trump","suffix":" took the dramatic step of banning travel from","offset":109,"length":12},{"detection":"[disappointed by the lack of broad measures in ]Trump['s plan to fight the pathogen, prompting traders]","prefix":"disappointed by the lack of broad measures in ","exact":"Trump","suffix":"'s plan to fight the pathogen, prompting traders","offset":408,"length":5}],"relevance":0.2,"resolutions":[{"name":"Donald John Trump","personid":"55206","paid":"34413176227","officerid":["55206","1309208","650373"],"commonname":"Donald Trump","score":0.9062719}]},"http:\/\/d.opencalais.com\/genericHasher-1\/d920b2cc-12bc-34e1-be7b-066ed7a7cd95":{"_typeGroup":"entities","_type":"Organization","forenduserdisplay":"false","name":"US Federal Reserve","nationality":"N\/A","score":5,"__label":"Processing section","__value":"Federal Reserve","_typeReference":"http:\/\/s.opencalais.com\/1\/type\/em\/e\/Organization","permid":"https:\/\/permid.org\/1-404010","instances":[{"detection":"[to bet of further aggressive easing by the ]Federal Reserve[.\n\nEuro Stoxx 50 futures STXEc1 plunged 8.3% to]","prefix":"to bet of further aggressive easing by the ","exact":"Federal Reserve","suffix":".\n\nEuro Stoxx 50 futures STXEc1 plunged 8.3% to","offset":505,"length":15}],"relevance":0.2},"http:\/\/d.opencalais.com\/comphash-1\/ca1ae707-1c75-3e1a-ba90-74c4ab722247":{"_typeGroup":"entities","_type":"Company","forenduserdisplay":"false","name":"SPX","nationality":"N\/A","score":3,"confidencelevel":"0.799","frompersoncareer":"0","__label":"Processing section","__value":"SPX","recognizedas":"name","confidence":{"statisticalfeature":"0.565","dblookup":"0.0","resolution":"0.98194927","aggregate":"0.799"},"resolutions":[{"name":"SPX CORPORATION","permid":"4295910086","primaryric":"SPXC.N","ispublic":"true","commonname":"SPX","score":0.98194927,"id":"https:\/\/permid.org\/1-4295910086","ticker":"SPXC"}],"_typeReference":"http:\/\/s.opencalais.com\/1\/type\/em\/e\/Company","instances":[{"detection":"[last traded down 3.6%, a day after the S&P 500 .]SPX[ lost 4.89%, leaving the index on the brink of]","prefix":"last traded down 3.6%, a day after the S&P 500 .","exact":"SPX","suffix":" lost 4.89%, leaving the index on the brink of","offset":837,"length":3}],"relevance":0.2},"http:\/\/d.opencalais.com\/genericHasher-1\/09bebb87-1ef5-373d-bd01-263e77470c65":{"_typeGroup":"entities","_type":"Continent","forenduserdisplay":"false","name":"Europe","tempscore":"5","__label":"1","__value":"Europe","_typeReference":"http:\/\/s.opencalais.com\/1\/type\/em\/e\/Continent","instances":[{"detection":"[took the dramatic step of banning travel from ]Europe[ to stem the spread of coronavirus, threatening]","prefix":"took the dramatic step of banning travel from ","exact":"Europe","suffix":" to stem the spread of coronavirus, threatening","offset":168,"length":6}],"relevance":0.2},"http:\/\/d.opencalais.com\/genericHasher-1\/82c86077-a322-3981-8153-75fdaf8dea46":{"_typeGroup":"relations","_type":"PersonTravel","forenduserdisplay":"true","transportation":"arrive","status":"future","__label":"1","__value":"travel from Europe","_typeReference":"http:\/\/s.opencalais.com\/1\/type\/em\/r\/PersonTravel","person":"http:\/\/d.opencalais.com\/pershash-1\/3cb6735d-ad40-304e-b561-6fae64c71c32","locationorigin":"http:\/\/d.opencalais.com\/genericHasher-1\/09bebb87-1ef5-373d-bd01-263e77470c65","instances":[{"detection":"[Donald Trump took the dramatic step of banning ]travel from Europe[ to stem the spread of coronavirus, threatening]","prefix":"Donald Trump took the dramatic step of banning ","exact":"travel from Europe","suffix":" to stem the spread of coronavirus, threatening","offset":156,"length":18}]},"http:\/\/d.opencalais.com\/genericHasher-1\/900fabe8-2a2e-31ad-9c3b-07783631dddf":{"_typeGroup":"relations","_type":"PersonLocation","forenduserdisplay":"false","locationstring":"Europe","__label":"2","__value":"travel from Europe","_typeReference":"http:\/\/s.opencalais.com\/1\/type\/em\/r\/PersonLocation","person":"http:\/\/d.opencalais.com\/pershash-1\/3cb6735d-ad40-304e-b561-6fae64c71c32","instances":[{"detection":"[Donald Trump took the dramatic step of banning ]travel from Europe[ to stem the spread of coronavirus, threatening]","prefix":"Donald Trump took the dramatic step of banning ","exact":"travel from Europe","suffix":" to stem the spread of coronavirus, threatening","offset":156,"length":18}]},"http:\/\/d.opencalais.com\/genericHasher-1\/1385be80-e294-390a-be4d-ce7288e56454":{"_typeGroup":"relations","_type":"PersonCareer","forenduserdisplay":"true","position":"President","careertype":"political","status":"current","__label":"3","__value":"U.S. President Donald Trump","_typeReference":"http:\/\/s.opencalais.com\/1\/type\/em\/r\/PersonCareer","person":"http:\/\/d.opencalais.com\/pershash-1\/3cb6735d-ad40-304e-b561-6fae64c71c32","country":"http:\/\/d.opencalais.com\/genericHasher-1\/a54ac18b-a956-3b5c-b235-0609ca899305","instances":[{"detection":"[Thursday as stocks dived and oil slumped after ]U.S. President Donald Trump[ took the dramatic step of banning travel from]","prefix":"Thursday as stocks dived and oil slumped after ","exact":"U.S. President Donald Trump","suffix":" took the dramatic step of banning travel from","offset":94,"length":27}]}}
    

## 6. Quota

Open PermID APIs have a daily quota limit. There is no API used to get quota information. However, the quota information is available in the HTTP's headers of response messages.
```
   x-permid-quota-daily: 5000
   x-permid-quota-used: 18
```
This library records this quota information and users can retrieve it by calling the following method.
```
get_usage()
```
This method returns a data frame contains the quota information recorded by this library.


```python
opid.get_usage()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Quota Daily</th>
      <th>Quota Used</th>
      <th>Time</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5000</td>
      <td>73</td>
      <td>Mon, 16 Mar 2020 10:30:04 GMT</td>
    </tr>
    <tr>
      <th>1</th>
      <td>5000</td>
      <td>83</td>
      <td>Mon, 16 Mar 2020 10:30:06 GMT</td>
    </tr>
    <tr>
      <th>2</th>
      <td>5000</td>
      <td>84</td>
      <td>Mon, 16 Mar 2020 10:30:27 GMT</td>
    </tr>
    <tr>
      <th>3</th>
      <td>5000</td>
      <td>86</td>
      <td>Mon, 16 Mar 2020 10:31:00 GMT</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5000</td>
      <td>87</td>
      <td>Mon, 16 Mar 2020 10:31:04 GMT</td>
    </tr>
  </tbody>
</table>
</div>



## Summary

Open PermID provides REST APIs to look up, search, match, and tag PermID entities. This example demonstrates how to use a Python Open PermID library. To use this library, you need to have an access token which is freely available when registering at the [PermID](https://permid.org/) website. The library is  easy to use and the source code is available in [GitHub](https://github.com/Refinitiv-API-Samples/Article.OpenPermID.Python.APIs).

For the complete usage guide, please refer to [Python: Open PermID APIs](https://developers.refinitiv.com/article/python-open-permid-apis)

