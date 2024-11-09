# Add assembly mate constraint

## Description
This sample demonstrates the creation of an assembly mate constraint.

## Code Samples 
Before running the sample, you need to open an assembly that contains at least two parts. Select planar faces on the two parts that will be used for the constraint and run the sample code. (Set the priority of the Select command and use the Shift-Select to select multiple faces.)

### iLogic
```vb
' Set a reference to the assembly component definintion.
Dim asmCompDef As AssemblyComponentDefinition = ThisDoc.Document.ComponentDefinition

' Set a reference to the select set.
Dim selectSet As SelectSet = ThisApplication.ActiveDocument.SelectSet

' Validate the correct data is in the select set.
If selectSet.Count <> 2 Then
    MsgBox("You must select the two entities valid for mate.")
    Exit Sub
End If

' Get the two entities from the select set.
Dim entity1 As Object = selectSet.Item(1)
Dim entity2 As Object = selectSet.Item(2)

' Create the insert constraint between the parts.
asmCompDef.Constraints.AddMateConstraint(entity1, entity2, 0)
```
[Official Autodesk help page](https://help.autodesk.com/view/INVNTOR/2025/ENU/?guid=AssemblyConstraints_AddMateConstraint_Sample)