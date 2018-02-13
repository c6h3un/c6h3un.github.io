# AD
Some useful steps managing ad server with os - windows server 2012R2.


## Create user account

- Log in AD server
- Open _Users and Computer_ in server dashboard
- Create user in your domain
- Apply username/password to the account

## Password policy
- Default policy
- Change policy
- User password changing

## Backup AD

### Install Windows Server Backup
- Open _Add roles and features wizard_ in server dashboard

- Select _Windows Server Backup_ in features step

### Config Server Backup

#### Backup to NAS

- Preparation
	- NAS IP
	- Backup Path
	- Connection Method
	- (Optional) authentication account & password
- Select _Tool>Windows Server Backup_ in Server Dashboard to open up the tool
- Select _Backup Once_ for testing or _Backup Schedule_ for regular backup
- Select Next
- Select Custom , Next
- Add Item > System State
- OK > Next
- Remote Shared Folder
- enter path
- username & password
- backup