# Window help
## 1. Switch Command Prompt and PowerShell on the Win+X Menu
- Open Windows Settings by pressing Win+I. Select Personalization.
- From the Personalization applet, select Taskbar. 
- Select the option to Replace Command Prompt with Windows PowerShell in the menu when I right-click the start button or press Windows key+X. 
- Select OK, and then close Windows Settings. 

## 2. CHECK permission user:
icacls folder/file name

PS F:\Website> icacls F:\Website
F:\Website BUILTIN\Administrators:(I)(F)
           CREATOR OWNER:(I)(OI)(CI)(IO)(F)
           NT AUTHORITY\SYSTEM:(I)(OI)(CI)(F)
           BUILTIN\Administrators:(I)(OI)(CI)(IO)(F)
           BUILTIN\Users:(I)(OI)(CI)(RX)

Successfully processed 1 files; Failed processing 0 files

## 3.
