``
Test
``
*Italic*

https://forum.vivaldi.net/topic/38561/windows-7-old-games-failing-to-run

## Issue

Games that use Microsoft Games Explorer are slow to launch or fail to launch altogether.

## Cause

When the game is launched, it executes this command:
C:\Windows\system32\rundll32.exe C:\Windows\system32\gameux.dll,GameUXShim \<gameGUID>;\<gameLauncher>;<gameLauncher's PID>

EEST Network Connections showed that rundll32.exe was connecting to 23.33.10.189.

Nirsoft's SmartSniff reported this:
````HTTP
GET /fwlink?linkid=30219&locale=en-US&clientType=VISTA_GAMES&clientVersion=6.1.2 HTTP/1.1
Cache-Control: no-cache
Connection: Keep-Alive
Pragma: no-cache
User-Agent: GamesWebServiceLocations
Host: go.microsoft.com

HTTP/1.1 302 Moved Temporarily
Location: http://movie.metaservices.microsoft.com/locater/WMServiceLocater.asmx/GetServiceLocationsForClient?locale=en-US&clientType=VISTA_GAMES&clientVersion=6.1.2
Server: Kestrel
Request-Context: appId=cid-v1:b47e5e27-bf85-45ba-a97c-0377ce0e5779
X-Response-Cache-Status: True
Content-Length: 0
Expires: Tue, 02 Nov 2021 18:31:06 GMT
Cache-Control: max-age=0, no-cache, no-store
Pragma: no-cache
Date: Tue, 02 Nov 2021 18:31:06 GMT
Connection: keep-alive
````

It's mentioned in the Vivaldi link that the system attempts to connect to https://games.metaservices.microsoft.com/games/SGamesWebService.asmx
In my situation, it connects to https://go.microsoft.com/fwlink?linkid=30219&locale=en-US&clientType=VISTA_GAMES&clientVersion=6.1.2 which expands into: http://movie.metaservices.microsoft.com/locater/WMServiceLocater.asmx/GetServiceLocationsForClient?locale=en-US&clientType=VISTA_GAMES&clientVersion=6.1.2

## Workaround
Create a registry string named 'Games' under this key:
HKEY_CLASSES_ROOT\Local Settings\Software\Microsoft\Windows\GameUX\ServiceLocation
You can leave it empty, or you can type localhost or 127.0.0.1 into it.

Just run this command, and it will create the empty value for you (no elevation needed):
````
reg add "HKEY_CLASSES_ROOT\Local Settings\Software\Microsoft\Windows\GameUX\ServiceLocation" /v Games
````
