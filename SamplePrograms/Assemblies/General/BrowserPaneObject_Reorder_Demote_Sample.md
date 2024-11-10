# Demote occurence

## Description
This sample demonstrates how to demote a top level occurrence in an assembly into a new sub-assembly occurrence.

## Code Samples 

### iLogic
```vb
' Get the active assembly document
Dim doc As AssemblyDocument = ThisDoc.Document

Dim def As AssemblyComponentDefinition = doc.ComponentDefinition

' Get the occurrence to be demoted
Dim occ As ComponentOccurrence = def.Occurrences.Item(1)

' Create a new sub-assembly to demote the occurrence into
Dim newSubAssy As AssemblyDocument = ThisApplication.Documents.Add(DocumentTypeEnum.kAssemblyDocumentObject, , False)

Dim matrix As Matrix = ThisApplication.TransientGeometry.CreateMatrix

' Create an instance of the new sub-assembly
Dim subAssyOcc As ComponentOccurrence = def.Occurrences.AddByComponentDefinition(newSubAssy.ComponentDefinition, matrix)

' Get the model browser
Dim pane As BrowserPane = doc.BrowserPanes.Item("Model")

' Get the browser node that corresponds to the new sub-assembly occurrence
Dim subAssyNode As BrowserNode = pane.GetBrowserNodeFromObject(subAssyOcc)

' Get the last visible child node under the sub-assembly occurrence
Dim targetNode As BrowserNode
For i As Long = subAssyNode.BrowserNodes.Count To 1 Step -1
    If (subAssyNode.BrowserNodes.Item(i).Visible) Then
        targetNode = subAssyNode.BrowserNodes.Item(i)
        Exit For
    End If
Next

' Get the browser node that corresponds to the occurrence to be demoted
Dim oSourceNode As BrowserNode = pane.GetBrowserNodeFromObject(occ)

' Demote the occurrence
pane.Reorder(targetNode, False, oSourceNode)
```
[Official Autodesk help page](https://help.autodesk.com/view/INVNTOR/2025/ENU/?guid=BrowserPaneObject_Reorder_Demote_Sample)