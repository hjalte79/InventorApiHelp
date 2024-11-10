# Assembly Move Occurrence

## Description
This sample demonstrates moving a component occurrence. This sample performs a translate, but a rotate can also be performed since the transform is defined using a matrix.

## Code Samples 
Before running the sample you need to open an assembly and select the occurrence to move. The sample code first moves the occurrence honoring any existing constraints and then moves it ignoring any constraints.

### iLogic
```vb
' Set a reference to the assembly component definintion.
Dim asmCompDef As AssemblyComponentDefinition = ThisDoc.Document.ComponentDefinition

' Get an occurrence from the select set.
Dim occurrence As ComponentOccurrence
Try
    occurrence = ThisApplication.ActiveDocument.SelectSet.Item(1)
Catch ex As Exception
    MsgBox("An occurrence must be selected.")
    Exit Sub
End Try

' Get the current transformation matrix from the occurrence.
Dim transform As Matrix = occurrence.Transformation

' Move the occurrence honoring any existing constraints.
transform.SetTranslation(ThisApplication.TransientGeometry.CreateVector(2, 2, 3))
occurrence.Transformation = transform

' Move the occurrence ignoring any constraints.
' Anything that causes the assembly to recompute will cause the
' occurrence to reposition itself to honor the constraints.
transform.SetTranslation(ThisApplication.TransientGeometry.CreateVector(3, 4, 5))
occurrence.SetTransformWithoutConstraints(transform)
```
[Official Autodesk help page](https://help.autodesk.com/view/INVNTOR/2025/ENU/?guid=AssemblyConstraints_AddInsertConstraint_Sample)