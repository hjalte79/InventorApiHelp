# Traverse an Assembly

## Description
This sample shows how to recursively traverse an assembly and get the count of leaf node components and subassemblies.

## Code Samples 

### iLogic
```vb
Public Sub Main()
    ' Set reference to active document.
    ' This assumes the active document is an assembly
    Dim doc As Inventor.AssemblyDocument = ThisDoc.Document

    ' Get assembly component definition
    Dim compDef As Inventor.ComponentDefinition = doc.ComponentDefinition

    Dim leafNodes As Long = 0
    Dim subAssemblies As Long = 0

    ProcessAllOccurences(compDef.Occurrences, leafNodes, subAssemblies)

    logger.Info("No of leaf nodes    : " + CStr(leafNodes))
    logger.Info("No of sub assemblies: " + CStr(subAssemblies))
End Sub

Private Sub ProcessAllOccurences(ByVal compOcc As ComponentOccurrences,
                                ByRef leafNodes As Long,
                                ByRef subAssemblies As Long)

    For Each subCompOcc As ComponentOccurrence In compOcc
        logger.Info(subCompOcc.Name)
        If (subCompOcc.DefinitionDocumentType = DocumentTypeEnum.kAssemblyDocumentObject) Then

            subAssemblies = subAssemblies + 1
            ProcessAllOccurences(subCompOcc.SubOccurrences, leafNodes, subAssemblies)

        Else

            leafNodes = leafNodes + 1

        End If
    Next

End Sub
```
[Official Autodesk help page](https://help.autodesk.com/view/INVNTOR/2025/ENU/?guid=AssemblyTraverse_Sample)