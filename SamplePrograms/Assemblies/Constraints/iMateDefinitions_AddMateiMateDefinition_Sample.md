# Add iMate Definition

## Description
Add iMate definitions using AddMateiMateDefinition and AddInsertiMateDefinition.

## Code Samples 


### iLogic
```vb
' Create a new part document, using the default part template.
Dim templateFileName = ThisApplication.FileManager.GetTemplateFile(DocumentTypeEnum.kPartDocumentObject)
Dim doc As PartDocument = ThisApplication.Documents.Add(DocumentTypeEnum.kPartDocumentObject, templateFileName)

' Set a reference to the component definition.
Dim compDef As PartComponentDefinition = doc.ComponentDefinition

' Create a new sketch on the X-Y work plane.
Dim sketch As PlanarSketch = compDef.Sketches.Add(compDef.WorkPlanes(3))

' Set a reference to the transient geometry object.
Dim transGeom As TransientGeometry = ThisApplication.TransientGeometry

' Draw a 4cm x 3cm rectangle with the corner at (0,0)
Dim rectangleLines As SketchEntitiesEnumerator = sketch.SketchLines.AddAsTwoPointRectangle(
                        transGeom.CreatePoint2d(0, 0),
                        transGeom.CreatePoint2d(4, 3))

' Create a profile.
Dim profile As Profile = sketch.Profiles.AddForSolid

' Create a base extrusion 1cm thick.
Dim extrudeDef As ExtrudeDefinition = compDef.Features.ExtrudeFeatures.CreateExtrudeDefinition(
    profile, PartFeatureOperationEnum.kNewBodyOperation)

extrudeDef.SetDistanceExtent(1, PartFeatureExtentDirectionEnum.kNegativeExtentDirection)

Dim extrude1 As ExtrudeFeature = compDef.Features.ExtrudeFeatures.Add(extrudeDef)

' Get the top face of the extrusion to use for creating the new sketch.
Dim frontFace As Face = extrude1.StartFaces.Item(1)

' Create a new sketch on this face, but use the method that allows you to
' control the orientation and orgin of the new sketch.
sketch = compDef.Sketches.AddWithOrientation(frontFace,
            compDef.WorkAxes.Item(1), True, True, compDef.WorkPoints(1))

' Create a sketch circle with the center at (2, 1.5).
Dim circle As SketchCircle = sketch.SketchCircles.AddByCenterRadius(transGeom.CreatePoint2d(2, 1.5), 0.5)

' Create a profile.
profile = sketch.Profiles.AddForSolid()

' Create the second extrude (a hole).
Dim extrude2 As ExtrudeFeature = compDef.Features.ExtrudeFeatures.AddByThroughAllExtent(
                profile, PartFeatureExtentDirectionEnum.kNegativeExtentDirection,
                PartFeatureOperationEnum.kCutOperation)

' Create a mate iMateDefinition on a side face of the first extrude.
Dim oMateiMateDefinition As MateiMateDefinition = compDef.iMateDefinitions.AddMateiMateDefinition(
                extrude1.SideFaces.Item(1), 0, , , "MateA")

' Create a match list of names to use for the next iMateDefinition.
Dim strMatchList(2) As String
strMatchList(0) = "InsertA"
strMatchList(1) = "InsertB"
strMatchList(2) = "InsertC"

' Create an insert iMateDefinition on the cylindrical face of the second extrude.
compDef.iMateDefinitions.AddInsertiMateDefinition(
    extrude2.SideFaces.Item(1), False, 0, , "InsertA", strMatchList)
```
[Official Autodesk help page](https://help.autodesk.com/view/INVNTOR/2025/ENU/?guid=iMateDefinitions_AddMateiMateDefinition_Sample)