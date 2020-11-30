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

![Visual Studio Extension Debug Log switch](https://user-images.githubusercontent.com/7321962/100649961-1eef0500-333b-11eb-97f6-b0cd4074769b.jpg)

So the directory for the Visual Studio experimental instance should look very similar to this:

![Visual Studio Experimental Instance Directory](https://user-images.githubusercontent.com/7321962/100649946-18608d80-333b-11eb-80b7-6a8cb552eac2.jpg)

Underneath this directory, you'll find a few directories which will require their files to be deleted.

![Visual Studio Component Level Cache Directory](https://user-images.githubusercontent.com/7321962/100649976-26161300-333b-11eb-8fff-bb48035a4773.jpg)

![Visual Studio Component Level Cache Delete ink](https://user-images.githubusercontent.com/7321962/100649996-2ca48a80-333b-11eb-8d23-b4fe473bfd4b.jpg)




