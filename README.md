# velo-client

commands to install client:

for windows clients:

./<velo-server-file-linux_x> --config repack --msi <velo_windows_x.msi> <client_config_yaml_file> <name_of_new_msi_for_specific_client.msi>

e.g.
./velociraptor-v0.74.0-rc1-linux-amd64 --config repack --msi velociraptor-v0.74.0-rc1-windows-amd64.msi sonangol_client.OC4AG.config.yaml velo_windows_amd64_agent_sonan.msi

on client server: - install the new msi file:
msiexec.exe /i velo_windows_amd64_agent_sonan.msi /qn

for linux clients:


server url:
https://34.118.75.59/
