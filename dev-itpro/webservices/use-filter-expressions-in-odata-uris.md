---
title: Using Filter Expressions in OData URIs
description: Learn how to use filter expressions in OData URIs to limit the results returned in a document.
author: jswymer
ms.author: jswymer
ms.custom: bap-template
ms.date: 01/28/2024
ms.reviewer: jswymer
ms.topic: conceptual
---
# Using filter expressions in OData URIs

You use filter expressions in OData URIs to limit the results in a returned document. This article lists the filter expressions, and describes the equivalent field or table filter in AL. It provides examples of syntax for using filter expressions in OData URIs and applications.  

## Filter expressions  

 To add a filter to an OData URI, add `$filter=` to the end of the name of the published web service. For example, the following URI filters the **City** field in the **Customer** page to return all customers who are located in Miami:  

```  
https://localhost:7048/BC240/ODataV4/Company('CRONUS International Ltd.')/Customer?$filter=City eq 'Miami'  
```  

The following table shows the filters that are supported in [!INCLUDE[prod_short](../developer/includes/prod_short.md)] OData web services and their equivalent AL filter expressions. All examples are based either on page 21, Customer \(published as **Customer**\), or on page 20, General Ledger Entry \(published as **GLEntry**\).  

|Definition|Example and explanation|Equivalent AL expression|  
|----------------|-----------------------------|---------------------------------|  
|Select a range of values|`$filter=Entry_No gt 610 and Entry_No lt 615`<br /><br /> Query on GLEntry service. Returns entry numbers 611 through 614.|..|
|In a list of values (schemaversion 2.1 only)|`$filter=EntryNo in (610, 612, 614)`<br /><br /> Query that returns entry numbers 610, 612, and 614<br /><br />Note: this requires adding `$schemaversion=2.1` to the OData query.||
|And|`$filter=Country_Region_Code eq 'ES' and Payment_Terms_Code eq '14 DAYS'`<br /><br /> Query on Customer service. Returns customers in Spain where Payment\_Terms\_Code=**14 DAYS**.|&|  
|Or|`$filter= Country_Region_Code eq 'ES' or Country_Region_Code eq 'US'`<br /><br /> Query on Customer service. Returns customers in Spain and the United States.<br /><br /> **Alert:** You can use OR operators to apply different filters on the same field. However, you can't use OR operators to apply filters on two different fields.|&#124;|  
|Less than|`$filter=Entry_No lt 610`<br /><br /> Query on GLEntry service. Returns entry numbers that are less than 610.|\<|  
|Greater than|`$filter= Entry_No gt 610`<br /><br /> Query on GLEntry service. Returns entry numbers 611 and higher.|>|  
|Greater than or equal to|`$filter=Entry_No ge 610`<br /><br /> Query on GLEntry service. Returns entry numbers 610 and higher.|>=|  
|Less than or equal to|`$filter=Entry_No le 610`<br /><br /> Query on GLEntry service. Returns entry numbers up to and including 610.|\<=|  
|Different from \(not equal\)|`$filter=VAT_Bus_Posting_Group ne 'EXPORT'`<br /><br /> Query on Customer service. Returns all customers with VAT\_Bus\_Posting\_Group not equal to EXPORT.|\<>|  
|endswith|`$filter=endswith(VAT_Bus_Posting_Group,'RT')`<br /><br /> Query on Customer service. Returns all customers with VAT\_Bus\_Posting\_Group values that end in 'RT'.|\*|  
|startswith|`$filter=startswith(Name, 'S')`<br /><br /> Query on Customer service. Returns all customers names beginning with 'S'.|\*|  
|contains|`$filter=contains(Name, 'urn')`<br /><br /> Query on Customer service. Returns customer records for customers with names containing the string “urn”.||  
|substring|`$filter=substring(Location_Code, 5) eq 'RED'`<br /><br /> Query on Customer service. Returns true for customers with the string RED in their location code starting as position 5.||  
|tolower|`$filter=tolower(Location_Code) eq 'code red'`||  
|toupper|`$filter=toupper(FText) eq '2ND ROW'`||  

>[!Note]
> Generally the filters become more flexible and more robust in `$schemaversion=2.1`.

<!--
|indexof|`$filter=indexof(Location_Code, 'BLUE') eq 0`<br /><br /> Query on Customer service. Returns customer records for customers having a location code beginning with the string BLUE.||
|replace|`$filter=replace(City, 'Miami', 'Tampa') eq 'CODERED'`||
|concat|`$filter=concat(concat(FText, ', '), FCode) eq '2nd row, CODE RED'`||  
|round|`$filter=round(FDecimal) eq 1`||  
|floor|`$filter=floor(FDecimal) eq 0`||  
|ceiling|`$filter=ceiling(FDecimal) eq 1`|| 
|trim|`$filter=trim(FCode) eq 'CODE RED'`||       -->

>[!Note]
> There is a special filter, `journals.templateDisplayName` which returns default journals if a user hasn't defined the filter criteria.

> [!NOTE]  
> You can learn more about setting the filters that are specific to AL language by checking out [Enter criteria in Filters](../developer/devenv-entering-criteria-in-filters.md) article.

### Filters without equivalent AL expressions

For filters that don't have equivalent AL expressions, the platform uses the closest available AL approximation. For example, `tolower` and `toupper` disable case sensitivity in comparisons, and `substring` gets turned into `*` wild card operators.

If there's no AL approximation, an error is reported. For example, an error can occur if you perform `Or` over different fields, like `field1 eq 1 or field2 eq 2`.

## Referencing different data types in Filter expressions

Use the appropriate notation for different data types with filter expressions.  

- Delimit string values by single quotation marks.  

- Numeric values require no delimiters.

## Nested function calls

Nested function calls in filter clauses are supported in `$schemaversion=2.1` and later. Nested function calls aren't supported in earlier schema versions, which means that filter clause expressions like `contains(tolower(field), 'some')` don't return the expected results. In this case, a partial case-insensitive text search - but instead either throws an error or returns an undefined result.

## See also

[OData Web Services](OData-Web-Services.md)  
[Microsoft OData Docs - Query options overview](/odata/concepts/queryoptions-overview)  
