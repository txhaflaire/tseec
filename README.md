# tseec

`tseec` is a macOS command-line utility designed for querying and exporting Transparency, Consent, and Control (TCC) entries. It simplifies access to macOS privacy settings, providing insights and allowing users to filter and export data for analysis or compliance purposes.

## Use Cases

- **Security Audits**: Review macOS TCC permissions for applications and services.
- **Compliance**: Export TCC data in JSON or plist format for documentation.
- **Debugging**: Identify applications and services with specific privacy permissions.
- **Automation**: Integrate with scripts to programmatically query and manage TCC data, or get this data programatically into your MDM solution.

## Functions Overview

- Query **system** or **user** TCC entries.
- Filter results by:
  - **Client**: Filter by specific application identifiers (e.g., `com.apple.Terminal`).
  - **Service**: Filter by service type (e.g., `kTCCServiceAccessibility`).
- Export TCC data to:
  - **JSON**: Human-readable format for easy sharing.
  - **plist**: Apple property list format for macOS tools.
- Support for exporting to files for further processing.

## Usage

`tseec` is located in `/usr/local/bin/tseec`

**NOTE:** In order to use `tseec` Full Disk Access needs to be given or a PPPC profile needs to be deployed.

```Usage: tseec [options]
Options:
  -h, --help      Show help information
  -s, --system    Query and display system TCC entries
  -u, --user      Query and display user TCC entries
  -e, --export    Export TCC entries (requires --json or --plist)
  --client        Filter results by client (e.g., --client com.apple.calculator)
  --service       Filter results by service (e.g., --service kTCCServiceAccessibility)
  --json          Specify JSON format for export
  --plist         Specify plist format for export
  --file          Specify if you want to export to a file

Example:
  tseec --user john.appleseed --export --json               Output user TCC entries in JSON format.
  tseec -u john.appleseed --file /tmp/tseec_export.plist    Exports user TCC entries in PLIST format to a specified filepath.
  tseec -s --file /tmp/tseec_export.json                    Exports system TCC entries in PLIST format to a specified filepath.
  ```

### Query TCC Entries
- Query **System** TCC entries specifically for the client `com.apple.Terminal` and export and have it piped to jq:
  ```
  sudo tseec -s --client com.apple.Terminal --export --json | jq
  ```

- Query **System** TCC entries specifically for the service `kTCCServiceScreenCapture` and export and have it piped to jq:
   ```
  sudo tseec -s --service kTCCServiceScreenCapture --export --json | jq
  ```

- Query **System** TCC entries for a specific service and print out the related client and the last modified date:
  ```
  sudo tseec -s --service kTCCServiceSystemPolicyAllFiles  --export --json | jq -r '.[] | "\(.client) - Last Modified: \(.lastModified)"'
  ```


  Each entry will show as the example below
  ```
    {
    "clientType": 0,
    "flags": 0,
    "lastModified": "2024-04-22 03:32:03",
    "authReason": "System Set",
    "client": "com.microsoft.teams",
    "bootUUID": "UNUSED",
    "authValue": 2,
    "indirectObjectIdentifier": "UNUSED",
    "service": "kTCCServiceScreenCapture",
    "lastReminded": "2024-04-22 03:32:03",
    "indirectObjectIdentifierType": 0,
    "authVersion": 1
  }
  ````

  

