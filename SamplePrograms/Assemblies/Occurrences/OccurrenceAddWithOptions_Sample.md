# Create assembly occurrence with representations

## Description
This sample demonstrates how to create an assembly occurrence by specifying various representations.

## Code Samples 
Before running this sample, make sure that the file C:\Temp\Reps.iam exists (or change the path in the sample). The file must contain a model state named MyModelState, a positional representation named MyPositionalRep and a design view representation named MyDesignViewRep.

### iLogic
```vb
' Set a reference to the assembly component definintion.
' This assumes an assembly document is open.
Dim asmCompDef As AssemblyComponentDefinition = ThisDoc.Document.ComponentDefinition

' Set a reference to the transient geometry object.
Dim oTG As TransientGeometry = ThisApplication.TransientGeometry

' Create a matrix. A new matrix is initialized with an identity matrix.
Dim matrix As Matrix = oTG.CreateMatrix

' Create a new NameValueMap object
Dim options As NameValueMap = ThisApplication.TransientObjects.CreateNameValueMap

' Set the representations to use when creating the occurrence.
options.Add("ModelState", "MyModelState")
options.Add("PositionalRepresentation", "MyPositionalRep")
options.Add("DesignViewRepresentation", "MyDesignViewRep")
options.Add("DesignViewAssociative", True)

' Add the occurrence.
Dim occ As ComponentOccurrence = asmCompDef.Occurrences.AddWithOptions("C:\Temp\Reps.iam", matrix, options)
```
[Official Autodesk help page](https://help.autodesk.com/view/INVNTOR/2025/ENU/?guid=OccurrenceAddWithOptions_Sample)