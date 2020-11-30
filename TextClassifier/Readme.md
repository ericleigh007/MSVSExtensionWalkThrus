# TextClassifier Using an Editor Item Template
This subpath is for the ```TextClassifier``` sample.  It adds a highlight (of a type hardcoded within the extension) to the entire editor buffer.

## Steps
In this case, all that is necessary is to create a VSIX Project and add a template item for Text Classifier.

## Documentation
The simple description is found [here in the docs](https://docs.microsoft.com/en-us/visualstudio/extensibility/creating-an-extension-with-an-editor-item-template?view=vs-2019#create-a-classifier-extension)

## Further Description
This code associates the classifier's MEF components with a specific `ContentType`.

```Csharp
    [Export(typeof(IClassifierProvider))]
    [ContentType("text")] // This classifier applies to all text files.
    internal class EditorClassifier1Provider : IClassifierProvider
```

It should be noted that the `ContentType` **text**  is a base content type from which many, if not all of the content types in Visual Studio derive.
Therefore, associating with this content type will affect most files in the editor.

The `EditorClassifier1Format` class is responsible for defining the display of the classifier.  These lines export the information for the MEF component.

```Csharp
    [Export(typeof(EditorFormatDefinition))]
    [ClassificationType(ClassificationTypeNames = "EditorClassifier1")]
    [Name("EditorClassifier1")]
    [UserVisible(true)] // This should be visible to the end user
    [Order(Before = Priority.Default)] // Set the priority to be after the default classifiers
    internal sealed class EditorClassifier1Format : ClassificationFormatDefinition
```

The `ClassificationTypeDefinition` is exported here:

```Csharp
        [Export(typeof(ClassificationTypeDefinition))]
        [Name("EditorClassifier1")]
        private static ClassificationTypeDefinition typeDefinition;
```

The `EditorClassifier1Provider` is exported here as an `IClassifierProvider` interface:

```Csharp
    [Export(typeof(IClassifierProvider))]
    [ContentType("text")] // This classifier applies to all text files.
    internal class EditorClassifier1Provider : IClassifierProvider
```

The `GetClassificationSpans` method inside `EditorClassifier.cs` is called whenever the edtior buffer changes 
so that the extension can return the set of spans which are to be classified after that change.

In our simple case, we just return one representing the entire snspshot span.

```Csharp
        public IList<ClassificationSpan> GetClassificationSpans(SnapshotSpan span)
        {
            var result = new List<ClassificationSpan>()
            {
                new ClassificationSpan(new SnapshotSpan(span.Snapshot, new Span(span.Start, span.Length)), this.classificationType)
            };

            return result;
        }
```

See the actual code for all the further gory details.   

## Output
The output looks like this:

![EditorClassifier Result Console App](https://user-images.githubusercontent.com/7321962/100626523-d70db500-331d-11eb-9d0c-6bc1497d40a7.jpg)
