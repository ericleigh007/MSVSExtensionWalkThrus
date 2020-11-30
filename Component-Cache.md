# Clearing the Component Cache
Because building and rebuilding the same extension, or multiple versions of an extension can confuse the 
[Managed Extensibility Framework](https://docs.microsoft.com/en-us/visualstudio/extensibility/managed-extensibility-framework-in-the-editor?view=vs-2019) over time, one
may need to clear the cache which is built when Visual Studio scans for extensions.

To do so, there are a few ways, but all amount to simply removing the cached files under 
%appData%\Local\Microsoft\Visual Studio\'vs-dir'\Component Cache\

For `vs-dir`, it is most likely that the directory will be on the only one on your system with the suffix `Exp`, which is used to denote
the experimental instance of Visual Studio.
