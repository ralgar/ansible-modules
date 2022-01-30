<!-- Project Title and Description -->
# cfssl_certificate

### Description

- The `cfssl_certificate` module requests an SSL/TLS certificate from a CFSSL API endpoint.
- The module can also request a certificate chain by providing the `chain_path` parameter.
- The module is fully idempotent, only making changes when required.

### Future Plans

- Ability to revoke certificates
- PKCS12 bundling


<!-- Requirements -->
## Prerequisites

- Ansible
- A CFSSL Server


<!-- Installation Instructions -->
## Installation

**There are a couple of ways to install this module, depending on your requirements.**

1. Add this repository directly as a git submodule:
   ```
   $ cd ansible/roles/<your-role>
   $ git submodule add https://github.com/ralgar/ansible-cfssl-certificate library
   ```
2. Clone this repository and copy the module file to your library:
   ```
   $ git clone https://github.com/ralgar/ansible-cfssl-certificate
   $ cp ansible-cfssl-certificate/cfssl_certificate.py ansible/library
   ```


<!-- Parameter Descriptions -->
## Parameters

- **cert_path:**
  - description: Absolute path for the certificate file.
  - required: true
  - type: path
- **cfssl_host:**
  - description: Hostname or IP address of the CFSSL API server.
  - default: localhost
  - type: str
- **cfssl_port:**
  - description: Port number of the CFSSL API server.
  - default: 8888
  - type: str
- **chain_path:**
  - description:
    - Absolute path for the certificate chain file.
    - If this value is not supplied, then no chain will be created.
  - required: false
  - type: path
- **common_name:**
  - description: The commonName field of the certificate subject.
  - required: true
  - type: str
- **hosts:**
  - description:
    - Valid hosts (Subject Alternative Names) for the certificate.
    - Can be an IP address, hostname, or FQDN.
  - required: true
  - type: list
  - elements: str
- **key_path:**
  - description: Absolute path for the private key file.
  - required: true
  - type: path
- **names:**
  - description:
    - Key/Value pairs that will be present in the certificate's subject name field.
    - Possible and common values are:
      - "C" (Country Name) - A two-letter country abbreviation.
      - "ST" (State) - A full-length Province or State name.
      - "L" (Locality) - A full-length City or Region name.
      - "O" (Organization) - The full name of your organization.
      - "OU" (Organizational Unit) - An organizational identifier (ex. 'Webserver').
      - "E" (Email Address) - Email address to associate with the certificate.
  - required: true
  - type: dict
- **profile:**
  - description: The CFSSL signing profile to use.
  - required: true
  - type: str


<!-- Usage Examples -->
## Examples

```yaml
- name: Generate a default certificate using a local CFSSL endpoint
  cfssl_certificate:
    cert_path: /var/pki/cert.pem
    key_path: /var/pki/key.pem
    common_name: My Certificate
    names:
      C: US
      ST: California
      L: San Francisco
      O: My HomeNet
      OU: Default Cert
    hosts:
      - default
      - default.home.internal
      - 192.168.1.50

- name: Generate a server certificate using a remote CFSSL endpoint
  cfssl_certificate:
    cfssl_host: pki.myorg.internal
    cert_path: /var/pki/cert.pem
    key_path: /var/pki/key.pem
    common_name: Server 1
    names:
      C: US
      ST: California
      L: San Francisco
      O: My Organization
      OU: Web Server
    hosts:
      - server1
      - server1.myorg.internal
      - 10.0.1.1

- name: Generate a client certificate using a remote CFSSL endpoint
  cfssl_certificate:
    cfssl_host: pki.myorg.internal
    cert_path: /var/pki/cert.pem
    key_path: /var/pki/key.pem
    common_name: Client 1
    names:
      C: US
      ST: California
      L: San Francisco
      O: My Organization
      OU: Client Machine
    hosts:
      - client1
      - client1.myorg.internal
      - 10.0.2.1
```


<!-- License -->
## License

Distributed under the GNU GPL v3.0 License. See `LICENSE` for more info.