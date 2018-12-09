# SIEM_WAF_BDS_integration
Integration between SIEM, Web Application firewall and Breach detection system to remediate an Advanced Persistant Threat


## FortiSIEM, FortiWEB, FortiSandbox Integration:

If FortiWeb doesn’t queue the uploaded files while being scanned by FortiSandbox. So, patient 0 will be infected for the malicious file will reach the back-end web server.

In an attempt to provide prevention this project provides a python script which can be used within FortiSIEM incident notification policy to delete the file from the web server.

 

## The scenario goes like this:

[Client]-----[FortiGate]------[FortiWeb]-----------[Web Server]

                                |                        |

                               [Fortisandbox]-------[FortiSIEM] 

FortiWeb receives an uploaded file from a client which is then sent to FortiSandbox for inspection
In the meanwhile, FortiWeb forwards the file to the Web server (a sample php upload page is attached)
FortiSandbox then issue a verdict which is sent to FortiSIEM via syslog. If the file is malicious a rule on FortiSIEM will trigger the python script (delete_malware.py) which will connect to the web server (Apache on Linux for this demo) search for the file by its md5 hash and delete it leaving a log in /tmp.
additionally the script puts the source IP uploading the malware into FortiGate Quarantine to block it from further communications.
 

## Now some mandatory prerequisites:

On the web server:
The credentials used in the script for ssh have to have the right privileges to delete files
The script has a constant which indicate a file system path from where to start searching for the file to delete
On FortiSIEM:
The incident used to trigger the script must provide the HashCode (md5) in incidents.xml
On FortiWeb:
A policy has to be set to forward uploaded files to FortiSandbox obviously

