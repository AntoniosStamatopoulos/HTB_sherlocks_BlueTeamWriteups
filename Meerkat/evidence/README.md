# Evidence

This directory contains extracted HTTP request logs used during the investigation.

These artifacts were analyzed to trace the attacker’s activity across multiple stages of the incident, including authentication attempts, exploitation of the Bonita application, remote command execution, and persistence-related actions.

Specifically:

* `login_attempts.txt` was used to review the authentication phase and the early exploitation activity that followed successful access, including malicious API interaction and command execution.
* `exploit_requests.txt` was used to examine later exploitation and post-compromise behavior, including payload delivery, script execution, and persistence-related activity.

The evidence contained in these files helped support the findings for:

* **Task 3** – CVE-2022-25237
* **Task 4** – `i18ntranslation`
* **Task 8** – `hffgra4unv`
* **Task 9** – `/home/ubuntu/.ssh/authorized_keys`
* **Task 10** – `T1098.004`

Together, these files helped reconstruct the attack flow and validate key parts of the investigation.
