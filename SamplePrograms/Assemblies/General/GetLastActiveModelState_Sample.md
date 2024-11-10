# Open assembly using last model state

## Description
This sample demonstrates how to open an assembly document in its last active model state.

## Code Samples

### iLogic
```vb
Dim fullFileName As String = "C:\temp\Assembly1.iam"

' Set a reference to the FileManager object.
Dim fileManager As FileManager = ThisApplication.FileManager

' Get the name of the last active model state.
Dim lastActiveModelState As String = fileManager.GetLastActiveModelState(fullFileName)

' Use the full file name and ModelState name to get the full document name.
Dim fullDocumentName As String = fileManager.GetFullDocumentName(fullFileName, lastActiveModelState)

' Open the document in the last active model state.
Dim doc As AssemblyDocument = ThisApplication.Documents.Open(fullDocumentName)
```
[Official Autodesk help page](https://help.autodesk.com/view/INVNTOR/2025/ENU/?guid=GetLastActiveModelState_Sample)