# Activity Report: Least-Privilege Enforcement Access Control

---

## Section 1: Identify the Authorization Issue in the Environment

 I identified several permission misconfigurations within the `/home/researcher2/projects` directory. One file was world‑writable, a restricted file granted unnecessary group access, a hidden archived file allowed write permissions when it should have been read‑only, and the `drafts` directory incorrectly allowed group execute access. These issues created opportunities for unauthorized modification and violated the principle of least privilege. 

---

## Section 2: Document the Incident

A. I began by confirming that I was in the correct working directory using `pwd`, verifying that the current location was `/home/researcher2/projects`. This ensured that all permission changes would apply to the intended files and directories. 
![Screenshot 1 – Current working directory](screenshots/image%20(1).png)

B. Next, I listed all files—including hidden files—using `ls -la`. The output displayed the project files, the `drafts` directory, and the hidden file `.project_x.txt`. It showed the `project_k.txt` was world‑writable and `.project_x.txt` had write permissions that were unauthorized.
![Screenshot 2 – Full listing with permissions and hidden file](screenshots/image%20(3).png)

C. I then addressed `project_k.txt`, which had permissions set to `rw-rw-rw-`, allowing all users to write to the file. I removed write access for others using `chmod o-w project_k.txt`, ensuring that only the owner and group retained write permissions.
![Screenshot 3 – project_k.txt permissions before/after fixing world write](screenshots/image%20(4).png)

D. After that, I reviewed `project_m.txt`, which was intended to be accessible only by the user. However, the group had read permissions. I corrected this by running `chmod g-r project_m.txt`, ensuring that only the user retained read and write access.
![Screenshot 4 – Adjusting group access on project_m.txt](screenshots/image%20(5).png)

E. I then examined the hidden file `.project_x.txt`. As an archived file, it should have been read‑only for both the user and group. I removed write permissions while preserving read access, enforcing proper least‑privilege restrictions.
![Screenshot 5 – Correcting permissions on .project_x.txt](screenshots/image%20(6).png)

F. Finally, I inspected the `drafts` directory. Only `researcher2` should have been able to access it, meaning only the user should have execute permissions. I removed the group execute permission to ensure that only the owner could enter and use the directory.
![Screenshot 6 – Updating execute permissions on drafts directory](screenshots/image%20(7).png)

G. To conclude, I performed another detailed listing to verify all changes. I confirmed that no files were world‑writable, `project_m.txt` no longer granted group access, `.project_x.txt` was read‑only for the user and group, and the `drafts` directory no longer allowed group execution. This final verification confirmed that the environment now aligned with least‑privilege requirements.
![Screenshot 7 – Final verification of file and directory permissions](screenshots/image%20(8).png)

---

## Section 3: Recommend One or More Remediations for Authorization Issues

I recommend implementing periodic permission audits to detect world‑writable files, overly permissive group access, and unnecessary execute permissions on sensitive directories. Enforcing least‑privilege policies will ensure that users and groups only have the access they need. I also recommend using automated scripts to flag risky permissions and providing training so users understand how to set and maintain secure file and directory permissions.
