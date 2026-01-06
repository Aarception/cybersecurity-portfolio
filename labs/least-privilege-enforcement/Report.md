# Activity Report: Least-Privilege Enforcement Access Control

| Section 1: Identify the authorization issue in the environment |  |
| :---- | ----- |
| I identified that the `/home/researcher2/projects` directory contained several misconfigured permissions. One file allowed write access to all users, a restricted file allowed unnecessary group access, a hidden archived file permitted writes when it should be read‑only, and the `drafts` directory granted execute access to the group. These issues exposed project data to potential unauthorized modification and violated the principle of least privilege. |  |
|  |  |

| Section 2: Document the incident |
| :---- |
| I first confirmed that I was working in the correct directory by running `pwd` and verifying that the current location was `/home/researcher2/projects`. This ensured that any permission changes I made would affect the intended files and directories. |

|  |
| !\[Screenshot 1 – Current working directory](screenshots/image%20(1).png) |
|  |

|  |
| Next, I listed all files, including hidden ones, with detailed permissions using `ls -la`. This output showed the project files, the `drafts` directory, and the hidden file `.project\\\_x.txt`. From this listing, I observed that `project\\\_k.txt` was world‑writable and `.project\\\_x.txt` allowed write access that should not have been granted. |

|  |
| !\[Screenshot 2 – Full listing with permissions and hidden file](screenshots/image%20(3).png) |
|  |

|  |
| I then focused on `project\\\_k.txt`, which had `rw-rw-rw-` permissions, meaning all users could write to it. To remove write access for others, I used `chmod o-w project\\\_k.txt` and confirmed that only the user and group retained write permissions. |

|  |
| !\[Screenshot 3 – project\_k.txt permissions before/after fixing world write](screenshots/image%20(4).png) |
|  |

|  |
| After that, I reviewed `project\\\_m.txt`, which was meant to be restricted so that only the user could read and write it. The permissions allowed the group to read the file. I corrected this by using `chmod g-r project\\\_m.txt`, ensuring that only the user had read and write access. |

|  |
| !\[Screenshot 4 – Adjusting group access on project\_m.txt](screenshots/image%20(5).png) |
|  |

|  |
| I then turned to the hidden file `.project\\\_x.txt`. This file had been archived and should only be readable by the user and group, not writable. I used `chmod` to remove write permissions while keeping read access for the user and group, effectively enforcing read‑only status on the hidden file. |

|  |
| !\[Screenshot 5 – Correcting permissions on .project\_x.txt](screenshots/image%20(6).png) |
|  |

|  |
| Finally, I examined the `drafts` directory. The requirement was that only `researcher2` should be able to access this directory and its contents, which means only the user should have execute permissions. I removed the group execute permission on `drafts` so that only the owner could enter and use the directory. |

|  |
| !\[Screenshot 6 – Updating execute permissions on drafts directory](screenshots/image%20(7).png) |
|  |

|  |
| To conclude, I ran another detailed listing to verify all changes. I confirmed that no files were world‑writable, `project\\\_m.txt` no longer granted group access, `.project\\\_x.txt` was read‑only for the user and group, and the `drafts` directory no longer allowed group execution. This final check validated that the permissions now aligned with the intended authorization policy. |

|  |
| !\[Screenshot 7 – Final verification of file and directory permissions](screenshots/image%20(8).png) |
|  |

| Section 3: Recommend one or more remediations for authorization issues |
| :---- |
| I recommend implementing periodic permission audits to detect world‑writable files, overly permissive group access, and unnecessary execute permissions on sensitive directories. Enforcing least‑privilege policies will ensure that users and groups only have the access they need. I also recommend using automated scripts to flag risky permissions and providing training so users understand how to set and maintain secure file and directory permissions. |

