---
title: "OnValidate (Fields) Trigger"
description: "OnValidate trigger for fields in AL for Business Central."
ms.date: 04/01/2021
ms.topic: reference
author: SusanneWindfeldPedersen
---

# OnValidate (Fields) Trigger

Runs when user input is validated.  

## Syntax

```AL
trigger OnValidate()
begin
    ...
end;
```    

## Applies to

- Fields  
  
## Remarks  

This trigger is run after the default validation behavior when data is entered in a field. During the default validation behavior, the system checks that the data type of the value entered matches the one defined for the field and that it complies with the property constraints set up in such field before the validation occurs. An error message displays if an error occurs in the trigger code. In case of an error, the user entry is not written to the database.  

The OnValidate trigger is also a field trigger at the page level. For more information, see [OnValidate (Page Fields) Trigger](devenv-onvalidate-page-fields-trigger.md). If both the table field and page field triggers are defined, then the OnValidate trigger on the table field is run before the OnValidate trigger on the page field.  

## Example

```AL
tableextension 50111 "CustomerExt" extends Customer
{
    fields
    {
        field(50112; Acronym; Text[15])
        {
            trigger OnValidate()
            begin
                rec.Acronym := rec.Acronym.ToUpper();
            end;
        }
    }
}
```
  
## See Also  

[Triggers](devenv-triggers.md)  
[Table and Field Triggers](devenv-table-and-field-triggers.md)  
[Table Properties](../properties/devenv-table-properties.md)    