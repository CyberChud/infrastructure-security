# Infrastructure Security Reference Lab

## High-Level Architecture

```mermaid
graph TD
    Internet((Internet))

    Firewall[OPNsense Firewall]

    Mgmt[Management Network]
    Internal[Internal Network]
    DMZ[DMZ Network]

    Ansible[Ansible Control Node]
    Linux[Linux Server]
    Web[Public Web Server]
    Wazuh[Wazuh SIEM]

    Internet --> Firewall

    Firewall --> Mgmt
    Firewall --> Internal
    Firewall --> DMZ

    Mgmt --> Ansible
    Internal --> Linux
    DMZ --> Web

    Linux --> Wazuh
    Web --> Wazuh
