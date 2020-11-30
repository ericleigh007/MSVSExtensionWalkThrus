# TextAdornment Using an Editor Item Template
This subpath is for the ```TextAdornment``` sample.  It adds an outline in the text in the buffer.

## Steps
In this case, all that is necessary is to create a VSIX Project and add a template item for Text Adornment.

## Documentation
The simple description is found [here in the docs](https://docs.microsoft.com/en-us/visualstudio/extensibility/creating-an-extension-with-an-editor-item-template?view=vs-2019#create-a-text-relative-adornment-extension)

## Further Description
This code associates the adornment with a specific `ContentType` and establishes the layer on which the adornment is placed.
```Csharp
    [Export(typeof(IWpfTextViewCreationListener))]
    [ContentType("text")]
    [TextViewRole(PredefinedTextViewRoles.Document)]
    internal sealed class TextAdornment1TextViewCreationListener : IWpfTextViewCreationListener
    {
        // Disable "Field is never assigned to..." and "Field is never used" compiler's warnings. Justification: the field is used by MEF.
#pragma warning disable 649, 169

        /// <summary>
        /// Defines the adornment layer for the adornment. This layer is ordered
        /// after the selection layer in the Z-order
        /// </summary>
        [Export(typeof(AdornmentLayerDefinition))]
        [Name("TextAdornment1")]
        [Order(After = PredefinedAdornmentLayers.Selection, Before = PredefinedAdornmentLayers.Text)]
        private AdornmentLayerDefinition editorAdornmentLayer;
```

It should be noted that the `ContentType` **text**  is a base content type from which many, if not all of the content types in Visual Studio derive.
Therefore, associating with this content type will affect most files in the editor.

## Output
The output looks like this:

![Visual Studio Extension TextAdornment Sample display](https://user-images.githubusercontent.com/7321962/100616876-a3c52900-3311-11eb-96b4-37767209993b.jpg)
