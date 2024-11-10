# Create Revit Export sample

## Description
This sample demonstrates how to create a RevitExport object.

## Code Samples 
This sample demonstrates how to create a RevitExport object. Open an assembly firstly before running this sample.

### iLogic
```vb
Dim doc As AssemblyDocument = ThisDoc.Document
Dim modelStates As ModelStates = doc.ComponentDefinition.ModelStates

' Actiate the Master model state if the active model state is substitute.
If modelStates.ActiveModelState.ModelStateType = ModelStateTypeEnum.kSubstituteModelStateType Then
    modelStates.Item(1).Activate()
    doc = ThisApplication.ActiveDocument
End If

Dim revitExportDef As RevitExportDefinition = doc.ComponentDefinition.RevitExports.CreateDefinition

revitExportDef.Location = "C:\Temp"
revitExportDef.FileName = "MyRevitExport.rvt"
revitExportDef.Structure = RevitExportStructureTypeEnum.kEachTopLevelComponentStructure
revitExportDef.EnableUpdating = True

' Create RevitExport.
Dim revitExport As RevitExport = doc.ComponentDefinition.RevitExports.Add(revitExportDef)
```
[Official Autodesk help page](https://help.autodesk.com/view/INVNTOR/2025/ENU/?guid=CreateRevitExportSample_Sample)