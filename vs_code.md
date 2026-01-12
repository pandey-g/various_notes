 ***<------------why am I not able to open a file in vs code new tab instead it causing the same tab to get closed and then opening in its place------------>***

 ```
 The issue is caused by Visual Studio Code's "Preview Mode," which is enabled by default. When you single-click a file, it opens in a temporary preview tab, indicated by an italicized file name. If you then single-click another file, it replaces the content in that same preview tab. 
To prevent this from happening, you can either double-click files to open them permanently or disable Preview Mode completely in your settings. 
Solution 1: Use double-click or "Keep Open"
You can keep a file permanently in its own tab without disabling Preview Mode entirely.
Double-click a file: Double-clicking a file in the Explorer panel will open it in a permanent tab.
Double-click the tab: If a file is already open in Preview Mode, double-clicking its italicized tab will convert it into a permanent tab.
Use the "Keep Open" command: While in Preview Mode, you can right-click the file's tab and select Keep Open from the context menu. 
Solution 2: Disable Preview Mode in settings
If you never want to use Preview Mode, you can disable it permanently.
Open the Command Palette by pressing Ctrl+Shift+P (Windows/Linux) or Cmd+Shift+P (macOS).
Type Open User Settings and select Preferences: Open User Settings.
In the settings search bar, type enable preview.
Uncheck the boxes for the following two settings:
Workbench > Editor: Enable Preview: Controls whether files opened by a single-click appear in Preview Mode.
Workbench > Editor: Enable Preview From Quick Open: Controls whether files opened via the Quick Open menu (Ctrl+P) appear in Preview Mode. 

```

***<--------------code . is not always working for opening the vs code . I have to open vs code manually and then select shift + cmd + p and then select install 'code' command in PATH .---------------------->***

The "code ." command on macOS frequently breaks after a restart, and you have to reinstall it because of a macOS security feature called App Translocation. If VS Code is not in the /Applications folder, macOS runs it from a temporary location, which breaks the shell command symbolic link after you reboot your machine. 
Solution 1: Correctly install VS Code
The most common cause is running the VS Code app from your Downloads folder or another temporary location. 
Navigate to your Downloads folder in Finder.
Drag and drop the Visual Studio Code.app file into your /Applications folder.
Open VS Code from the /Applications folder.
Open the Command Palette (Cmd + Shift + P), search for Shell Command, and select Shell Command: Install 'code' command in PATH.
Close and reopen your terminal. The code . command should now work permanently. 
Solution 2: Manually create a permanent symbolic link
If the first solution doesn't fix the issue, you can manually fix the broken link.
Open your Terminal application.
Run the following command to remove the old, broken symbolic link. You may need to enter your administrator password.
```
sh
sudo rm /usr/local/bin/code
```
Use code with caution.

Create a new symbolic link that points to the correct location of the VS Code executable in your /Applications folder.
```
sh
sudo ln -s /Applications/Visual\ Studio\ Code.app/Contents/Resources/app/bin/code /usr/local/bin/code
```
Use code with caution.

Restart your terminal. The code . command should now work persistently. 
Solution 3: Add VS Code to your shell's profile
As a last resort, you can manually add the VS Code executable to your shell's profile. This is most reliable for users who don't want to use the default /Applications folder. 
For Zsh (the default macOS shell)
Open your .zshrc file in a text editor:
```
sh
open ~/.zshrc
```
Use code with caution.

Add the following line to the file, then save and close it:
```
sh
export PATH="$PATH:/Applications/Visual Studio Code.app/Contents/Resources/app/bin"
```
Use code with caution.

Apply the changes to your current terminal session:
```
sh
source ~/.zshrc
```
Use code with caution.

 
For Bash
Open your .bash_profile file in a text editor:
```
sh
open ~/.bash_profile
```
Use code with caution.

Add the same line, save the file, and then run source ~/.bash_profile