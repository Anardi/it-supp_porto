# Linux User and Permission Management

## Overview
This lab demonstrates Linux user and group administration, directory permission configuration, and access control verification. The objective is to ensure that authorized users can access shared resources while unauthorized users are restricted.

## Environment
- **Operating System:** Ubuntu Linux
- **Access:** Terminal with sudo privileges
- **Tools:** `useradd`, `groupadd`, `usermod`, `id`, `groups`, `chmod`, `chown`, `ls`, `namei`

## Objectives
- Create and manage Linux users and groups.
- Configure directory ownership and permissions.
- Verify authorized and unauthorized access.
- Troubleshoot common permission issues.
- Document findings and verify the resolution.

## Scenario 1: Create a User and Group

### Problem
A support team requires a shared directory that can only be accessed by authorized team members.

### Investigation and Configuration
Create a group for the support team:

```bash
sudo groupadd support-team
```

Create a user account:

```bash
sudo useradd -m -s /bin/bash support01
sudo passwd support01
```

Add the user to the support group:

```bash
sudo usermod -aG support-team support01
```

Verify the account and group membership:

```bash
id support01
groups support01
```

### Expected Result
The user `support01` should exist and belong to the `support-team` group.

## Scenario 2: Configure Directory Permissions

### Problem
The support team needs a shared directory where team members can read, write, and manage files, while other users cannot access the directory.

### Resolution
Create the shared directory:

```bash
sudo mkdir -p /srv/support-data
```

Set the directory owner and group:

```bash
sudo chown root:support-team /srv/support-data
```

Configure permissions:

```bash
sudo chmod 770 /srv/support-data
```

Verify the configuration:

```bash
ls -ld /srv/support-data
```

### Permission Explanation
The permission `770` means:

- **Owner (`7`):** Read, write, and execute.
- **Group (`7`):** Read, write, and execute.
- **Others (`0`):** No permissions.

The expected directory listing should show `root` as the owner and `support-team` as the group, with permissions equivalent to `drwxrwx---`.

## Scenario 3: Test User Access

### Authorized Access
Test whether `support01` can create a file in the shared directory:

```bash
sudo -u support01 touch /srv/support-data/test.txt
```

Verify that the file exists:

```bash
ls -l /srv/support-data
```

### Unauthorized Access
Create a test user who is not a member of `support-team`:

```bash
sudo useradd -m testuser
```

Attempt to create a file as that user:

```bash
sudo -u testuser touch /srv/support-data/unauthorized.txt
```

### Expected Result
- `support01` should be able to create files in the shared directory.
- `testuser` should receive a `Permission denied` error, provided the user is not a member of `support-team` and no other access mechanism grants permission.

If the test does not produce the expected result, inspect group membership and directory permissions before drawing a conclusion.

## Troubleshooting
Use the following commands to investigate permission issues:

```bash
id support01
groups support01
ls -ld /srv
ls -ld /srv/support-data
namei -l /srv/support-data
```

### Common Causes
| Issue | Possible Cause | Action |
|---|---|---|
| User cannot access the directory | Incorrect group membership | Verify with `id` and `groups` |
| User receives `Permission denied` | Incorrect directory permissions | Inspect with `ls -ld` |
| Group membership appears unchanged | Existing login session has not refreshed | Start a new login session and recheck |
| Access fails despite correct directory permissions | A parent directory lacks execute permission | Inspect the path with `namei -l` |

## Verification Checklist

- [ ] User account exists.
- [ ] User belongs to the correct group.
- [ ] Directory ownership is configured correctly.
- [ ] Directory permissions match the intended access policy.
- [ ] Authorized user can create a file.
- [ ] Unauthorized user cannot create a file.
- [ ] Results are documented with command output or screenshots.

## Incident Report
Complete this section after running the lab.

- **Problem:** Describe the permission or access issue.
- **Investigation:** List the commands used to identify the cause.
- **Findings:** Record the actual command output.
- **Root Cause:** Explain the confirmed cause of the issue.
- **Resolution:** Describe the changes made.
- **Verification:** Record the results of the access tests.

## Evidence
Store screenshots or terminal output in the repository's `evidence/` directory.

Suggested evidence:

1. User and group verification.
2. Directory ownership and permission listing.
3. Successful file creation by the authorized user.
4. Access denial for the unauthorized user.

Example, if the screenshot exists at the specified path:

```markdown
![User and group verification](../evidence/user-permission-test.png)
```

## Skills Demonstrated
- Linux user and group administration
- File ownership and permission management
- Access control verification
- Command-line troubleshooting
- Technical documentation and evidence collection

## Conclusion
This lab demonstrates how Linux user accounts, groups, and directory permissions can be configured to manage access to shared resources. The results should be based on actual command output and access tests performed in the lab environment.
