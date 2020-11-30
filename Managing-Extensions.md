# Managing Extensions in the Experimental Instance
If you're using this code or other code and continually building sample extensions, it is likely you'll encounter the situation where Visual Studio has extensions
loaded that you no longer want loaded.

The Experimental Instance is a totally separate install of Visual Studio, so it will have its own extensions installed, as opposed to the ones that your development instance
possesses.  As a matter of fact, unless you add them manually, the Experimental Instance will only have extensions installed that you have built and run with your development
environment, and it WILL NOT REMOVE THEM OR DISABLE THEM.

It can be seen how this could become a problem, of instance, if you have one extension that highlights text, and yet another highlights it a different way.  

## Managing Extensions
In the experimental instance, just like any other instance of Visual Studio, you can see the installed extensions and disable them or uninstall them.  It is not only
necessary, but certainly recommended that you uninstall and even delete extensions you no longer require, as each additional extension will add to the startup time,
and increase the debug-loop time of your development.

To manage the extensions, just go to the `Extensions->Managed Extensions` menu, and select the `Installed` node of the tree.

[managed extensions plate]

Once an extension is disabled, or even uninstalled, you'll notice no difference until you exit Visual Studio, because extension changes are deferred until Visual Studio
is no longer running.

[deferred extension]

[modify block]


