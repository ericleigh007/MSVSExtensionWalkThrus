# Clearing the Component Cache
Because building and rebuilding the same extension, or multiple versions of an extension can confuse the 
[Managed Extensibility Framework](https://docs.microsoft.com/en-us/visualstudio/extensibility/managed-extensibility-framework-in-the-editor?view=vs-2019) over time, one
may need to clear the cache which is built when Visual Studio scans for extensions.

**Note** a new feature in Visual Studio 2019 is that it no longer scans everywhere for extensions.  To save time, it only scans extension directories.  This is rumored to speed up startup time, and it avoids finding extensions that are no longer installed, or not enabled.

To do so, there are a few ways, but all amount to simply removing the cached files under 
`%appData%\Local\Microsoft\Visual Studio\'vs-dir'\Component Cache\`

For `vs-dir`, it is most likely that the directory will be on the only one on your system with the suffix `Exp`, which is used to denote
the experimental instance of Visual Studio.  That directory suffix is set in the Debug settings for the experimental instance, which is part of the Visual Studio Extensibility
extension workings.

[debug settings plate]

So the directory for the Visual Studio experimental instance should look very similar to this:

[exp dir plate]

Underneath this directory, you'll find a few directories which will require their files to be deleted.

[exp dir cache delete plate]




