# wifi-pswd

@echo off

rem List Wi-Fi profiles and extract profile names
for /F "tokens=2 delims=: " %%i in ('netsh wlan show profiles ^| findstr "All User Profile"') do (
    echo Wi-Fi Profile: %%i
    netsh wlan show profile name="%%i" key=clear | findstr "Key Content"
) > wifi_passwords.txt

rem Display completion message
echo Wi-Fi profile passwords have been saved to wifi_passwords.txt
pause
