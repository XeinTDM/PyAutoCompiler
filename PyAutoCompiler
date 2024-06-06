@echo off
setlocal EnableDelayedExpansion

cd /d "%~dp0"

for %%f in (*.py) do (
    set "originalFilename=%%~nxf"
)

:loop
set "newname=%RANDOM%_script.py"
rename "%originalFilename%" "%newname%"

start /wait cmd /c pyinstaller --onefile "%newname%"
set "builtFile=dist\!newname:~0,-3!.exe"

:waitloop
if not exist "%builtFile%" (
    timeout /t 1 /nobreak >nul
    goto waitloop
)

del "!newname:~0,-3!.spec"

rename "%newname%" "%originalFilename%"

goto loop

endlocal
