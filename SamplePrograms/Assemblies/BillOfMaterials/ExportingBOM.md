# Using the BOM APIs

## Description
This sample demonstrates exporting the Assembly BOM to an external file.

## Code Samples 
This sample exports the structured and parts only views of the active assembly document to an Excel file. Other file formats are also supported.

### iLogic
```vb
' Set a reference to the assembly document.
' This assumes an assembly document is active.
Dim doc As AssemblyDocument = ThisDoc.Document

' Set a reference to the BOM
Dim bom As BOM = doc.ComponentDefinition.BOM

' Set the structured view to 'all levels'
bom.StructuredViewFirstLevelOnly = False

' Make sure that the structured view is enabled.
bom.StructuredViewEnabled = True

' Set a reference to the "Structured" BOMView
Dim structuredBOMView As BOMView = bom.BOMViews.Item("Structured")

' Export the BOM view to an Excel file
structuredBOMView.Export("C:\temp\BOM-StructuredAllLevels.xls", FileFormatEnum.kMicrosoftExcelFormat)

' Make sure that the parts only view is enabled.
bom.PartsOnlyViewEnabled = True

' Set a reference to the "Parts Only" BOMView
Dim partsOnlyBOMView As BOMView = bom.BOMViews.Item("Parts Only")

' Export the BOM view to an Excel file
partsOnlyBOMView.Export("C:\temp\BOM-PartsOnly.xls", FileFormatEnum.kMicrosoftExcelFormat)
```
[Official Autodesk help page](https://help.autodesk.com/view/INVNTOR/2025/ENU/?guid=BOMView_Export_Sample)