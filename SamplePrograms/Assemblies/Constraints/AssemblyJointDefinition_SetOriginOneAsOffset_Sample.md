# Create planar AssemblyJoint with offset to origins

## Description
This sample demonstrates how to create a planar AssemblyJoint with offset to the OriginOne and OriginTwo.

## Code Samples 
Create a part with some solid and make sure there are linear edges in it, save it as C:\Temp\Part1.ipt or you need to edit the iLogic code to change the paths to make it work.

### iLogic
```vb
Dim doc As AssemblyDocument = ThisApplication.Documents.Add(DocumentTypeEnum.kAssemblyDocumentObject)

Dim compDef As AssemblyComponentDefinition = doc.ComponentDefinition

Dim matrix As Matrix = ThisApplication.TransientGeometry.CreateMatrix()

' Create two occurrences for adding assembly joint, make sure the sample Part1 has linear edge in it.
Dim occ1 As ComponentOccurrence = compDef.Occurrences.Add("C:\Temp\Part1.ipt", matrix)

matrix.SetTranslation(ThisApplication.TransientGeometry.CreateVector(20, 20, 20))

Dim oOcc2 As ComponentOccurrence = compDef.Occurrences.Add("C:\Temp\Part1.ipt", matrix)

' Create two GeometryIntent objects for creating assembly joint.
Dim origin1 As GeometryIntent
Dim origin2 As GeometryIntent
For Each oEdge As Edge In occ1.SurfaceBodies(1).Edges
    If (oEdge.GeometryType = CurveTypeEnum.kLineSegmentCurve) Then
        origin1 = compDef.CreateGeometryIntent(oEdge, PointIntentEnum.kMidPointIntent)
        Exit For
    End If
Next

For Each oEdge In oOcc2.SurfaceBodies(1).Edges
    If oEdge.GeometryType = CurveTypeEnum.kLineSegmentCurve Then
        origin2 = compDef.CreateGeometryIntent(oEdge, PointIntentEnum.kMidPointIntent)
        Exit For
    End If
Next

' Create AssemblyJointDefinition
Dim jointDef As AssemblyJointDefinition = compDef.Joints.CreateAssemblyJointDefinition(AssemblyJointTypeEnum.kPlanarJointType, origin1, origin2)

jointDef.SetOriginOneAsOffset(5, 5)
jointDef.SetOriginTwoAsOffset(2, 2)

logger.Info(jointDef.OriginOneDefinitionType = AssemblyJointOriginDefinitionTypeEnum.kOffsetOriginDefinitionType)

' Create assembly joint.
compDef.Joints.Add(jointDef)
```
[Official Autodesk help page](https://help.autodesk.com/view/INVNTOR/2025/ENU/?guid=AssemblyJointDefinition_SetOriginOneAsOffset_Sample)