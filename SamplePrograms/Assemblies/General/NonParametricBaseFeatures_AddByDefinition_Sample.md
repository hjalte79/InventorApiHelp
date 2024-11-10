# Associative body copy

## Description
The following sample demonstrates copying bodies (associatively and non-associatively) across parts in an assembly.

## Code Samples 
Before running the sample, create an assembly with two parts in it. The sample copies the body from the first part into the second part.

### iLogic
```vb
' Set a reference to the active assembly document.
Dim assemblyDoc As AssemblyDocument = ThisDoc.Document

Dim assemblyDef As AssemblyComponentDefinition = assemblyDoc.ComponentDefinition

Dim occurrence1 As ComponentOccurrence = assemblyDef.Occurrences.Item(1)

Dim partDef1 As PartComponentDefinition = occurrence1.Definition

Dim occurrence2 As ComponentOccurrence = assemblyDef.Occurrences.Item(2)

Dim partDef2 As PartComponentDefinition = occurrence2.Definition

' Get the source solid body from the first part.
Dim sourceBody As SurfaceBody = partDef1.SurfaceBodies.Item(1)

Dim sourceBodyProxy As SurfaceBodyProxy
occurrence1.CreateGeometryProxy(sourceBody, sourceBodyProxy)

' Create an associative surface base feature in the second part.
Dim featureDef1 As NonParametricBaseFeatureDefinition = partDef2.Features.NonParametricBaseFeatures.CreateDefinition

Dim collection As ObjectCollection = ThisApplication.TransientObjects.CreateObjectCollection

collection.Add(sourceBodyProxy)

featureDef1.BRepEntities = collection
featureDef1.OutputType = BaseFeatureOutputTypeEnum.kSurfaceOutputType
featureDef1.TargetOccurrence = occurrence2
featureDef1.IsAssociative = True

Dim baseFeature1 As NonParametricBaseFeature = partDef2.Features.NonParametricBaseFeatures.AddByDefinition(featureDef1)

' Create a non-associative solid base feature in the second part.
Dim featureDef2 As NonParametricBaseFeatureDefinition = partDef2.Features.NonParametricBaseFeatures.CreateDefinition

featureDef2.BRepEntities = collection
featureDef2.OutputType = BaseFeatureOutputTypeEnum.kSolidOutputType
featureDef2.TargetOccurrence = occurrence2

Dim baseFeature2 As NonParametricBaseFeature = partDef2.Features.NonParametricBaseFeatures.AddByDefinition(featureDef2)

assemblyDoc.Update()
```
[Official Autodesk help page](https://help.autodesk.com/view/INVNTOR/2025/ENU/?guid=NonParametricBaseFeatures_AddByDefinition_Sample)