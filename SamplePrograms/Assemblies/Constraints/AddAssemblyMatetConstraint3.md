# Add mate constraint with limits

## Description
This sample demonstrates the creation of an assembly mate constraint with maximum and minimum limits defined.

## Code Samples 

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

' Create the mate constraint between the parts, with an offset value of 0.
Dim mate As MateConstraint = asmCompDef.Constraints.AddMateConstraint(entity1, entity2, 0)

' Set a maximum value of 2 inches
mate.ConstraintLimits.MaximumEnabled = True
mate.ConstraintLimits.Maximum.Expression = "25 mm"

' Set a minimum value of -2 inches
mate.ConstraintLimits.MinimumEnabled = True
mate.ConstraintLimits.Minimum.Expression = "-25 mm"
```
[Official Autodesk help page](https://help.autodesk.com/view/INVNTOR/2025/ENU/?guid=AssemblyConstraints_AddMateConstraint3_Sample)