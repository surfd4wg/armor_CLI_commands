##### armor_agent_3.0_CLI_commands #####

Get UUID from Linux system:
sudo dmidecode | grep UUID

##### Trend Agent #####
# All of the Trend statuses: (Put into a armorstatus.sh file, chmod +x, then ./armorstatus.sh)
sudo /opt/armor/armor trend status | sed -n '/time=/!p' | sed 's/[^{]*\({.*\)/\1/' | jq '.' > armorstatus.txt 2>/dev/null
sudo jq '{ "hostName", "lastIPUsed", "platform" }' armorstatus.txt 2>/dev/null
sudo jq  '{"integrityMonitoring state": .integrityMonitoring.state, "integrityMonitoring status": .integrityMonitoring.moduleStatus.agentStatus, "integrityMonitoringmessage": .integrityMonitoring.moduleStatus.agentStatusMessage}' armorstatus.txt 2>/dev/null
sudo jq  '{"intrusionPrevention state": .intrusionPrevention.state, "intrusionPrevention status": .intrusionPrevention.moduleStatus.agentStatus, "intrusionPrevention message": .intrusionPrevention.moduleStatus.agentStatusMessage}' armorstatus.txt 2>/dev/null
sudo jq  '{"antiMalware state": .antiMalware.state, "antiMalware status": .antiMalware.moduleStatus.agentStatus, "antiMalware message": .antiMalware.moduleStatus.agentStatusMessage}' armorstatus.txt 2>/dev/null

##### FIM #####
sudo /opt/armor/armor fim list-assigned-rules | sed -n '/time=/!p' | sed 's/[^{]*\({.*\)/\1/' | jq '.' > fimassignedrules.txt 2>/dev/null
/opt/armor/armor trend recommendation-scan
/opt/armor/armor fim on auto-apply-recommendations=on

##### IDS/IPS #####
sudo /opt/armor/armor ips list-assigned-rules | sed -n '/time=/!p' | sed 's/[^{]*\({.*\)/\1/' | jq '.' > ipsassignedrules.txt 2>/dev/null

/opt/armor/armor ips list-available-rules
/opt/armor/armor ips list-assigned-rules

#IDS
/opt/armor/armor ips detect  # Turn on detection mode
/opt/armor/armor ips detect auto-apply-recommendations=on

#IPS
/opt/armor/armor ips prevent # Turn on prevention mode
/opt/armor/armor ips prevent 
/opt/armor/armor ips prevent auto-apply-recommendations=on
/opt/armor/armor ips off # Turn off prevention mode

##### Vulnerability Management #####
/opt/armor/armor vuln service-restart
/opt/armor/armor vuln sync-agent-id
/opt/armor/armor vuln service-check

##### Logging #####
/opt/armor/armor logging sync-file-paths
/opt/armor/armor logging sync-event-logs (not working)

/opt/armor/armor logging apache-enable
/opt/armor/armor logging add-file-paths /var/www/html/
/opt/armor/armor logging apache-describe-config
/opt/armor/armor logging apache-sync-config
/opt/armor/armor logging apache-add-access-paths /var/www/html/
/opt/armor/armor logging apache-add-error-paths /var/www/html/

#-----

# All in one (Trent Agent Status - Malware):
sudo /opt/armor/armor trend status | sed -n '/time=/!p' | sed 's/[^{]*\({.*\)/\1/' | jq  '{"antiMalware state": .antiMalware.state, "antiMalware status": .antiMalware.moduleStatus.agentStatus, "antiMalware message": .antiMalware.moduleStatus.agentStatusMessage}'

# All in one (Trent Agent Status - IDS/IPS):
sudo /opt/armor/armor trend status | sed -n '/time=/!p' | sed 's/[^{]*\({.*\)/\1/' | jq  '{"intrusionPrevention state": .intrusionPrevention.state, "intrusionPrevention status": .intrusionPrevention.moduleStatus.agentStatus, "intrusionPrevention message": .intrusionPrevention.moduleStatus.agentStatusMessage}'

# All in one (Trent Agent Status - FIM):
sudo /opt/armor/armor trend status | sed -n '/time=/!p' | sed 's/[^{]*\({.*\)/\1/' | jq  '{"integrityMonitoring state": .integrityMonitoring.state, "integrityMonitoring status": .integrityMonitoring.moduleStatus.agentStatus, "integrityMonitoringmessage": .integrityMonitoring.moduleStatus.agentStatusMessage}'

# All in one (Trent Agent Status - Malware):
sudo /opt/armor/armor trend status | sed -n '/time=/!p' | sed 's/[^{]*\({.*\)/\1/' | jq '{ "hostName", "lastIPUsed", "platform" }'
