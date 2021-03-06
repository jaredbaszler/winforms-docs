---
title: Overview
page_title: Fields | UI for WinForms Documentation
description: Fields
slug: winforms/wordsprocessing/concepts/fields
tags: fields
published: True
position: 0
previous_url: wordsprocessing-concepts-fields
---

# Fields



__Fields__ in the __RadFlowDocument__ model are special constructions that hold data which can change/be updated – for example page numbers or merge fields. Fields consist of field code and field result. The field code fragment defines how the field result should be calculated when the field is updated. The field result fragment holds the latest calculated result. In the model these two fragments are separated by a special type of inline – [FieldCharacter]({%slug winforms/wordsprocessing/model/fieldcharacter%}). __FieldCharacters__ are 3 types:

* __Start__: Defines the start of the field.
          

* __End__: Defines the end of the field.
          

* __Separate__: Separates the code and result fragments.

The inlines between the __start__ and __separate__ field characters form the __code fragment__ and the inlines between the __separate__ and __end__ field characters form the __result fragment__.

Here is how simple page field looks like inside the document:

![wordsprocessing-concepts-fields 001](images/wordsprocessing-concepts-fields001.png)

In the document object model Fields are represented by the[Field](http://www.telerik.com/help/winforms/t_telerik_windows_documents_flow_model_fields_field.html) abstract class which holds references to the __Start__, __Separate__ and __End__ field characters that are related to the field.
      

## Inserting Fields

The suggested way to insert field is to use the __InsertField()__ method of [RadFlowDocumentEditor]({%slug winforms/wordsprocessing/editing/radflowdocumenteditor%}) class. It takes care of creating and inserting the code and result fragments as well as placing the appropriate field character inlines to separate them. The __InsertField()__ method returns an instance of the __FieldInfo__ class. It holds references to the start, separate and end field characters and also provides an API for getting the code and result fragments and updating the field.

Here is how to create a document with a single __Date__ field using __RadFlowDocumenteditor__:

{{source=..\SamplesCS\WordsProcessing\Concepts\WordsProcessingFields.cs region=radwordsprocessing_concepts_fields_0}} 
{{source=..\SamplesVB\WordsProcessing\Concepts\WordsProcessingFields.vb region=radwordsprocessing_concepts_fields_0}} 

````C#
RadFlowDocumentEditor editor = new RadFlowDocumentEditor(new RadFlowDocument());
// Create an outer field with empty code fragment
FieldInfo outerFieldInfo = editor.InsertField(string.Empty, "if field result");
// Move the editor after the field start character
editor.MoveToInlineEnd(outerFieldInfo.Start);
// Create the a code fragment with a nested TIME field
editor.InsertText("IF ");
editor.InsertField("TIME \\@ HH", "time field result");
editor.InsertText(" < 12 \"Good morning!\" \"Good afternoon!\"");
outerFieldInfo.UpdateField();
Console.WriteLine(outerFieldInfo.GetResult()); // Output: Good afternoon!

````
````VB.NET
Dim editor As New RadFlowDocumentEditor(New RadFlowDocument())
' Create an outer field with empty code fragment
Dim outerFieldInfo As FieldInfo = editor.InsertField(String.Empty, "if field result")
' Move the editor after the field start character
editor.MoveToInlineEnd(outerFieldInfo.Start)
' Create the a code fragment with a nested TIME field
editor.InsertText("IF ")
editor.InsertField("TIME \@ HH", "time field result")
editor.InsertText(" < 12 ""Good morning!"" ""Good afternoon!""")
outerFieldInfo.UpdateField()
Console.WriteLine(outerFieldInfo.GetResult())
' Output: Good afternoon!

````

{{endregion}} 

You can also create and insert all the parts of the field manually by creating a __FieldInfo__ instance and adding all the inlines to the document structure. Here is how to achieve the same result as in the previous example:

{{source=..\SamplesCS\WordsProcessing\Concepts\WordsProcessingFields.cs region=radwordsprocessing_concepts_fields_1}} 
{{source=..\SamplesVB\WordsProcessing\Concepts\WordsProcessingFields.vb region=radwordsprocessing_concepts_fields_1}} 

````C#
RadFlowDocumentEditor editor = new RadFlowDocumentEditor(new RadFlowDocument());
// Create an outer field with empty code fragment
FieldInfo outerFieldInfo = editor.InsertField(string.Empty, "if field result");
// Move the editor after the field start character
editor.MoveToInlineEnd(outerFieldInfo.Start);
// Create the a code fragment with a nested TIME field
editor.InsertText("IF ");
editor.InsertField("TIME \\@ HH", "time field result");
editor.InsertText(" < 12 \"Good morning!\" \"Good afternoon!\"");
outerFieldInfo.UpdateField();
Console.WriteLine(outerFieldInfo.GetResult()); // Output: Good afternoon!

````
````VB.NET
Dim editor As New RadFlowDocumentEditor(New RadFlowDocument())
' Create an outer field with empty code fragment
Dim outerFieldInfo As FieldInfo = editor.InsertField(String.Empty, "if field result")
' Move the editor after the field start character
editor.MoveToInlineEnd(outerFieldInfo.Start)
' Create the a code fragment with a nested TIME field
editor.InsertText("IF ")
editor.InsertField("TIME \@ HH", "time field result")
editor.InsertText(" < 12 ""Good morning!"" ""Good afternoon!""")
outerFieldInfo.UpdateField()
Console.WriteLine(outerFieldInfo.GetResult())
' Output: Good afternoon!

````

{{endregion}} 




You can see that the manual approach is more verbose and prone to errors. If not all of the field characters are inserted the result is  an invalid document. Using the __RadFlowDocumentEditor__ on the other hand guarantees that the document integrity is maintained.
        

## Updating Fields

__RadWordsPorcessing__ supports updating of some fields types. When a field is updated, its result fragment is replaced with the calculated  result value. Also the __Field__ property of the corresponding __FieldInfo__ object will be initialized to an instance of a __Field__ class that matches the recognized field type.

Here is a list of the field types that support updating:

* Formulas and Expressions (formulas and expressions begin with " = ") 

* IF

* COMPARE

* DATE

* TIME

* HYPERLINK

If the field type is not one of the above the result will not be updated and the Field property of the FieldInfo class will be set to an instance of a  CustomCodeField. The complete list of field codes and the switches for each of them can be found in the [Docx specification](http://www.ecma-international.org/publications/standards/Ecma-376.htm).

Updating a single field is done with the __UpdateField()__ method of the __FieldInfo__ class:

{{source=..\SamplesCS\WordsProcessing\Concepts\WordsProcessingFields.cs region=radwordsprocessing_concepts_fields_2}} 
{{source=..\SamplesVB\WordsProcessing\Concepts\WordsProcessingFields.vb region=radwordsprocessing_concepts_fields_2}} 

````C#
RadFlowDocumentEditor editor = new RadFlowDocumentEditor(new RadFlowDocument());
// Create an outer field with empty code fragment
FieldInfo outerFieldInfo = editor.InsertField(string.Empty, "if field result");
// Move the editor after the field start character
editor.MoveToInlineEnd(outerFieldInfo.Start);
// Create the a code fragment with a nested TIME field
editor.InsertText("IF ");
editor.InsertField("TIME \\@ HH", "time field result");
editor.InsertText(" < 12 \"Good morning!\" \"Good afternoon!\"");
outerFieldInfo.UpdateField();
Console.WriteLine(outerFieldInfo.GetResult()); // Output: Good afternoon!

````
````VB.NET
Dim editor As New RadFlowDocumentEditor(New RadFlowDocument())
' Create an outer field with empty code fragment
Dim outerFieldInfo As FieldInfo = editor.InsertField(String.Empty, "if field result")
' Move the editor after the field start character
editor.MoveToInlineEnd(outerFieldInfo.Start)
' Create the a code fragment with a nested TIME field
editor.InsertText("IF ")
editor.InsertField("TIME \@ HH", "time field result")
editor.InsertText(" < 12 ""Good morning!"" ""Good afternoon!""")
outerFieldInfo.UpdateField()
Console.WriteLine(outerFieldInfo.GetResult())
' Output: Good afternoon!

````

{{endregion}} 

Note that field result is not automatically updated upon insertion. The initial result fragment is passed as a parameter to the __InsertField()__ method.

All fields in the document can be updated using __UpdateFields()__ of __RadFlowDocument__:

{{source=..\SamplesCS\WordsProcessing\Concepts\WordsProcessingFields.cs region=radwordsprocessing_concepts_fields_3}} 
{{source=..\SamplesVB\WordsProcessing\Concepts\WordsProcessingFields.vb region=radwordsprocessing_concepts_fields_3}} 

````C#
RadFlowDocumentEditor editor = new RadFlowDocumentEditor(new RadFlowDocument());
// Create an outer field with empty code fragment
FieldInfo outerFieldInfo = editor.InsertField(string.Empty, "if field result");
// Move the editor after the field start character
editor.MoveToInlineEnd(outerFieldInfo.Start);
// Create the a code fragment with a nested TIME field
editor.InsertText("IF ");
editor.InsertField("TIME \\@ HH", "time field result");
editor.InsertText(" < 12 \"Good morning!\" \"Good afternoon!\"");
outerFieldInfo.UpdateField();
Console.WriteLine(outerFieldInfo.GetResult()); // Output: Good afternoon!

````
````VB.NET
Dim editor As New RadFlowDocumentEditor(New RadFlowDocument())
' Create an outer field with empty code fragment
Dim outerFieldInfo As FieldInfo = editor.InsertField(String.Empty, "if field result")
' Move the editor after the field start character
editor.MoveToInlineEnd(outerFieldInfo.Start)
' Create the a code fragment with a nested TIME field
editor.InsertText("IF ")
editor.InsertField("TIME \@ HH", "time field result")
editor.InsertText(" < 12 ""Good morning!"" ""Good afternoon!""")
outerFieldInfo.UpdateField()
Console.WriteLine(outerFieldInfo.GetResult())
' Output: Good afternoon!

````

{{endregion}} 

## Syntax and Switches

The syntax of a field code is as follows:
        

field-type [field-argument] [switches]

* *field-type*: The type of the field. For example: HYPERINK.

* *argument*: The argument of the field. This is optional as some of the fields do not require an argument.
            

* *switches*: One or several additional properties of the field. The syntax of a switch is the following:
            *\switch-character [switch-argument]*

* *switch-character*: Character defining the switch. For example the "\o" switch for HYPERINK fields defines the tooltip switch.

* *switch-argument*: The argument of the switch. The argument is optional as not all switches require an argument.
                

Below is an example of field code: <br>![wordsprocessing-concepts-fields 003](images/wordsprocessing-concepts-fields003.png)

## Nested Fields

Fields can also be nested in each other. If there are nested fields inside the code fragment of a field – their result will be used when calculating the result of the outer field.

Here is an example of cratering a field which will be evaluated to appropriate greeting based on the time of the day.

{{source=..\SamplesCS\WordsProcessing\Concepts\WordsProcessingFields.cs region=radwordsprocessing_concepts_fields_4}} 
{{source=..\SamplesVB\WordsProcessing\Concepts\WordsProcessingFields.vb region=radwordsprocessing_concepts_fields_4}} 

````C#
RadFlowDocumentEditor editor = new RadFlowDocumentEditor(new RadFlowDocument());
// Create an outer field with empty code fragment
FieldInfo outerFieldInfo = editor.InsertField(string.Empty, "if field result");
// Move the editor after the field start character
editor.MoveToInlineEnd(outerFieldInfo.Start);
// Create the a code fragment with a nested TIME field
editor.InsertText("IF ");
editor.InsertField("TIME \\@ HH", "time field result");
editor.InsertText(" < 12 \"Good morning!\" \"Good afternoon!\"");
outerFieldInfo.UpdateField();
Console.WriteLine(outerFieldInfo.GetResult()); // Output: Good afternoon!

````
````VB.NET
Dim editor As New RadFlowDocumentEditor(New RadFlowDocument())
' Create an outer field with empty code fragment
Dim outerFieldInfo As FieldInfo = editor.InsertField(String.Empty, "if field result")
' Move the editor after the field start character
editor.MoveToInlineEnd(outerFieldInfo.Start)
' Create the a code fragment with a nested TIME field
editor.InsertText("IF ")
editor.InsertField("TIME \@ HH", "time field result")
editor.InsertText(" < 12 ""Good morning!"" ""Good afternoon!""")
outerFieldInfo.UpdateField()
Console.WriteLine(outerFieldInfo.GetResult())
' Output: Good afternoon!

````

{{endregion}} 

![wordsprocessing-concepts-fields 002](images/wordsprocessing-concepts-fields002.png)

When calling the UpdateField() method all nested fields inside the code fragment of the field are also be updated. This is also true when using the UpdateFields() method of RadFlowDocument.
        

## FieldInfo Class

__FieldInfo__ is the main entry point when working with fields. It serves as "glue" between the start, separate and end field characters of a fields. Each field character also holds a reference to its FieldInfo class through the FieldInfo property.
        

>note The only way to create __FieldCharacter__ is by creating __FieldInfo__ instance. To preserve the document integrity all field characters should be inserted and removed from the document together. If the RadFlowDocumentEditor class is used for insertion – this is done automatically.
>


__FieldInfo__ exposes several properties and methods for working with fields:
        

* __Start__: A reference to the Start field character.
            

* __Separator__: A reference to the Separator field character.
            

* __End__: A reference to the End field character.
            

* __IsLocked__: Specifies if the field is locked. Locked fields are not updated.
            

* __IsDirty__: Specifies if the field should be updated before it is displayed. This property is useful when creating a document and you want to assure the field is updated when the document is opened by an application.
            

* __UpdateField()__: Recalculates the field result fragment and updates the Field property.
            

* __GetCode()__: Gets the current code fragment as string.
            

* __GetResult()__: Gets the current result fragment as string.
            

* __Field__: Gets the current Field object (e.g. DateField) associated to the field info. Note, that this property is updated every time the field is updated.
            

When exporting documents to DOCX format you can use the __IsDirty__ property of an individual fields or the __AutoUpdateFields__ property of the export settings of the __DocxFormatProvider__ which will cause the consumer to update the fields when the document is opened. More information about the export settings of the provider is available [here](51c1eb56-5131-4fe7-9ae7-b1f74b8b70d1).

# See Also

 * [Document model]({%slug winforms/wordsprocessing/model%})

 * [FieldCharacter]({%slug winforms/wordsprocessing/model/fieldcharacter%})

 * [RadFlowDocumentEditor]({%slug winforms/wordsprocessing/editing/radflowdocumenteditor%})
