## Aim
To install Wazuh and configure it to detect and visualize brute-force login attempts on DVWA by creating custom detection rules and security dashboards.

## Requirements
- Wazuh open-source security platform
- Ubuntu Server
- Google Chrome browser
- Windows system with XAMPP
- DVWA web application and Wazuh agent

## Procedure / Steps
1. Install the Wazuh all-in-one deployment on Ubuntu:
   `curl -sO https://packages.wazuh.com/4.14/wazuh-install.sh && sudo bash ./wazuh-install.sh -a`
2. Find the Ubuntu server IP using `hostname -I`. Open `https://<IP_Address>` in Chrome and sign in to the Wazuh dashboard.
3. In **Server management → Endpoint summary**, choose **Add agent**, select Windows, enter the Wazuh server address and an agent name, then run the generated installation and service-start commands in PowerShell as Administrator.
4. Confirm that the Windows agent appears as active. Open **Threat intelligence → Threat hunting** to review collected Windows events.
5. Install DVWA under the XAMPP `htdocs` directory. Rename `config.inc.php.dist` to `config.inc.php`, configure the database user as `root` with a blank password as in the practical, start Apache and MySQL, and open `http://localhost/DVWA-master/login.php`.
6. Configure the Windows Wazuh agent's `ossec.conf` to monitor the XAMPP Apache `access.log`. Restart the Wazuh service.
7. In the Wazuh dashboard, open **Server Management → Manage rules files**, edit `local_rules.xml`, and add the custom rules described in the practical: a rule for DVWA `POST` login requests and a frequency rule that detects three attempts from the same source IP within 60 seconds (rule ID `100061`).
8. Use **Ruleset Test** with a POST entry from Apache `access.log` to check pre-decoding, decoding and rule matching.
9. Submit three failed DVWA logins within one minute. Check **Threat hunting** and confirm rule `100061` is generated.
10. Create a Metric visualization using `wazuh-alerts-*`, filter `rule.id = 100061`, and save it as **DVWA 3-Login Brute Force Alerts**.
11. Create an Area chart filtered on rule `100061`, with `@timestamp` as a date histogram, and save as **DVWA Attack Timeline**. Create a Pie chart using `data.srcip` terms and save as **DVWA Attacker IPs**.
12. Create the **DVWA Security Monitoring** dashboard, add the three visualizations, arrange them, and save.

## Expected Result
Wazuh collects the DVWA Apache access logs, identifies repeated failed login attempts from the same source IP within 60 seconds, generates rule `100061`, and displays the events in the Threat Hunting view and the **DVWA Security Monitoring** dashboard.

## Conclusion
In this practical, Wazuh was configured to monitor web application activity and detect brute-force login attempts using custom correlation rules. By integrating DVWA with Wazuh, collecting Apache access logs, and creating visual dashboards, the security events were successfully detected, analyzed, and presented in an organized manner.

## Screenshots

### 00-virtualbox-wazuh-server-and-wazuh-login
<img width="1253" height="705" alt="00-virtualbox-wazuh-server-and-wazuh-login" src="https://github.com/user-attachments/assets/a976cf05-d57c-436c-b0ad-20520322b646" />

### 01-wazuh-overview-dashboard
<img width="1253" height="705" alt="01-wazuh-deploy-new-agent-windows" src="https://github.com/user-attachments/assets/87d333cc-824e-452e-8fb1-ef118a940d8c" />

### 02-wazuh-deploy-new-agent-windows
<img width="1253" height="705" alt="02-wazuh-overview-dashboard" src="https://github.com/user-attachments/assets/440564e8-d8b4-47e5-a03c-c1d8b900d4d8" />

### 03-wazuh-agent-optional-settings-and-service-start
<img width="1253" height="705" alt="03-wazuh-agent-optional-settings-and-service-start" src="https://github.com/user-attachments/assets/65e99cb9-4756-464d-bdf6-ebce3976b172" />

### 04-wazuh-threat-hunting-dashboard
<img width="1253" height="669" alt="04-wazuh-threat-hunting-dashboard" src="https://github.com/user-attachments/assets/29f5f942-ac1f-4ef9-a577-4069e233df07" />

### 05-xampp-control-panel-and-dvwa-login
<img width="1253" height="630" alt="05-xampp-control-panel-and-dvwa-login" src="https://github.com/user-attachments/assets/727c8a75-261c-487e-a41d-7ac615ecc829" />

### 06-xampp-apache-logs-and-ossec-conf-localfile
<img width="1253" height="694" alt="06-xampp-apache-logs-and-ossec-conf-localfile" src="https://github.com/user-attachments/assets/02c70924-2191-4c07-9177-35656534818b" />

### 07-wazuh-rules-files-list
<img width="1253" height="705" alt="07-wazuh-rules-files-list" src="https://github.com/user-attachments/assets/44bd2c1c-9629-4d7f-a990-1f8cfaf52b7e" />

### 08-local-rules-xml-rule-100060-100061
<img width="1253" height="675" alt="08-local-rules-xml-rule-100060-100061" src="https://github.com/user-attachments/assets/7cc98200-7e22-4cf3-8d79-4677d6ce72e1" />

### 09-wazuh-ruleset-test-login-log
<img width="1253" height="672" alt="09-wazuh-ruleset-test-login-log" src="https://github.com/user-attachments/assets/d010589f-6b44-47f4-9667-14647e69b500" />

### 10-dvwa-login-failed-and-alert-rule-100061
<img width="1253" height="705" alt="10-dvwa-login-failed-and-alert-rule-100061" src="https://github.com/user-attachments/assets/5a218e66-00d5-4926-8211-662e737f96fd" />

### 11-wazuh-create-first-visualization
<img width="1253" height="670" alt="11-wazuh-create-first-visualization" src="https://github.com/user-attachments/assets/56d04718-d5a8-4702-a022-1bd5911f46fc" />

### 12-visualization-edit-filter-rule-id-100061
<img width="1253" height="678" alt="12-visualization-edit-filter-rule-id-100061" src="https://github.com/user-attachments/assets/9475735e-8875-4b00-a21b-9c20a5bacd6f" />

### 13-wazuh-visualizations-list
<img width="1253" height="627" alt="13-wazuh-visualizations-list" src="https://github.com/user-attachments/assets/6e3f7d3d-ebd4-481b-9112-3b9110903176" />

### 14-dvwa-security-monitoring-dashboard
<img width="1253" height="630" alt="14-dvwa-security-monitoring-dashboard" src="https://github.com/user-attachments/assets/cf0bf5ce-30bf-4111-a1bb-811334cc8d22" />

