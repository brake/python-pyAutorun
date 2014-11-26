pyAutorun
=========

Allows running portable applications (Windows) from removable drives basing on predefined profiles.

Based on these building blocks:
 - application descriptors
 - run configurations
 - computer names

Sets of building blocks should be defined and linked together in a config file (actually a INI-file). 

There are several kinds of building blocks:

### 1. Application description

Example:
```ini
; you can use macro "%(date)s" in <cmd> to insert date in format: YYYYMMDD
[app.Winrar]
; start action only if file specified does not exist
StartIfNotExists= c:\backup\backup%(date)s.rar
; start action only if file exists
;; StartIfExists= c:\backup\backup%(date)s.rar
; Name and path to executable and parameters
cmd = winrar.exe a -msinfo.dat;*.zip -k -os -r  c:\backup\backup%(date)s @backuplist.cfg
; Initial window state
; Can be one from: [min, max, default]
; Default value is "default" :)
winstate = default
; Wait for completion
; Default value is 0
wait = 1

[app.TotalCmd]
cmd = \app\totalcmd\totalcmd.exe /i=%commander_path%\config\totalcmd.ini /f=%commander_path%\config\ftp.ini
; Do not expand environment variables in "cmd"
; It need for Total Commander (but can be used for any application)
; because it expands variables by himself
expandenv = 0 

[app.TrueCrypt.work]
; Application will be started only if path or file exists
StartIfNotExists = %(SecureDrive)s:\
; Name and path to executable and parameters
cmd = \tcrpt\TrueCrypt.exe /q /a /v "c:\users\user\mystuff.dat" /l %(SecureDrive)s
; Wait for allplication completion   
wait = 1 

[app.Skype]
StartIfExists = %(SecureDrive)s:\
cmd = %(SecureDrive)s:\skype\SkypePortable.exe
wait = 0
winstate = default
workdir = %(SecureDrive)s:\skype 
```

### 2. Computer description 

Example:

```ini
[computer.work]
UseSecureDrive = 1
PrefferedDrvLetter = M
run1 = app.TrueCrypt.work
run2 = app.Skype

[computer.Home]
; runs application described in section "app.Winrar"
run1 = app.Winrar

; special section to be processed when computer is unknown
; i.e. when sectiom [computer.Computername] is not found
;[computer.\UNKNOWN/]

; special section to be processed on any computer
; this section processed before all other "computer." sections
[computer.\ALL/]
run1 = app.TotalCmd         
```

