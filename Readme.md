# PowerShell F7 Command Selector

This script allows you to display a searchable grid of your favorite commands by pressing the `F7` key in your PowerShell terminal. You can then select a command to insert it directly into the command line.

## Setup

### 1. Create the Commands File

First, you need to create a `my_commands.txt` file. This file will store your commands in a CSV format.

- The file must have three columns in this specific order: `Category`, `Command`, `Description`.
- The file should **not** have a header row.

Here is an example of the content for `my_commands.txt`:

```csv
System,Get-Process,Get a list of processes
System,Get-Service,Get a list of services
Git,git status,Check the status of the repository
Git,git pull,Fetch and merge from the remote repository
```

### 2. Add the Script to Your PowerShell Profile

Add the following PowerShell code to your PowerShell profile (e.g., `$PROFILE`):

```powershell
Set-PSReadlineKeyHandler -Key F7 -ScriptBlock {
    # Path to your commands file
    $CommandsPath = 'C:\Users\super_user\Documents\my_commands.txt'

    # 1. Read the file as CSV with new column order
    $CommandsList = Import-Csv -Path $CommandsPath -Header Category, Command, Description

    # 2. Display the list in GridView (display order in GridView based on this order)
    $selectedCommand = $CommandsList |
        Out-GridView -Title "Select Command (F7)" -PassThru

    if ($selectedCommand) {
        # 3. Extract only the Command column to insert into the command line
        [Microsoft.PowerShell.PSConsoleReadLine]::RevertLine()
        [Microsoft.PowerShell.PSConsoleReadLine]::Insert($selectedCommand.Command)
    }
}
```

## Usage

After adding this code to your PowerShell profile, you can press F7 to view and select commands.

## Notes

- Make sure your `my_commands.txt` file is set up with CSV format and columns in this order: Category, Command, Description
- The file should not have a header row.