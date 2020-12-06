# FirstMenuCommand Using a Template
This subpath is for the `FirstMenuCommand` sample.  It adds an item to the tools menu that causes a command to be executed when it is selected.

## Steps
For this walkthrough, a VSIX Project is created and a template item is added.  The template causes a dialog box to be 
displayed when the menu item is selected.  In the 2nd part of the walkthrough, a few lines of code
are modified to cause the code to invoke Notepad.exe instead of merely displaying a dialog box.

## Documentation
The simple description is found [here in the docs](https://docs.microsoft.com/en-us/visualstudio/extensibility/creating-an-extension-with-a-menu-command?view=vs-2019#create-a-menu-command)

## Further Description

This code in the extension manifest indicates what menu the command is placed on, its icon, and its textual description.
It is placed there by the template and would be modified if the designer wanted to modify the name of the command,
the icon, or the menu it is added to.

In this case, the menu to which to add the command is set as `IDM_VS_MENU_TOOLS` (the **Tools** menu).

The menu command added is simply **Invoke FirstCommand**.

```XML
    <Groups>
      <Group guid="guidFirstMenuCommandPackageCmdSet" id="MyMenuGroup" priority="0x0600">
        <Parent guid="guidSHLMainMenu" id="IDM_VS_MENU_TOOLS"/>
      </Group>
    </Groups>

    <!--Buttons section. -->
    <!--This section defines the elements the user can interact with, like a menu command or a button
        or combo box in a toolbar. -->
    <Buttons>
      <!--To define a menu group you have to specify its ID, the parent menu and its display priority.
          The command is visible and enabled by default. If you need to change the visibility, status, etc, you can use
          the CommandFlag node.
          You can add more than one CommandFlag node e.g.:
              <CommandFlag>DefaultInvisible</CommandFlag>
              <CommandFlag>DynamicVisibility</CommandFlag>
          If you do not want an image next to your command, remove the Icon node /> -->
      <Button guid="guidFirstMenuCommandPackageCmdSet" id="FirstCommandId" priority="0x0100" type="Button">
        <Parent guid="guidFirstMenuCommandPackageCmdSet" id="MyMenuGroup" />
        <Icon guid="guidImages" id="bmpPic1" />
        <Strings>
          <ButtonText>Invoke FirstCommand</ButtonText>
        </Strings>
      </Button>
    </Buttons>
```

The constructor in the `FirstCommand` class is responsible for associating the command with the menu.

```Csharp
        private FirstCommand(AsyncPackage package, OleMenuCommandService commandService)
        {
            this.package = package ?? throw new ArgumentNullException(nameof(package));
            commandService = commandService ?? throw new ArgumentNullException(nameof(commandService));

            var menuCommandID = new CommandID(CommandSet, CommandId);
            var menuItem = new MenuCommand(this.StartNotepad, menuCommandID);
            commandService.AddCommand(menuItem);
        }
``` 

First, it validates its arguments, `package` and `commandService` which is good practice to avoid extensions causing
unexplained problems in the Visual Studio tool.

Next it adds the command to the specified menu.

A handler is added to the class to cause the menu command to invoke the `Notepad.exe` executable.
As this is just a template/example, there isn't much error handling, but an actual extension might make sure
that the executable to be invoked exists and can be accessed -- perhaps by enclosing the invocation in a 
`try/catch` block.

```Csharp
        private void StartNotepad(object sender, EventArgs e)
        {
            ThreadHelper.ThrowIfNotOnUIThread();

            Process proc = new Process();
            proc.StartInfo.FileName = @"notepad.exe";
            // proc.StartInfo.FileName = @"calc.exe";
            proc.Start();
        }
```

See the actual code for all the further gory details.   

## Output
When the template is first invoked, the output looks like this:

- The extension is shown on the extensions installed menu.

![Visual Studio Extensions - FirstMenuCommand extension on menu](https://user-images.githubusercontent.com/7321962/101282755-c0100c80-37ce-11eb-9e32-dd1082c8c01b.png)

- The menu item is added.

![Visual Studio Extension - FirstMenuCommand Invoke FirstCommand](https://user-images.githubusercontent.com/7321962/101282754-bf777600-37ce-11eb-8b91-2dbb39c418ea.png)

- At first, it merely pops a dialog

![Visual Studio Extension FirstMenuCommand - invoked showing messagebox](https://user-images.githubusercontent.com/7321962/101282753-bf777600-37ce-11eb-824d-96ffdddc91fa.jpg)

- After modification, it invokes notepad.exe

![Visual Studio Extensions - FirstMenuCommand - invoke Notepad](https://user-images.githubusercontent.com/7321962/101282751-be464900-37ce-11eb-969d-9f44606ce06d.gif)

**Note** If all the samples are done together they will interact.  See the top-level readme for more information on managing the development of multiple extensions.
