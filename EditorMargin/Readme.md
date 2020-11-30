# EditorMargin Using an Editor Item Template
This subpath is for the ```EditorMargin``` sample.  It adds a label to a margin in the editor viewport.  What may not be obvious is that Editor Margins are present
on ALL margins, where one may have thought "Margin" referred to the left of right margin, Margins can be 
on any side of the viewport.

## Steps
In this case, all that is necessary is to create a VSIX Project and add a template item for Editor Margin.

## Documentation
The simple description is found [here in the docs](https://docs.microsoft.com/en-us/visualstudio/extensibility/creating-an-extension-with-an-editor-item-template?view=vs-2019#create-a-margin-extension)

## Further Description

This code exports to MEF the Editor Margin and defines its parameters.  In our case, sort of un-intuitively, the "margin" is at the bottom
of the editor viewport, specified by `PredefinedMarginNames.Bottom`.

```Csharp
    [Export(typeof(IWpfTextViewMarginProvider))]
    [Name(EditorMargin1.MarginName)]
    [Order(After = PredefinedMarginNames.HorizontalScrollBar)]  // Ensure that the margin occurs below the horizontal scrollbar
    [MarginContainer(PredefinedMarginNames.Bottom)]             // Set the container to the bottom of the editor window
    [ContentType("text")]                                       // Show this margin for all text-based types
    [TextViewRole(PredefinedTextViewRoles.Interactive)]
    internal sealed class EditorMargin1Factory : IWpfTextViewMarginProvider
```

The `EditorMargin1` class inherits from `Canvas` so its contents are all specified as relative to that canvas.  Notice how
`ClipToBounds` is set to prevent the margin from overwriting any other content in the editor.

In this example, we are adding a `Label` to the content of the `Canvas`, but that content could be 
as simple or as complex as the specific extension's purpose requires.

Also, to be of much use, the `Label` would likely be defined and updated to indicate some status 
of the Editor or the extension, instead of being hard-coded as shown here.

```Csharp
        public EditorMargin1(IWpfTextView textView)
        {
            this.Height = 20; // Margin height sufficient to have the label
            this.ClipToBounds = true;
            this.Background = new SolidColorBrush(Colors.LightGreen);

            // Add a green colored label that says "Hello EditorMargin1"
            var label = new Label
            {
                Background = new SolidColorBrush(Colors.LightGreen),
                Content = "Hello EditorMargin1",
            };

            this.Children.Add(label);
        }
``` 

See the actual code for all the further gory details.   

## Output
The output looks like this:

![Visual Studio Extension EditorMargin output](https://user-images.githubusercontent.com/7321962/100644553-5bb6fe00-3333-11eb-9da7-7ab2f4caa896.jpg)

**Note** If all the samples are done together they will interact (because all of the extensions will be loaded in the experimental instance). Here, you see BOTH the `TextAdornment` and `ViewportAdornment` extension both loaded.
