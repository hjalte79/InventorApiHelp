# iMate Result Creation

## Description
This sample demonstrates creating an iMate result using two existin iMate definitions.

## Code Samples 
To use this sample create a new part by extruding a rectangle to create a cube. Create a mate iMate on one of the faces. This sample assumes the iMate is named the default name used in the English version of Inventor, which is iMate:1. If the iMate definition is created with another name you can either edit the name of the iMate definition in the part file, or edit the sample code below to use the different name. Save the part to C:\Temp\iMatePart.ipt. Finally, have an assembly open and run the sample code.

### iLogic
```vb
' Get the component definition of the currently open assembly.
' This will fail if an assembly document is not open.
Dim asmCompDef As AssemblyComponentDefinition = ThisDoc.Document.ComponentDefinition

' Create a new matrix object.  It will be initialized to an identity matrix.
Dim matrix As Matrix = ThisApplication.TransientGeometry.CreateMatrix

' Place the first occurrence.
Dim occ1 As ComponentOccurrence = asmCompDef.Occurrences.Add("C:\Temp\iMatePart.ipt", matrix)

' Place the second occurrence, but adjust the matrix slightly so they're
' not right on top of each other.
matrix.Cell(1, 4) = 10
Dim occ2 As ComponentOccurrence = asmCompDef.Occurrences.Add("C:\Temp\iMatePart.ipt", matrix)

' Look through the iMateDefinitions defined for the first occurrence
' and find the one named "iMate:1".  This loop demonstrates using the
' Count and Item properties of the iMateDefinitions object.
Dim iMateDef1 As iMateDefinition
For i As Long = 1 To occ1.iMateDefinitions.Count
    If (occ1.iMateDefinitions.Item(i).Name = "iMate:1") Then
        iMateDef1 = occ1.iMateDefinitions.Item(i)
        Exit For
    End If
Next

If (iMateDef1 Is Nothing) Then
    MsgBox("An iMate definition named ""iMate:1"" does not exist in " & occ1.Name)
    Exit Sub
End If

' Look through the iMateDefinitions defined for the second occurrence
' and find the one named "iMate:1".  This loop demonstrates using the
' For Each method of iterating through a collection.
Dim foundDefinition As Boolean
Dim iMateDef2 As iMateDefinition
For Each iMateDef2 In occ2.iMateDefinitions
    If (iMateDef2.Name = "iMate:1") Then
        foundDefinition = True
        Exit For
    End If
Next

If (Not foundDefinition) Then
    MsgBox("An iMate definition named ""iMate:1"" does not exist in " & occ2.Name)
    Exit Sub
End If

' Create an iMate result using the two definitions.
asmCompDef.iMateResults.AddByTwoiMates(iMateDef1, iMateDef2)
```
[Official Autodesk help page](https://help.autodesk.com/view/INVNTOR/2025/ENU/?guid=iMateResult_Sample)