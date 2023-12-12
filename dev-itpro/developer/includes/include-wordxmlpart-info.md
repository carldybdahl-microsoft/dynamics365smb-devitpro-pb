This method returns the data structure of the report as structured XML that complies with custom XML parts in Microsoft Word. The following table provides a simplified overview of the XML that is returned by the method.  

|XML|Description|  
|---------|-----------------|  
|`<?xml version="1.0" encoding="utf-16"?>`|Header|  
|`<NavWordReportXmlPart xmlns="urn:microsoft-../report/<reportname>/<id>/"`|XML namespace specification. `<reportname>` is the name assigned to the report object. `<id>` is the ID that is assigned to the report.|  
|`..<Labels>`<br /><br /> `....<ColumnNameCaption>ColumnNameCaption</ColumnNameCaption>`<br /><br /> `....<LabelName>LabelCaption</LabelName>`<br /><br /> `..</Labels>`|Contains all the labels for the report. Labels are listed in alphabetical. The element includes labels that are related to columns that have the [IncludeCaption Property](../properties/devenv-includecaption-property.md) set to **Yes** and labels that are defined in the labels section of the report.<br /><br /> -   Label elements that are related to columns have the format `<ColumnNameCaption>ColumnNameCaption</ColumnNameCaption>`, where `ColumnName` is determined by the column's [Name Property](../properties/devenv-properties.md).<br />-   Label elements from the labels section of the report have the format `<LabelName>LabelCaption</LableName`, where `LabelName` is determined by the label's [Name Property](../properties/devenv-properties.md) and `LabelCaption` is determined by the label's [Caption Property](../properties/devenv-caption-property.md).|  
|`..<DataItem1>`<br /><br /> `....<DataItem1Column1>DataItem1Column1</DataItem1Column1>`|Top-level data item and columns. Columns are listed in alphabetical order.<br /><br /> The element names and values are determined by the Name property of the data item or column.|  
|`....<DataItem2>`<br /><br /> `......<DataItem2Column1>DataItem2Column1</DataItem2Column1>`<br /><br /> `....</DataItem2>`<br /><br /> `....<DataItem3>`<br /><br /> `......<DataItem3Column1>DataItem3Column1</DataItem3Column1>`<br /><br /> `....</DataItem3>`|Data items and columns that are nested in the top-level data item. Columns are listed in alphabetical order under the respective data item.|  
|`..</DataItem1>`<br /><br /> `</NavWordReportXmlPart>`|Closing elements.|  


Word custom XML parts enable you to integrate business data into Word documents. For example, the WordXMLPart method is used internally by [!INCLUDE[prod_short](./prod_short.md)] when you are creating report layouts in Word. You can use this method to create a custom XML part, and then, together with the [SaveAsXML Method \(Reports\)](../methods-auto/report/reportinstance-saveasxml-method.md) method and additional data merging tools, you can implement your own functionality for mapping and laying out report data in Word documents. To create a custom XML part, you can save the return value to an .xml file that is encoded in UTF-16 \(16-bit Unicode Transformation Format\). The resultant file can be added to Word documents as a custom XML part to map the report data set as XML data.  