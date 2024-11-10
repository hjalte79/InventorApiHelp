# iMate Creation During Occurrence Placement

## Description
This sample demonstrates creating multiple iMate results when adding an occurrence into an assembly. This uses the AddUsingiMate method which is the equivalent of using the Place Component command and checking the Use iMate check box on the dialog.

## Code Samples 
To use this sample create a new part by extruding a rectangle to create a cube. Create a mate iMate on one of the faces. Next, create a flush iMate on any of the faces connecting to the first face. Save this part to C:\TempiMatePart.ipt. Finally, have an assembly open and run the sample code.

### iLogic
```vb
' Get the component definition of the currently open assembly.
' This will fail if an assembly document is not open.
Dim asmCompDef As AssemblyComponentDefinition = ThisDoc.Document.ComponentDefinition

' Create a new matrix object. It will be initialized to an identity matrix.
Dim matrix As Matrix = ThisApplication.TransientGeometry.CreateMatrix()

' Place the first occurrence.
Dim occ As ComponentOccurrence = asmCompDef.Occurrences.Add("C:\Temp\iMatePart.ipt", matrix)

' Place the second occurrence, but use iMates for its placement. This is
' equivalent to "Use iMate" check box on the "Place Component" dialog.
Dim occEnumerator As ComponentOccurrencesEnumerator = asmCompDef.Occurrences.AddUsingiMates("C:\Temp\iMatePart.ipt", False)

' Since the 'PlaceAllMatching' flag was specified as False, we can be
' sure that just one ComponentOccurrence was returned in the enumerator.
Dim placedOcc As ComponentOccurrence = occEnumerator.Item(1)
```
[Official Autodesk help page](https://help.autodesk.com/view/INVNTOR/2025/ENU/?guid=AddUsingiMates_Sample)