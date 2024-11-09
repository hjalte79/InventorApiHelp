# Using the BOM APIs

## Description
This sample demonstrates the Bill of Materials API functionality in assemblies.

## Code Samples 
Have an assembly document open and run the following sample. Then check the "iLogic log"

### iLogic
```vb
Public Sub Main()
    ' Set a reference to the assembly document.
    ' This assumes an assembly document is active.
    Dim doc As AssemblyDocument = ThisDoc.Document

    Dim firstLevelOnly As Boolean
    If MsgBox("First level only?", vbYesNo) = vbYes Then
        firstLevelOnly = True
    Else
        firstLevelOnly = False
    End If

    ' Set a reference to the BOM
    Dim bom As BOM = doc.ComponentDefinition.BOM

    ' Set whether first level only or all levels.
    If firstLevelOnly Then
        bom.StructuredViewFirstLevelOnly = True
    Else
        bom.StructuredViewFirstLevelOnly = False
    End If

    ' Make sure that the structured view is enabled.
    bom.StructuredViewEnabled = True

    'Set a reference to the "Structured" BOMView
    Dim bomView As BOMView = bom.BOMViews.Item("Structured")

    logger.Info("Item / Quantity / Part Number / Description")
    logger.Info("----------------------------------------------------------------------------------")

    'Initialize the tab for ItemNumber
    Dim itemTab As Long
    itemTab = -3
    QueryBOMRowProperties(bomView.BOMRows, itemTab)
End Sub

Private Sub QueryBOMRowProperties(bomRows As BOMRowsEnumerator, itemTab As Long)
    itemTab = itemTab + 3
    ' Iterate through the contents of the BOM Rows.
    Dim i As Long
    For i = 1 To bomRows.Count
        ' Get the current row.
        Dim row As BOMRow = bomRows.Item(i)

        'Set a reference to the primary ComponentDefinition of the row
        Dim compDef As ComponentDefinition = row.ComponentDefinitions.Item(1)

        Dim partNumProperty As [Property]
        Dim descripProperty As [Property]

        If TypeOf compDef Is VirtualComponentDefinition Then
            'Get the file property that contains the "Part Number"
            'The file property is obtained from the virtual component definition
            partNumProperty = compDef.PropertySets.Item("Design Tracking Properties").Item("Part Number")

            'Get the file property that contains the "Description"
            descripProperty = compDef.PropertySets.Item("Design Tracking Properties").Item("Description")

            logger.Info(TAB(itemTab) & row.ItemNumber & "/ " & row.ItemQuantity & "/ " & partNumProperty.Value & "/ " & descripProperty.Value)
        Else
            'Get the file property that contains the "Part Number"
            'The file property is obtained from the parent
            'document of the associated ComponentDefinition.
            partNumProperty = compDef.Document.PropertySets.Item("Design Tracking Properties").Item("Part Number")

            'Get the file property that contains the "Description"
            descripProperty = compDef.Document.PropertySets.Item("Design Tracking Properties").Item("Description")

            logger.Info(TAB(itemTab) & row.ItemNumber & "/ " & row.ItemQuantity & "/ " & partNumProperty.Value & "/ " & descripProperty.Value)

            'Recursively iterate child rows if present.
            If Not row.ChildRows Is Nothing Then
                QueryBOMRowProperties(row.ChildRows, itemTab)
            End If
        End If
    Next
    itemTab = itemTab - 3
End Sub

Private Function TAB(length As Integer) As String
    Return New String(" ", length)
End Function
```
[Official Autodesk help page](https://help.autodesk.com/view/INVNTOR/2025/ENU/?guid=BOM_Sample)