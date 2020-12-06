# Microsoft Visual Studio Extension Walk Throughs
This repo contains the full source code for each of the Visual Studio Extensibility Examples.  For the actual steps, see the documentation linked inside each sample, and KNOW that sample may be updated by Microsoft to be different or better than this repo).  For now at least, instead of having to go the docs and step through the examples, just clone this repo and enjoy.

I thought it may have been nice for Microsoft to include the code on github, but they didn't. so I'm adding it.

These were produced using Visual Studio 2019 16.8.2 and greater.

More samples will be added as time passes.

*ericleigh007 and ELSSystems make no representations regarding this repo's fitness for purpose.  It is offered only as an assistance in understanding the offical docs*

## Other Helpful Notes

When developing multiple extensions, the MEF cache which is used to combine Visual Studio functionality can get unweildy, or just plain corrupted.  
- One way to fix this is to [**Clear the MEF Component Cache**](Component-Cache.md).  The directory for your experimental instance, explained here, is used if you wish to
- [**Reset the experimental instance**](https://docs.microsoft.com/en-us/visualstudio/extensibility/creating-an-extension-with-a-menu-command?view=vs-2019#clean-up-the-experimental-environment)

In order to uninstall, disable or delete extensions to prevent anomalies when debugging with the experimental instance,
see:  [Managing Extensions in the Experimental Instance](Managing-Extensions.md)


