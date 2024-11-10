# Adding iPart occurrences to an assembly

## Description
This sample demonstrates adding iPart occurrences to an assembly.

## Code Samples 
Before running the sample, make sure that C:\temp\iPartFactory.ipt exists and that it is an iPart factory.

### iLogic
```vb
' Open the factory document invisible.
Dim factoryDoc As PartDocument = ThisApplication.Documents.Open("C:\temp\iPartFactory.ipt", False)

' Set a reference to the component definition.
Dim compDef As PartComponentDefinition = factoryDoc.ComponentDefinition

' Make sure we have an iPart factory.
If (compDef.IsiPartFactory = False) Then
    MsgBox("Chosen document is not a factory.", MsgBoxStyle.Exclamation)
    Exit Sub
End If

' Set a reference to the factory.
Dim iPartFactory As iPartFactory = compDef.iPartFactory

' Get the number of rows in the factory.
Dim numRows As Integer = iPartFactory.TableRows.Count

' Create a new assembly document
Dim doc As AssemblyDocument = ThisApplication.Documents.Add(DocumentTypeEnum.kAssemblyDocumentObject, , True)
Dim occs As ComponentOccurrences = doc.ComponentDefinition.Occurrences

Dim pos As Matrix = ThisApplication.TransientGeometry.CreateMatrix()
Dim stepDistance As Double = 0#

' Add an occurrence for each member in the factory.
For iRow As Long = 1 To numRows

    stepDistance = stepDistance + 10

    ' Add a translation along X axis
    pos.SetTranslation(ThisApplication.TransientGeometry.CreateVector(stepDistance, stepDistance, 0))

    Dim occ As ComponentOccurrence = occs.AddiPartMember("C:\temp\iPartFactory.ipt ", pos, iRow)
Next
```
[Official Autodesk help page](https://help.autodesk.com/view/INVNTOR/2025/ENU/?guid=AddiPartMember_Sample)