# Add assembly insert constraint

## Description
This sample demonstrates the creation of an assembly insert constraint.

## Code Samples 
Before running the sample, you need to open an assembly that contains at least two parts. Select circular edges on the two parts that will be used for the constraint and run the sample code. (Set the priority of the Select command and use the Shift-Select to select multiple edges.)

### iLogic
```vb
' Set a reference to the assembly component definintion.
Dim asmCompDef As AssemblyComponentDefinition = ThisDoc.Document.ComponentDefinition

' Set a reference to the select set.
Dim selectSet As SelectSet = ThisApplication.ActiveDocument.SelectSet

' Validate the correct data is in the select set.
If selectSet.Count <> 2 Then
    MsgBox("You must select the two circular edges for the insert.")
    Exit Sub
End If

If Not TypeOf selectSet.Item(1) Is Edge Or Not TypeOf selectSet.Item(2) Is Edge Then
    MsgBox("You must select the two circular edges for the insert.")
    Exit Sub
End If

' Get the two edges from the select set.
Dim edge1 As Edge = selectSet.Item(1)
Dim edge2 As Edge = selectSet.Item(2)

' Create the insert constraint between the parts.
asmCompDef.Constraints.AddInsertConstraint(edge1, edge2, True, 0)
```
[Official Autodesk help page](https://help.autodesk.com/view/INVNTOR/2025/ENU/?guid=AssemblyConstraints_AddInsertConstraint_Sample)