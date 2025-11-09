# linux-least-privilege-lab

This project demonstrates a hands-on audit and remediation of file permissions in a Linux environment to enforce the Principle of Least Privilege. This was a lab exercise from the Google Cybersecurity Certificate program.

---

## Objective

To perform a security audit on a user's project directory, identify file and directory permissions that violate security policy, and use `chmod` to apply the correct permissions.

---

## 1. Vulnerability Assessment

The first step was to assess the security posture of the `/home/researcher2/projects` directory. Using `ls -la`, I identified several permission misconfigurations.

**Findings:**
*   **World-Writable File:** `project_k.txt` had permissions `-rw-rw-rw-`, granting write access to all users on the system.
*   **Insecure Hidden File:** `.project_x.txt` had write permissions (`-rw--w----`) when policy required it to be read-only.
*   **Excessive Directory Permissions:** The `drafts/` directory (`drwx--x---`) improperly granted group members execute access, allowing them to traverse the directory.

## 2. Permission Hardening (Remediation)

I addressed each finding systematically using the `chmod` command to harden the permissions.

#### Securing the World-Writable File
Removed write access for the "other" category to align with policy.
```bash
chmod o-w /home/researcher2/projects/project_k.txt
# New Permissions: -rw-rw-r--
```

#### Hardening the Hidden Archive
Set the file to be read-only for the user and group, and assigned no permissions for "other".
```bash
chmod 440 /home/researcher2/projects/.project_x.txt
# New Permissions: -r--r-----
```

#### Securing the Directory
Restricted access to the `drafts/` directory to be accessible only by the owner.
```bash
chmod 700 /home/researcher2/projects/drafts
# New Permissions: drwx------
```

## 3. Final Result

After applying the changes, a final `ls -la` verification confirmed that all permissions now adhere to the principle of least privilege. The identified access control risks were successfully mitigated, securing the project directory from unauthorized access and modification.

---

## Summary of Skills

This lab demonstrates proficiency in fundamental Linux security administration:
*   **File System Auditing:** Using `ls -la` to inspect and analyze file and directory permissions.
*   **Access Control:** Understanding and applying the concepts of user, group, and other permissions.
*   **Permission Management:** Using the `chmod` command with both symbolic (`o-w`) and numeric (`440`, `700`) modes to enforce security policies.
*   **Principle of Least Privilege:** Practical application of a core cybersecurity principle in a Linux environment.

---

## Security Context

The Principle of Least Privilege, as demonstrated in this lab, is a fundamental security concept that directly addresses vulnerabilities such as "Broken Access Control" from the OWASP Top 10. By ensuring users and processes only have the minimum necessary permissions to perform their tasks, the attack surface is significantly reduced, preventing unauthorized access to sensitive data and system functions. This practice is crucial for maintaining the confidentiality, integrity, and availability of information systems.

---

## License

This project is licensed under the [MIT License](./LICENSE.md) - see the LICENSE.md file for details.

---

## Contributing

Contributions are welcome! If you have suggestions for improvements or want to enhance the lab, please feel free to open an issue or submit a pull request.
