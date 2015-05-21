# 4d-component-reset-debugger-window
Helper method to reset the debugger window bounds in v13, v14 and v15

Example
---
```
DEBUGGER_RESET_WINDOW
```

Source
---
```
Case of 
 : (Is_v13 )

  $path:=Get 4D folder+"4D Preferences v13.4DPreferences"

  If (Test path name($path)=Is a document)
   
   $dom:=DOM Parse XML source($path)
   ARRAY TEXT($windows;0)
   $window:=DOM Find XML element($dom;"preferences/internal_prefs_4d/windows/window";$windows)
   C_LONGINT($i)
   
    For ($i;1;Size of array($windows))
      $window:=$windows{$i}
      $name:=""
      DOM GET XML ATTRIBUTE BY NAME($window;"name";$name)
      If ($name="4ddebugger")
        DOM REMOVE XML ELEMENT($window)
      End if 
    End for 
   
    C_LONGINT($indentOption)
    XML SET OPTIONS($dom;XML Indentation;XML No indentation)
    ON ERR CALL("FILESYSTEM_ERROR")
    DOM EXPORT TO FILE($dom;$path)
    ON ERR CALL("")
    DOM CLOSE XML($dom)

  End if 

Else 

  $path:=Get 4D folder(Active 4D Folder)+\
  "4D Window Bounds v"+Substring(Application version;1;2)+Folder separator+\
  "coreDialog"+Folder separator+\
  "[projectForm]"+Folder separator+\
  "4ddebugger.json"

 If (Test path name($path)=Is a document)
    ON ERR CALL("FILESYSTEM_ERROR")
    DELETE DOCUMENT($path)
    ON ERR CALL("")
 End if 

End case 
```
