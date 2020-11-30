# TextAdornment Using an Editor Item Template
This subpath is for the ```TextAdornment``` sample.  It adds an outline around certain text (determined by the extension itself) within the editor buffer.

## Steps
In this case, all that is necessary is to create a VSIX Project and add a template item for Text Adornment.

## Documentation
The simple description is found [here in the docs](https://docs.microsoft.com/en-us/visualstudio/extensibility/creating-an-extension-with-an-editor-item-template?view=vs-2019#create-a-text-relative-adornment-extension)

## Further Description
This code associates the adornment MEF components with a specific `ContentType` and establishes the layer on which the adornment is placed.

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

The `TextAdornment1` class is responsible for Initializing, responding to layout changes, and drawing the actual text adornment on the view in the chosen layer.

The constructor of the `TextAdornment` class gets the view, creates the layer, and then establishes its `OnLayoutChanged` method to handle any changes in the layout (because the adornment must be re-drawn on each such change)

```Csharp
        public TextAdornment1(IWpfTextView view)
        {
            if (view == null)
            {
                throw new ArgumentNullException("view");
            }

            this.layer = view.GetAdornmentLayer("TextAdornment1");

            this.view = view;
            this.view.LayoutChanged += this.OnLayoutChanged;
```

The `OnLayoutChanged` method responds to any changes, so the layer may be drawn again.

```CSharp
        internal void OnLayoutChanged(object sender, TextViewLayoutChangedEventArgs e)
        {
            foreach (ITextViewLine line in e.NewOrReformattedLines)
            {
                this.CreateVisuals(line);
            }
        }
```

See the actual code for all the further gory details.   Also, it is instructive to set a break within the extension in the experimental instance, so the way the extension code is called may be studied.  This helps one get a bit more of a feel for how things work behind the scenes.

## Output
The output looks like this:

![Visual Studio Extension TextAdornment Sample display](https://user-images.githubusercontent.com/7321962/100616876-a3c52900-3311-11eb-96b4-37767209993b.jpg)
