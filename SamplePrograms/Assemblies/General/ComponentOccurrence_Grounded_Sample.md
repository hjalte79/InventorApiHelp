# Assembly Ground Occurrences

## Description
This sample demonstrates grounding an assembly occurrence.

## Code Samples 
Before running the sample, you need to open an assembly and create a part file called C:\TempPart1.ipt, or edit the sample code to point to another part file if desired.

*(Remarks: This text, from the official page, seems not to belong to this code. You just need an open assembly with atleast 1 occurence.)*

### iLogic
```vb
' Set a reference to the assembly component definintion.
' This assumes an assembly document is open.
Dim asmCompDef As AssemblyComponentDefinition = ThisApplication.ActiveDocument.ComponentDefinition

' Ask whether to delete or suppress the existing constraints.
Dim delete As Boolean
If MsgBox("Do you want to delete all existing constraints?", vbYesNo + vbQuestion) = vbYes Then
    delete = True
Else
    delete = False
End If

' Iterate through all of the constraints and perform the specified operation.
For Each constraint As AssemblyConstraint In asmCompDef.Constraints
    If delete Then
        constraint.Delete()
    Else
        constraint.Suppressed = True
    End If
Next

' Iterate through all of the occurrences and ground them.
For Each occurrence As ComponentOccurrence In asmCompDef.Occurrences
    occurrence.Grounded = True
Next
```
[Official Autodesk help page](https://help.autodesk.com/view/INVNTOR/2025/ENU/?guid=ComponentOccurrence_Grounded_Sample)