# Inventor API examples

- Sample Programs
  - Assemblies
    - Analysis
      - [Interference Analysis](SamplePrograms/Assemblies/Analysis/InterferenceAnalysis.md)
    
    - Bill of Materials
      - [Using the BOM APIs](SamplePrograms/Assemblies/BillOfMaterials/UsingBomApis.md)
      - [Exporting the assembly BOM](SamplePrograms/Assemblies/BillOfMaterials/ExportingBOM.md)
    
    - Constraints
      - [Add assembly insert constraint](SamplePrograms/Assemblies/Constraints/AddAssemblyInsertConstraint.md)
      - [Add mate constraint using work planes in parts](SamplePrograms/Assemblies/Constraints/AddAssemblyMatetConstraint1.md)
      - [Add mate constraint with limits](SamplePrograms/Assemblies/Constraints/AddAssemblyMatetConstraint3.md)
      - [Create planar AssemblyJoint with offset to origins](SamplePrograms/Assemblies/Constraints/AssemblyJointDefinition_SetOriginOneAsOffset_Sample.md)
      - [Create rotational assembly joint](SamplePrograms/Assemblies/Constraints/AssemblyRotationalJoint_Sample.md)
      - [Add iMate Definition](SamplePrograms/Assemblies/Constraints/iMateDefinitions_AddMateiMateDefinition_Sample.md)
      - [iMate Result Creation](SamplePrograms/Assemblies/Constraints/iMateResult_Sample.md)
    
    - General
      - [Add assembly occurrences to a new folder](SamplePrograms/Assemblies/General/BrowserPaneObject_AddBrowserFolder_Sample.md)
      - [Demote occurence](SamplePrograms/Assemblies/General/BrowserPaneObject_Reorder_Demote_Sample.md)
      - [Promote occurence](SamplePrograms/Assemblies/General/BrowserPaneObject_Reorder_Promote_Sample.md)
      - [Assembly Ground Occurrences](SamplePrograms/Assemblies/General/ComponentOccurrence_Grounded_Sample.md)
      - [Create Revit Export sample](SamplePrograms/Assemblies/General/CreateRevitExportSample_Sample.md)
      - [Open assembly using last model state](SamplePrograms/Assemblies/General/GetLastActiveModelState_Sample.md)
      - [Associative body copy](SamplePrograms/Assemblies/General/NonParametricBaseFeatures_AddByDefinition_Sample.md)
      - [Shrink wrap substitute in assembly](SamplePrograms/Assemblies/General/Shrinkwrap_Sample.md)


## Changes made to official documentation
- Converted code from VBa to iLogic/VB.Net code
  - keywords like 'Set' and 'Call' could be removed in many places.
  - the enum names needed to be added.
    - 'kSurfaceOutputType' -> 'BaseFeatureOutputTypeEnum.kSurfaceOutputType'
  - The declaring and setting a variables can be done in one line. 
- Used the more reliable iLogic object/properties for getting the executing document.
  - 'ThisApplication.ActiveDocumet' -> 'ThisDoc.Document'
- Changed the variable naming convention, from 'Hungarian' to 'camelCase' notation.


[Official Autodesk help page](https://help.autodesk.com/view/INVNTOR/2025/ENU/?guid=GUID-DE98632B-3DC0-422B-A1C6-8A5A15C99E11)