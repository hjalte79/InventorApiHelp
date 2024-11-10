# Promote occurence

## Description
This sample demonstrates how to promote an occurrence.

## Code Samples 
*(Remarks: Missing text here on official page. You need an open assmebly and the first occurence needs to be a assembly.)*

### iLogic
```vb
' Get the active assembly document
Dim doc As AssemblyDocument = ThisDoc.Document

Dim def As AssemblyComponentDefinition = doc.ComponentDefinition

' Get the top level occurrence of an assembly
Dim subAssyOcc As ComponentOccurrence = def.Occurrences.Item(1)

' Get the 2nd level occurrence under the assembly occurrence
Dim subOcc As ComponentOccurrenceProxy = def.Occurrences.Item(1).SubOccurrences.Item(1)

Dim pane As BrowserPane = doc.BrowserPanes.Item("Model")

' Get the browser nodes corresponding to the two occurrences
Dim targetNode As BrowserNode = pane.GetBrowserNodeFromObject(subAssyOcc)

Dim sourceNode As BrowserNode = pane.GetBrowserNodeFromObject(subOcc)

' Reorder the nodes to promote the sub-occurrence to the top level
pane.Reorder(targetNode, True, sourceNode)
```
[Official Autodesk help page](https://help.autodesk.com/view/INVNTOR/2025/ENU/?guid=BrowserPaneObject_Reorder_Promote_Sample)