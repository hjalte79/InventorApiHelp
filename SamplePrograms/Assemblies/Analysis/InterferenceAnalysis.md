# Interference Analysis

## Description 
This sample demonstrates the functions used to calculate interference analysis in an assembly.

## Code Samples 
To use this sample have an assembly open that contains mutiple parts. Depending on preselected parts when running the sample, you'll get different results. If one part is selected, that one part will be checked against the rest of the assembly. If more than one part is selected you have the choice of checking for interference among the selected parts or checking the selected parts against the rest of the assembly. If no parts are selected it will check every part against every other part.

### iLogic
```vb
Dim doc As AssemblyDocument = ThisDoc.Document

' Find all selected occurrences and add them to an ObjectCollection.
Dim selectedOccs As ObjectCollection = ThisApplication.TransientObjects.CreateObjectCollection
For Each item As Object In doc.SelectSet
    If item.Type = ObjectTypeEnum.kComponentOccurrenceObject Then
        selectedOccs.Add(item)
    End If
Next

' If no occurrences are selected check for interference of
' all parts against all parts.  If one occurrence is selected, check
' for interference between that occurrence and the rest of the assembly.
' If more than one occurrence is selected let the user decide if it
' should check for interference between the parts in the selection or
' between the selected parts and the rest of the assembly.
Dim results As InterferenceResults
Dim checkSet As ObjectCollection = ThisApplication.TransientObjects.CreateObjectCollection
If selectedOccs.Count = 0 Then
    ' Add all occurrences to the object collection
    For Each occ As ComponentOccurrence In doc.ComponentDefinition.Occurrences
        checkSet.Add(occ)
    Next

    ' Get the interference between everything.
    results = doc.ComponentDefinition.AnalyzeInterference(checkSet)
ElseIf selectedOccs.Count = 1 Then
    ' Add all occurrences except the selected occurrence to the object collection.
    For Each oOcc In doc.ComponentDefinition.Occurrences
        If Not oOcc Is selectedOccs.Item(1) Then
            checkSet.Add(oOcc)
        End If
    Next

    ' Get the interference between the selected occurrence everything else.
    results = doc.ComponentDefinition.AnalyzeInterference(selectedOccs, checkSet)
Else
    If MsgBox("Check interference between selected occurrences and all other occurrences?", vbYesNo + vbQuestion) = vbYes Then
        ' Add all occurrences except the selected occurrences to the object collection.
        For Each occ In doc.ComponentDefinition.Occurrences
            ' Check to see if this occurrences is already selected.
            Dim selected As Boolean = False
            For i = 1 To selectedOccs.Count
                If selectedOccs.Item(i) Is occ Then
                    selected = True
                    Exit For
                End If
            Next

            If Not selected Then
                checkSet.Add(occ)
            End If
        Next

        ' Check interference between the selected items.
        results = doc.ComponentDefinition.AnalyzeInterference(selectedOccs, checkSet)
    Else
        ' Check interference between the selected items.
        results = doc.ComponentDefinition.AnalyzeInterference(selectedOccs)
    End If
End If

If results.Count = 1 Then
    MsgBox("There is 1 interference.")
ElseIf results.Count > 1 Then
    MsgBox("There are " & results.Count & " interferences.")
End If

If results.Count > 0 Then
    Dim oHS1 As HighlightSet = doc.HighlightSets.Add()
    oHS1.Color = ThisApplication.TransientObjects.CreateColor(255, 0, 0)
    Dim oHS2 As HighlightSet = doc.HighlightSets.Add()
    oHS2.Color = ThisApplication.TransientObjects.CreateColor(0, 255, 0)

    For i = 1 To results.Count
        oHS1.Clear()
        oHS2.Clear()
        oHS1.AddItem(results.Item(i).OccurrenceOne)
        oHS2.AddItem(results.Item(i).OccurrenceTwo)
        MsgBox("Occurrences are highlighted from interference " & i)
    Next

    oHS1.Clear()
    oHS2.Clear()
Else
    MsgBox("There is no interference.")
End If
```

[Official Autodesk help page](https://help.autodesk.com/view/INVNTOR/2025/ENU/?guid=GUID-3C33DEB3-4441-4AB9-892A-262084072036)
