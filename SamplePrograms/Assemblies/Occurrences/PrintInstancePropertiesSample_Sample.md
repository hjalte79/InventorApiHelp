# Print instance properties of all components in an assembly

## Description
This sample demonstrates how to get the instance properties of all components in an assembly.

## Code Samples 
This iLogic sample demonstrates how to get the instance properties of all components in an assembly.

### iLogic
```vb
Public Sub Main()
    If ThisApplication.ActiveDocument Is Nothing Then
        MsgBox("Please open an assembly document!")
    ElseIf (ThisApplication.ActiveDocument.DocumentType <> DocumentTypeEnum.kAssemblyDocumentObject) Then
        MsgBox("Please open an assembly document!")
    Else

        Dim doc As AssemblyDocument = ThisApplication.ActiveDocument

        GetInstancePropInfo(doc.ComponentDefinition.Occurrences)

    End If
End Sub

Sub GetInstancePropInfo(occs As ComponentOccurrences)

    Dim occ As ComponentOccurrence
    Dim tempOccu As ComponentOccurrence

    ' The Instance Properties is accessiable via ComponentOccurrence only
    ' so below will get the ComponentOccurrence from ComponentOccurrenceProxy.
    For Each tempOccu In occs
        If (tempOccu.Type = ObjectTypeEnum.kComponentOccurrenceProxyObject) Then
            occ = tempOccu.NativeObject

        Else
            occ = tempOccu
        End If

        logger.Info(occ.Name)
        '  Instance Properties
        If (occ.OccurrencePropertySetsEnabled) Then
            For Each oProp As Inventor.Property In occ.OccurrencePropertySets(1)

                ' Print property info
                logger.Info("    " & oProp.DisplayName & ":" & oProp.Expression)
            Next
        End If

        GetInstancePropInfo(tempOccu.SubOccurrences)
    Next
End Sub
```
[Official Autodesk help page](https://help.autodesk.com/view/INVNTOR/2025/ENU/?guid=AssemblyConstraints_AddInsertConstraint_Sample)