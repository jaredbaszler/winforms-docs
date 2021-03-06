---
title: Settings
page_title: Settings | UI for WinForms Documentation
description: Settings
slug: winforms/wordsprocessing/formats-and-conversion/docx/settings
tags: settings
published: True
position: 2
previous_url: wordsprocessing-formats-and-conversion-docx-settings
---

# Settings

__DocxFormatProvider__ allows for import of docx documents and respectively export of RadFlowDocument to docx. Additionally, the import/export settings provide modification options. The current article outlines the available settings.

## Export Settings

The export settings which you can specify are as follows: 

__InvalidDocumentAction__

This setting allows you to define the action which should be executed in case the __RadFlowDocument__ that is about to be exported is not compliant with Open Office XML standard. The possible values for the enumeration are:
              

* __None__: The RadFlowDocument will be exported as is without making any validations. This can result in invalid document that may not be opened correctly.
                  

* __ThrowAnException__: DocxFormatProvider runs validation over the document passed for export. If the document is invalid, the provider throws an __InvalidDocumentException__ which contains the list with the validation errors. This is particularly useful when you want to assure you are building valid docx documents.
                  

* __Repair__: DocxFormatProvider runs validation over the document passed for export. If there is a validation exception, the provider tries to repair it. The result is a valid docx document, but the method changes the structure of the input document.


__AutoUpdateFields__

The __AutoUpdateFields__ setting indicates if fields should be auto-updated when the exported document is opened. The default value is __False__.
              
The following code snippet shows how you can create and specify particular export settings to DocxFormatProvider.

{{source=..\SamplesCS\WordsProcessing\FormatsAndConversion\Docx\WordsProcessingWordsProcessingSettings.cs region=radwordsprocessing_formats_and_conversion_docx_settings_0}} 
{{source=..\SamplesVB\WordsProcessing\FormatsAndConversion\Docx\WordsProcessingWordsProcessingSettings.vb region=radwordsprocessing_formats_and_conversion_docx_settings_0}} 

````C#
            
DocxFormatProvider provider = new DocxFormatProvider();
DocxExportSettings exportSettings = new DocxExportSettings();
exportSettings.AutoUpdateFields = true;
exportSettings.InvalidDocumentAction = InvalidDocumentAction.ThrowException;
provider.ExportSettings = exportSettings;

````
````VB.NET
Dim provider As New DocxFormatProvider()
Dim exportSettings As New DocxExportSettings()
exportSettings.AutoUpdateFields = True
exportSettings.InvalidDocumentAction = InvalidDocumentAction.ThrowException
provider.ExportSettings = exportSettings

````

{{endregion}}
