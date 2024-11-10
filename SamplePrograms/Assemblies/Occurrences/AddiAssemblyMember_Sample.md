# Adding iAssembly occurrences

## Description
This sample demonstrates adding iAssembly occurrences to an assembly.

## Code Samples 
Before running the sample, make sure that C:\temp\iAssemblyFactory.iam exists and that it is an iAssembly factory.

### iLogic
```vb
' Open the factory document invisible.
Dim factoryDoc As AssemblyDocument = ThisApplication.Documents.Open("C:\temp\iAssemblyFactory.iam", False)

' Set a reference to the component definition.
Dim compDef As AssemblyComponentDefinition = factoryDoc.ComponentDefinition

' Make sure we have an iAssembly factory.
If (compDef.IsiAssemblyFactory = False) Then
    MsgBox("Chosen document is not a factory.", MsgBoxStyle.Exclamation)
    Exit Sub
End If

' Set a reference to the factory.
Dim iAssyFactory As iAssemblyFactory = compDef.iAssemblyFactory

' Get the number of rows in the factory.
Dim numRows As Integer = iAssyFactory.TableRows.Count

' Create a new assembly document
Dim doc As AssemblyDocument = ThisApplication.Documents.Add(DocumentTypeEnum.kAssemblyDocumentObject, , True)

Dim occs As ComponentOccurrences = doc.ComponentDefinition.Occurrences

Dim pos As Matrix = ThisApplication.TransientGeometry.CreateMatrix

Dim stepDistance As Double = 0#

' Add an occurrence for each member in the factory.
For iRow As Long = 1 To numRows

    stepDistance = stepDistance + 10

    ' Add a translation along X axis
    pos.SetTranslation(ThisApplication.TransientGeometry.CreateVector(stepDistance, stepDistance, 0))

    occs.AddiAssemblyMember("C:\temp\iAssemblyFactory.iam ", pos, iRow)
Next
```
[Official Autodesk help page](https://help.autodesk.com/view/INVNTOR/2025/ENU/?guid=AddiAssemblyMember_Sample)