# TextClassifier Using an Editor Item Template
This subpath is for the ```ViewportAdornment``` sample.  It adds a square to the upper right-hand corner of the viewport within the editor.

## Steps
In this case, all that is necessary is to create a VSIX Project and add a template item for Viewport Adornment.

## Documentation
The simple description is found [here in the docs](https://docs.microsoft.com/en-us/visualstudio/extensibility/creating-an-extension-with-an-editor-item-template?view=vs-2019#create-a-viewport-relative-adornment-extension)

## Further Description

This code exports to MEF the viewport adornment and defines the layer on which it resides.

```Csharp
    [Export(typeof(IWpfTextViewCreationListener))]
    [ContentType("text")]
    [TextViewRole(PredefinedTextViewRoles.Document)]
    internal sealed class ViewportAdornment1TextViewCreationListener : IWpfTextViewCreationListener
    {
        // Disable "Field is never assigned to..." and "Field is never used" compiler's warnings. Justification: the field is used by MEF.
#pragma warning disable 649, 169

        /// <summary>
        /// Defines the adornment layer for the scarlet adornment. This layer is ordered
        /// after the selection layer in the Z-order
        /// </summary>
        [Export(typeof(AdornmentLayerDefinition))]
        [Name("ViewportAdornment1")]
        [Order(After = PredefinedAdornmentLayers.Caret)]
        private AdornmentLayerDefinition editorAdornmentLayer;
```

The `ViewportAdornment1` constructor is responsible for getting the layer on which the adornment will be drawn and 
registering for the viewport size changed event so that the adornment may be redrawn whenever
the viewport requires it.

This code handles that part.   There is other code within the class that defines the size of the adornment
that can be seen in the actual code.

```CSharp
        public ViewportAdornment1(IWpfTextView view)
        {
            if (view == null)
            {
                throw new ArgumentNullException("view");
            }

            this.view = view;

#region Pre-defined Adornment  // Region added by els to the sample to simplify the markdown
#endregion

            this.adornmentLayer = view.GetAdornmentLayer("ViewportAdornment1");

            this.view.LayoutChanged += this.OnSizeChanged;
        }
```

The code in `OnSizeChanged` handles the `LayoutChanged` event, by clearing and redrawing the adornment when necessary, using the pre-defined items

```Csharp
        private void OnSizeChanged(object sender, EventArgs e)
        {
            // Clear the adornment layer of previous adornments
            this.adornmentLayer.RemoveAllAdornments();

            // Place the image in the top right hand corner of the Viewport
            Canvas.SetLeft(this.image, this.view.ViewportRight - RightMargin - AdornmentWidth);
            Canvas.SetTop(this.image, this.view.ViewportTop + TopMargin);

            // Add the image to the adornment layer and make it relative to the viewport
            this.adornmentLayer.AddAdornment(AdornmentPositioningBehavior.ViewportRelative, null, null, this.image, null);
        }
```

See the actual code for all the further gory details.   

## Output
The output looks like this:

![Visual Studio Extension ViewportAdornment walkthru display](https://user-images.githubusercontent.com/7321962/100636399-94ea7080-3329-11eb-9a0e-dfd66b80f3d0.jpg)

**Note** If all the samples are done together they will interact (because all of the extensions will be loaded in the experimental instance). Here, you see BOTH the `TextAdornment` and `ViewportAdornment` extension both loaded.
