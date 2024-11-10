# Add assembly occurrences to a new folder

## Description
The following sample demonstrates the creation of a shrinkwrap substitute within an assembly.

## Code Samples 
Open any assembly document and run the sample. A shrinkwrap part is created at the same location as the assembly.

### iLogic
```vb
' Set a reference to the active assembly document
Dim doc As AssemblyDocument = ThisDoc.Document
Dim def As AssemblyComponentDefinition = doc.ComponentDefinition

' Create a new part document that will be the shrinkwrap substitute
Dim partDoc As PartDocument = ThisApplication.Documents.Add(DocumentTypeEnum.kPartDocumentObject, , False)
Dim partDef As PartComponentDefinition = partDoc.ComponentDefinition

Dim derivedAssemblyDef As DerivedAssemblyDefinition = partDef.ReferenceComponents.DerivedAssemblyComponents.CreateDefinition(doc.FullDocumentName)

' Set various shrinkwrap related options
derivedAssemblyDef.DeriveStyle = DerivedComponentStyleEnum.kDeriveAsSingleBodyNoSeams
derivedAssemblyDef.IncludeAllTopLevelWorkFeatures = DerivedComponentOptionEnum.kDerivedIncludeAll
derivedAssemblyDef.IncludeAllTopLevelSketches = DerivedComponentOptionEnum.kDerivedIncludeAll
derivedAssemblyDef.IncludeAllTopLeveliMateDefinitions = DerivedComponentOptionEnum.kDerivedExcludeAll
derivedAssemblyDef.IncludeAllTopLevelParameters = DerivedComponentOptionEnum.kDerivedExcludeAll
derivedAssemblyDef.ReducedMemoryMode = True
derivedAssemblyDef.SetHolePatchingOptions(DerivedHolePatchEnum.kDerivedPatchAll)
derivedAssemblyDef.SetRemoveByVisibilityOptions(DerivedGeometryRemovalEnum.kDerivedRemovePartsAndFaces, 25)

' Create the shrinkwrap component
Dim derivedAssembly As DerivedAssemblyComponent = partDef.ReferenceComponents.DerivedAssemblyComponents.Add(derivedAssemblyDef)

' Save the part
Dim substituteFileName As String
substituteFileName = Left$(doc.FullFileName, Len(doc.FullFileName) - 4)
substituteFileName = substituteFileName & "_ShrinkwrapSubstitute.ipt"

ThisApplication.SilentOperation = True
partDoc.SaveAs(substituteFileName, False)
ThisApplication.SilentOperation = False

' Create a substitute level of detail using the shrinkwrap part.
Dim substituteModelState As ModelState = def.ModelStates.AddSubstitute(substituteFileName)

' Release reference of the invisibly opened part document.
partDoc.ReleaseReference()
```
[Official Autodesk help page](https://help.autodesk.com/view/INVNTOR/2025/ENU/?guid=Shrinkwrap_Sample)