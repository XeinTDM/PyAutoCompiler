# PyAutoCompiler

PyAutoCompiler is a batch script designed to automate the compilation of Python scripts using PyInstaller. It renames the Python script before compiling it, ensuring a unique name each time. The script also cleans up by removing the `.spec` file created by PyInstaller, maintaining a tidy working directory.


## Features
- Automatically compiles Python scripts in its directory using PyInstaller.
- Renames the Python script to a unique name before compilation.
- Removes the `.spec` file created by PyInstaller to keep the directory clean.

## Usage
1. Place the batch script in the same directory as the Python script you wish to compile.
2. Ensure PyInstaller is installed in your environment. You can install it using pip:
```bash
pip install pyinstaller
```
3. Run the batch script. It will automatically rename, compile, and clean up your Python script.

## Script Details
Here's a breakdown of what the script does:

1. **Set up the environment:** The script enables delayed environment variable expansion and changes the working directory to the script's location.
```bash
@echo off
setlocal EnableDelayedExpansion
cd /d "%~dp0"
```

2. **Identify the Python script:** The script identifies the first Python script (`.py` file) in the directory.
```bash
for %%f in (*.py) do (
    set "originalFilename=%%~nxf"
)
```

3. **Main loop:** The script enters a loop where it performs the following steps:
- Generates a new unique name for the Python script.
- Renames the original Python script to this new name.
- Compiles the renamed Python script using PyInstaller.
- Waits for the compilation to complete.
- Deletes the .spec file created by PyInstaller.
- Renames the compiled Python script back to its original name.
```bash
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
```

4. **Clean up:** The script ends and restores the environment settings.
```bash
endlocal
```

## Notes
- This script assumes there is only one Python script in the directory. If there are multiple scripts, it will only process the first one it finds.
- Ensure you have the necessary permissions to rename and delete files in the directory.

## Contributions
Contributions are welcome! Please fork this repository and submit pull requests.
