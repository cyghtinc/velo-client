# velo-client

commands to install client:

download the agent:
wget https://github.com/cyghtinc/velo-client/raw/refs/heads/main/velo_windows_amd64_agent_sonan-final.msi

or 

Invoke-WebRequest -Uri "https://github.com/cyghtinc/velo-client/raw/refs/heads/main/velo_windows_amd64_agent_sonan-final.msi" -UseBasicParsing

Invoke-WebRequest -Uri "https://github.com/cyghtinc/velo-client/raw/refs/heads/main/velo_windows_amd64_agent_sonan-final.msi" -UseBasicParsing -OutFile c:\users\velo_windows_amd64_agent_sonan-final.msi

Invoke-WebRequest -Uri "https://github.com/cyghtinc/velo-client/raw/refs/heads/main/velobai.msi" -UseBasicParsing -OutFile c:\users\velobai.msi


if there is a problem with ssl/tls certificate
use the following command

Add-Type @"
using System.Net;
using System.Security.Cryptography.X509Certificates;
public class TrustAllCertsPolicy : ICertificatePolicy {
    public bool CheckValidationResult(ServicePoint srvPoint, X509Certificate certificate, WebRequest request, int certificateProblem) {
        return true;
    }
}
"@
[System.Net.ServicePointManager]::CertificatePolicy = New-Object TrustAllCertsPolicy



$webclient = New-Object System.Net.WebClient
$webclient.DownloadFile("https://github.com/cyghtinc/velo-client/raw/refs/heads/main/velo_windows_amd64_agent_sonan-final.msi", "C:\Users\velo_windows_amd64_agent_sonan-final.msi")


#also can work
$webclient.DownloadFile("https://ufile.io/s1tsyh0q", "C:\Users\velo_windows_amd64_agent_sonan-final.msi")




on client server: - install the new msi file: (must give file full path of the msi)

msiexec.exe /i c:\users\velo_windows_amd64_agent_sonan.msi /qn

or with logs

msiexec.exe /i velo_windows_amd64_agent_sonan.msi /qn /l*v log.log

#sometimes the config file is incorrect, need to modify it and remove the ":80" from both server config file
and also from client config file





creating new msi deploy file for windows clients:

./<velo-server-file-linux_x> --config repack --msi <velo_windows_x.msi> <client_config_yaml_file> <name_of_new_msi_for_specific_client.msi>

e.g.

./velociraptor-v0.74.0-rc1-linux-amd64 --config repack --msi velociraptor-v0.74.0-rc1-windows-amd64.msi sonangol_client.OC4AG.config.yaml velo_windows_amd64_agent_sonan.msi

new command syntax if above not working:

./velociraptor-v0.74.1-linux-amd64 --config server.config.yaml config repack \
  --msi velociraptor-v0.74.0-rc1-windows-amd64.msi \
  client.spincitysolutions-OJRTG.config.yaml \
  velo_windows_amd64_agent_spincity.msi


for linux clients:


server url:
https://34.118.75.59:80/


the config file for each client can be downloaded from velo GUI in the home page.
under current orgs.
<img width="1670" height="774" alt="image" src="https://github.com/user-attachments/assets/9dd8edff-e9bc-4370-8302-6aa366cda917" />
