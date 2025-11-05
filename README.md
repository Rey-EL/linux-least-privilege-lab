# Linux Least Privilege Lab Report

## Objective
As part of the Google Cybersecurity Certificate, my objective in this lab was to perform a hands-on authorization audit of a user's project directory (`/home/researcher2/projects`) and remediate any findings to enforce the principle of least privilege. The goal was to ensure that files and directories were not exposed to unauthorized access or modification.

## Assessment Phase
Operating as the `researcher2` user, I began by establishing a baseline of the directory's security posture. I used `ls -la` to list all files, including hidden files, and their detailed permissions. This initial inspection revealed several critical misconfigurations:

*   **World-Writable File:** `project_k.txt` had permissions `-rw-rw-rw-`, granting write access to "other" users, which violates policy.
*   **Insecure Hidden File:** The hidden archive `.project_x.txt` had permissions `-rw--w----`. Policy required it to be read-only for user and group, but it retained write permissions.
*   **Excessive Directory Permissions:** The `drafts/` directory had permissions `drwx--x---`. This improperly granted group members execute access, allowing them to "enter" the directory and access its contents.

## Remediation Phase
I addressed each finding systematically by applying targeted `chmod` commands:

1.  **Securing the World-Writable File:** To remove write access for "others" from `project_k.txt`:
    ```bash
    chmod o-w /home/researcher2/projects/project_k.txt
    ```
    (New Permissions: `-rw-rw-r--`)

2.  **Hardening the Hidden Archive:** To set read-only permissions for user and group (using numeric mode 440) on `.project_x.txt`:
    ```bash
    chmod 440 /home/researcher2/projects/.project_x.txt
    ```
    (New Permissions: `-r--r-----`)

3.  **Securing the Directory:** To restrict access for the `drafts/` directory to only the user (using numeric mode 700):
    ```bash
    chmod 700 /home/researcher2/projects/drafts
    ```
    (New Permissions: `drwx------`)

## Result
Upon completing the remediation, I re-ran `ls -la` to verify all changes were applied. The directory now adheres to the principle of least privilege: all world-writable flags are gone, sensitive files are restricted, and private directories are inaccessible to unauthorized groups. This exercise successfully mitigated the identified access control risks.