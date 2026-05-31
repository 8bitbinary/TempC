@echo off
:: Request admin rights immediately
net session >nul 2>&1
if %errorlevel% neq 0 (
    powershell -NoProfile -Command "Start-Process '%~f0' -Verb RunAs"
    exit /b
)

setlocal EnableExtensions EnableDelayedExpansion
color 0B
title TempC by 8bit_binary freeware lisence

:: Log file on Desktop
set "LOGFILE=%USERPROFILE%\Desktop\TempC_log.txt"

:: Start logging
call :log "=================================================="
call :log "TempC started"
call :log "This is a batch file that cleans Temp files,runs sfc scan,empties the recycle bin"
call :log "Title: TempC by 8bit_binary freeware lisence"
call :log "Log file: %LOGFILE%"
call :log "=================================================="
call :log ""

call :log "Cleaning %TEMP%..."
del /f /s /q "%temp%\*" >> "%LOGFILE%" 2>&1
for /d %%i in ("%temp%\*") do rd /s /q "%%i" >> "%LOGFILE%" 2>&1

call :log "Cleaning C:\Windows\Temp..."
del /f /s /q "C:\Windows\Temp\*" >> "%LOGFILE%" 2>&1
for /d %%i in ("C:\Windows\Temp\*") do rd /s /q "%%i" >> "%LOGFILE%" 2>&1

call :log "Emptying Recycle Bin..."
powershell -NoProfile -Command "Clear-RecycleBin -Force" >> "%LOGFILE%" 2>&1

:: Display the manual instruction messages
echo.
call :log "TempC is 90% completed please run this command in cmd"
call :log "To do the last 10%, and the code in question is 'sfc /scannow'"
call :log "Opening a new Admin CMD window for you now..."

:: Launch a new elevated CMD window, resize it to 58x8, and stay open
start "SFC Ready Window" cmd /k "mode con: cols=58 lines=8 && echo Please type: 'sfc /scannow' optional"

call :log ""
call :log "TempC finished."
call :log "=================================================="
pause
exit /b

:log
echo %~1
echo %~1>> "%LOGFILE%"
exit /b