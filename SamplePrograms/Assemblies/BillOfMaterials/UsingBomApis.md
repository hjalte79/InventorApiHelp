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
    Dim oDoc As AssemblyDocument = ThisDoc.Document

    Dim FirstLevelOnly As Boolean
    If MsgBox("First level only?", vbYesNo) = vbYes Then
        FirstLevelOnly = True
    Else
        FirstLevelOnly = False
    End If

    ' Set a reference to the BOM
    Dim oBOM As BOM = oDoc.ComponentDefinition.BOM

    ' Set whether first level only or all levels.
    If FirstLevelOnly Then
        oBOM.StructuredViewFirstLevelOnly = True
    Else
        oBOM.StructuredViewFirstLevelOnly = False
    End If

    ' Make sure that the structured view is enabled.
    oBOM.StructuredViewEnabled = True

    'Set a reference to the "Structured" BOMView
    Dim oBOMView As BOMView = oBOM.BOMViews.Item("Structured")

    Logger.Info("Item / Quantity / Part Number / Description")
    Logger.Info("----------------------------------------------------------------------------------")

    'Initialize the tab for ItemNumber
    Dim ItemTab As Long
    ItemTab = -3
    QueryBOMRowProperties(oBOMView.BOMRows, ItemTab)
End Sub

Private Sub QueryBOMRowProperties(oBOMRows As BOMRowsEnumerator, ItemTab As Long)
    ItemTab = ItemTab + 3
    ' Iterate through the contents of the BOM Rows.
    Dim i As Long
    For i = 1 To oBOMRows.Count
        ' Get the current row.
        Dim oRow As BOMRow = oBOMRows.Item(i)

        'Set a reference to the primary ComponentDefinition of the row
        Dim oCompDef As ComponentDefinition = oRow.ComponentDefinitions.Item(1)

        Dim oPartNumProperty As [Property]
        Dim oDescripProperty As [Property]

        If TypeOf oCompDef Is VirtualComponentDefinition Then
            'Get the file property that contains the "Part Number"
            'The file property is obtained from the virtual component definition
            oPartNumProperty = oCompDef.PropertySets.Item("Design Tracking Properties").Item("Part Number")

            'Get the file property that contains the "Description"
            oDescripProperty = oCompDef.PropertySets.Item("Design Tracking Properties").Item("Description")

            Logger.Info(TAB(ItemTab) & oRow.ItemNumber & "/ " & oRow.ItemQuantity & "/ " & oPartNumProperty.Value & "/ " & oDescripProperty.Value)
        Else
            'Get the file property that contains the "Part Number"
            'The file property is obtained from the parent
            'document of the associated ComponentDefinition.
            oPartNumProperty = oCompDef.Document.PropertySets _
            .Item("Design Tracking Properties").Item("Part Number")

            'Get the file property that contains the "Description"
            oDescripProperty = oCompDef.Document.PropertySets _
            .Item("Design Tracking Properties").Item("Description")

            Logger.Info(TAB(ItemTab) & oRow.ItemNumber & "/ " & oRow.ItemQuantity & "/ " & oPartNumProperty.Value & "/ " & oDescripProperty.Value)

            'Recursively iterate child rows if present.
            If Not oRow.ChildRows Is Nothing Then
                Call QueryBOMRowProperties(oRow.ChildRows, ItemTab)
            End If
        End If
    Next
    ItemTab = ItemTab - 3
End Sub

Private Function TAB(length As Integer) As String
    Return New String(" ", length)
End Function
```
[Official Autodesk help page](https://help.autodesk.com/view/INVNTOR/2025/ENU/?guid=BOM_Sample)