# Create rotational assembly joint

## Description
This sample demonstrates creating a mate constraint between two occurrences using the work planes within those occurrences.

## Code Samples 
This sample demonstrates creating an assembly joint. It connects the midpoints of the edges of two faces using a rotational joint. To do this it first creates a geometry intent object of the midpoint of the edge and then creates another intent using the face and the midpoint intent. It does this to create to midpoint intents which it then uses to create the rotational connection.

The sample uses and existing part that must be set up to allow it to work correctly. To create the sample part you can use any part that has a planar face and a linear edge connected to that planar face. A simple box is sufficient. In this part Add a mate iMate to the planar face and rename the iMate to "Face1". Also add a mate iMate to a linear edge that is on the face previously named and rename this iMate to "Edge1". Save the part to "C:\Temp\SamplePart.ipt" or any other name and edit the code below to reference the file. You can then run the sample code which will create a new assembly, insert two instances of the part and create a rotational connection between them. Then it will animation the rotation by driving the connection.

### iLogic
```vb
Public Sub Main()
    ' Create a new assembly document.
    Dim templateFileName = ThisApplication.FileManager.GetTemplateFile(DocumentTypeEnum.kAssemblyDocumentObject)
    Dim asmDoc As AssemblyDocument = ThisApplication.Documents.Add(DocumentTypeEnum.kAssemblyDocumentObject, templateFileName)

    Dim asmDef As AssemblyComponentDefinition = asmDoc.ComponentDefinition

    ' Place an occurrence into the assembly.
    Dim occ1 As ComponentOccurrence
    Dim occ2 As ComponentOccurrence
    Dim trans As Matrix = ThisApplication.TransientGeometry.CreateMatrix
    occ1 = asmDef.Occurrences.Add("C:\Temp\SamplePart.ipt", trans)

    ' Place a second occurrence with the matrix adjusted so it fits correctly with the first occurrence.
    trans.Cell(1, 4) = 6 * 2.54
    occ2 = asmDef.Occurrences.Add("C:\Temp\SamplePart.ipt", trans)

    ' Get Face 1 from occ1 and create a FaceProxy.
    Dim faceOnOcc1 As Face = GetNamedEntity(occ1, "Face1")

    ' Get Face 1 from occ2 and create a FaceProxy.
    Dim faceOnOcc2 As Face = GetNamedEntity(occ2, "Face1")

    ' Get Edge 1 from occ2 and create an EdgeProxy.
    Dim edgeOnOcc2 As Edge = GetNamedEntity(occ2, "Edge1")

    ' Get Edge 3 from occ1 and create an EdgeProxy.
    Dim edgeOnOcc1 As Edge = GetNamedEntity(occ1, "Edge1")

    ' Create an intent to the center of Edge1.
    Dim edgeOcc2Intent As GeometryIntent = asmDef.CreateGeometryIntent(edgeOnOcc2, PointIntentEnum.kMidPointIntent)

    ' Create an intent to the center of Edge3.
    Dim edgeOcc1Intent As GeometryIntent = asmDef.CreateGeometryIntent(edgeOnOcc1, PointIntentEnum.kMidPointIntent)

    ' Create two intents to define the geometry for the joint.
    Dim intentOne As GeometryIntent = asmDef.CreateGeometryIntent(faceOnOcc2, edgeOcc2Intent)
    Dim intentTwo As GeometryIntent = asmDef.CreateGeometryIntent(faceOnOcc1, edgeOcc1Intent)

    ' Create a rotational jont between the two parts.
    Dim jointDef As AssemblyJointDefinition = asmDef.Joints.CreateAssemblyJointDefinition(
    AssemblyJointTypeEnum.kRotationalJointType, intentOne, intentTwo)
    jointDef.FlipAlignmentDirection = False
    jointDef.FlipOriginDirection = True

    Dim joint As AssemblyJoint = asmDef.Joints.Add(jointDef)

    ' Make the joint visible.
    joint.Visible = True

    ' Drive the joint to animate it.
    joint.DriveSettings.StartValue = "0 deg"
    joint.DriveSettings.EndValue = "180 deg"
    joint.DriveSettings.GoToStart()
    joint.DriveSettings.PlayForward()
    joint.DriveSettings.PlayReverse()
End Sub


' This finds the entity associated with an iMate of a specified name.  This
' allows iMates to be used as a generic naming mechansim.
Private Function GetNamedEntity(Occurrence As ComponentOccurrence, Name As String) As Object
    ' Look for the iMate that has the specified name in the referenced file.
    Dim partDef As PartComponentDefinition = Occurrence.Definition
    Dim resultEntity As Object = Nothing
    For Each iMate As iMateDefinition In partDef.iMateDefinitions
        ' Check to see if this iMate has the correct name.
        If UCase(iMate.Name) = UCase(Name) Then
            ' Get the geometry assocated with the iMate.
            Dim entity As Object = iMate.entity

            ' Create a proxy.
            Occurrence.CreateGeometryProxy(entity, resultEntity)

            Exit For
        End If
    Next

    ' Return the found entity, or Nothing if a match wasn't found.
    Return resultEntity
End Function
```
[Official Autodesk help page](https://help.autodesk.com/view/INVNTOR/2025/ENU/?guid=AssemblyRotationalJoint_Sample)