# Add assembly occurrences to a new folder

## Description
Demonstrates assembly occurrences to a new folder.

## Code Samples 
Have an assembly with at least one occurrence in it and run the sample.

### iLogic
```vb
Dim doc As AssemblyDocument = ThisDoc.Document

Dim def As AssemblyComponentDefinition = doc.ComponentDefinition

Dim pane As BrowserPane = doc.BrowserPanes.ActivePane

Dim occurrenceNodes As ObjectCollection = ThisApplication.TransientObjects.CreateObjectCollection()

For Each occ As ComponentOccurrence In def.Occurrences

    Dim oNode As BrowserNode = pane.GetBrowserNodeFromObject(occ)
    occurrenceNodes.Add(oNode)

Next

pane.AddBrowserFolder("My Occurrence Folder", occurrenceNodes)
```
[Official Autodesk help page](https://help.autodesk.com/view/INVNTOR/2025/ENU/?guid=BrowserPaneObject_AddBrowserFolder_Sample)