# FirstMenuCommand Using a Template
This subpath is for the `TopLevelMenu` sample.  It adds an item to the **extensions** menu (prior to VS 2019, this added a 
command to the menu bar where **FILE/EDIT/VIEW**, etc, were placed).

## Steps
For this walkthrough, a VSIX Project is created and a template item is added.  After this, the code created by the
template is modified to place an item on the **extensions** menu.

## Documentation
The simple description is found [here in the docs](https://docs.microsoft.com/en-us/visualstudio/extensibility/adding-a-menu-to-the-visual-studio-menu-bar?view=vs-2019)

## Further Description

This code in the vsct file indicates the grouping and hierachy of menus and commands added.
Here, a top-level menu is created, tied to the `IDG_VS_MM_TOOLSADDINS` menu (previous to VS 2019 this was the top line, but in VS 2019 and later,
this is the **EXTENSIONS** menu).

Groups are used to form a hierarchy of menus and commands, so that the actual command is present in a lower-tier.
This shows how extensions can create a logical hierarchy so that many commands can be added in a logical
manner.

The heirarchy is defined in the XML file, but for simplicy, here's what it looks like:
```
    IDG_VS_MM_TOOLSADDINS Menu
        TopLevelMenu Menu          
            MyMenuGroup  Group
                TestCommandId Button

```

The XML to accomplish this is a bit more complex, but shouldn't be difficult to understand.
We've removed some comments to keep the code tightly spaced

```XML
    <Menus>
      <Menu guid="guidTopLevelMenuPackageCmdSet" id="TopLevelMenu" priority="0x700" type="Menu">
        <Parent guid="guidSHLMainMenu" id="IDG_VS_MM_TOOLSADDINS" />
        <Strings>
          <ButtonText>Test Menu</ButtonText>
        </Strings>
      </Menu>
    </Menus>
    
    <Groups>
      <Group guid="guidTopLevelMenuPackageCmdSet" id="MyMenuGroup" priority="0x0600">
        <Parent guid="guidTopLevelMenuPackageCmdSet" id="TopLevelMenu"/>
      </Group>
    </Groups>

    <Buttons>
      <Button guid="guidTopLevelMenuPackageCmdSet" id="TestCommandId" priority="0x0100" type="Button">
        <Parent guid="guidTopLevelMenuPackageCmdSet" id="MyMenuGroup" />
        <Icon guid="guidImages" id="bmpPic1" />
        <Strings>
          <ButtonText>Test Command</ButtonText>
        </Strings>
      </Button>
    </Buttons>
```

The `TestCommand` private constructor creates the command and defines an event handler for it.

```Csharp
        private TestCommand(AsyncPackage package, OleMenuCommandService commandService)
        {
            this.package = package ?? throw new ArgumentNullException(nameof(package));
            commandService = commandService ?? throw new ArgumentNullException(nameof(commandService));

            var menuCommandID = new CommandID(CommandSet, CommandId);
            var menuItem = new MenuCommand(this.Execute, menuCommandID);
            commandService.AddCommand(menuItem);
        }
``` 

First, it validates its arguments, `package` and `commandService` which is good practice to avoid extensions causing
unexplained problems in the Visual Studio tool.

Next it adds the command to the specified menu.

A handler is added to the class to cause the menu command to pop a dialog confirming that the command can be
handled.

```Csharp
        private void Execute(object sender, EventArgs e)
        {
            ThreadHelper.ThrowIfNotOnUIThread();
            string message = string.Format(CultureInfo.CurrentCulture, "Inside {0}.MenuItemCallback()", this.GetType().FullName);
            string title = "TestCommand";

            // Show a message box to prove we were here
            VsShellUtilities.ShowMessageBox(
                this.package,
                message,
                title,
                OLEMSGICON.OLEMSGICON_INFO,
                OLEMSGBUTTON.OLEMSGBUTTON_OK,
                OLEMSGDEFBUTTON.OLEMSGDEFBUTTON_FIRST);
        }
```

See the actual code for all the further gory details.   

## Output
When the template is first invoked, the output looks like this:

- The menu item is added at the top level (under EXTENSIONS)

![Visual Studio Extension - TopLevelMenu first level](https://user-images.githubusercontent.com/7321962/101289009-8a304f80-37f1-11eb-8445-083f044da318.jpg)

- A nested menu under that invokes the command that displays a test dialog.

![Visual Studio Extensions  - TopLevelMenu output](https://user-images.githubusercontent.com/7321962/101289011-8ac8e600-37f1-11eb-84ce-874e65a933bc.gif)


**Note** If all the samples are done together they will interact.  See the top-level readme for more information on managing the development of multiple extensions.
