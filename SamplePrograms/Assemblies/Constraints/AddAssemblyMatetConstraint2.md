# Add mate constraint using work planes in parts

## Description
This sample demonstrates creating a mate constraint between two occurrences using the work planes within those occurrences.

## Code Samples 
To use the sample, have an assembly open that contains at least two occurrences, (part or subassembly), and run the program.

### iLogic
```vb
' Set a reference to the assembly component definintion.
Dim asmCompDef As AssemblyComponentDefinition = ThisDoc.Document.ComponentDefinition

' Get references to the two occurrences to constrain.
' This arbitrarily gets the first and second occurrence.
Dim occ1 As ComponentOccurrence = asmCompDef.Occurrences.Item(1)
Dim occ2 As ComponentOccurrence = asmCompDef.Occurrences.Item(2)

' Get the XY plane from each occurrence.  This goes to the
' component definition of the part to get this information.
' This is the same as accessing the part document directly.
' The work plane obtained is in the context of the part,
' not the assembly.
Dim partPlane1 As WorkPlane = occ1.Definition.WorkPlanes.Item(3)
Dim partPlane2 As WorkPlane = occ2.Definition.WorkPlanes.Item(3)

' Because we need the work plane in the context of the assembly
' we need to create proxies for the work planes.  The proxies
' represent the work planes in the context of the assembly.
Dim asmPlane1 As WorkPlaneProxy
occ1.CreateGeometryProxy(partPlane1, asmPlane1)

Dim asmPlane2 As WorkPlaneProxy
occ2.CreateGeometryProxy(partPlane2, asmPlane2)

' Create the constraint using the work plane proxies.
asmCompDef.Constraints.AddMateConstraint(asmPlane1, asmPlane2, 0)
```
[Official Autodesk help page](https://help.autodesk.com/view/INVNTOR/2025/ENU/?guid=AssemblyConstraints_AddMateConstraint2_Sample)