# Assembly Add Occurrence

## Description
This sample demonstrates placing an assembly occurrence.

## Code Samples 
Before running the sample, you need to open an assembly and create a part file called C:\Temp\Part1.ipt, or edit the sample code to point to another part file if desired.

### iLogic
```vb
' Set a reference to the assembly component definintion.
' This assumes an assembly document is open.
Dim asmCompDef As AssemblyComponentDefinition = ThisDoc.Document.ComponentDefinition

' Set a reference to the transient geometry object.
Dim oTG As TransientGeometry = ThisApplication.TransientGeometry

' Create a matrix.  A new matrix is initialized with an identity matrix.
Dim matrix As Matrix = oTG.CreateMatrix()

' Set the rotation of the matrix for a 45 degree rotation about the Z axis.
matrix.SetToRotation(Math.PI / 4, oTG.CreateVector(0, 0, 1), oTG.CreatePoint(0, 0, 0))

' Set the translation portion of the matrix so the part will be positioned
' at (3,2,1).
matrix.SetTranslation(oTG.CreateVector(3, 2, 1))

' Add the occurrence.
Dim occ As ComponentOccurrence = asmCompDef.Occurrences.Add("C:\Temp\Part1.ipt", matrix)
```
[Official Autodesk help page](https://help.autodesk.com/view/INVNTOR/2025/ENU/?guid=AddOccurrence_Sample)