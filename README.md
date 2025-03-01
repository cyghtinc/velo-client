# velo-client

commands to install client:

download the agent:
wget https://github.com/cyghtinc/velo-client/raw/refs/heads/main/velo_windows_amd64_agent_sonan-final.msi

or 

Invoke-WebRequest -Uri "https://github.com/cyghtinc/velo-client/raw/refs/heads/main/velo_windows_amd64_agent_sonan-final.msi" -UseBasicParsing

Invoke-WebRequest -Uri "https://github.com/cyghtinc/velo-client/raw/refs/heads/main/velo_windows_amd64_agent_sonan-final.msi" -UseBasicParsing -OutFile c:\users\velo_windows_amd64_agent_sonan-final.msi

if there is a problem with ssl/tls certificate
use the following command

Add-Type @"using System.Net;
using System.Security.Cryptography.X509Certificates;
public class TrustAllCertsPolicy : ICertificatePolicy {
    public bool CheckValidationResult(ServicePoint srvPoint, X509Certificate certificate, WebRequest request, int certificateProblem) {
        return true;
    }
}"@
[System.Net.ServicePointManager]::CertificatePolicy = New-Object TrustAllCertsPolicy

$webclient = New-Object System.Net.WebClient
$webclient.DownloadFile("https://github.com/cyghtinc/velo-client/raw/refs/heads/main/velo_windows_amd64_agent_sonan-final.msi", "C:\Users\velo_windows_amd64_agent_sonan-final.msi")


on client server: - install the new msi file: (must file full path of the msi)

msiexec.exe /i c:\users\velo_windows_amd64_agent_sonan.msi /qn

or with logs

msiexec.exe /i velo_windows_amd64_agent_sonan.msi /qn /l*v log.log


creating new msi deploy file for windows clients:

./<velo-server-file-linux_x> --config repack --msi <velo_windows_x.msi> <client_config_yaml_file> <name_of_new_msi_for_specific_client.msi>

e.g.

./velociraptor-v0.74.0-rc1-linux-amd64 --config repack --msi velociraptor-v0.74.0-rc1-windows-amd64.msi sonangol_client.OC4AG.config.yaml velo_windows_amd64_agent_sonan.msi



for linux clients:


server url:
https://34.118.75.59/
