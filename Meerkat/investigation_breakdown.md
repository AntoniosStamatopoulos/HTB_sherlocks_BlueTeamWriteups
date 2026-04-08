## Investigation Breakdown

### Task 1 – Application Identification

The PCAP file was analyzed using Wireshark, focusing on HTTP traffic.

**Filter used:**

```
http
```

Inspection of request URIs revealed repeated communication with `/bonita/` endpoints, identifying the application in use as Bonita BPM.

---

### Task 2 – Attack Type Identification

Multiple HTTP POST requests targeting the login endpoint were observed.

**Filter used:**

```
http.request.method == "POST" && http.request.uri contains "/loginservice"
```

Each request contained different username and password combinations and originated from an automated client (`python-requests`), indicating a credential stuffing attack.

---

### Task 3 – Exploited Vulnerability

After successful authentication, the attacker accessed internal API endpoints.

**Filter used:**

```
http contains "API"
```

The observed behavior (API abuse, file upload, and command execution) aligns with CVE-2022-25237, which allows authorization bypass and remote code execution in Bonita BPM.

---

### Task 4 – Authorization Bypass Technique

The attacker appended a specific string to API endpoints to bypass authorization controls.

**Filter used:**

```
http contains "i18ntranslation"
```

This revealed manipulated requests such as:

```
/bonita/API/pageUpload;i18ntranslation
```

---

### Task 5 – Credential Usage Analysis

Login attempts were filtered and analyzed to determine the number of credential combinations used.

**Filter used:**

```
http.request.method == "POST" && http.request.uri == "/bonita/loginservice"
```

After excluding irrelevant and duplicate entries, 56 unique username and password combinations were identified.

---

### Task 6 – Successful Authentication

A successful login was identified based on HTTP response codes.

**Filter used:**

```
http.response.code == 204
```

The corresponding request contained valid credentials, which were then used by the attacker to access the system.

---

### Task 7 – External Resource Usage

Post-exploitation activity revealed the use of an external text-sharing platform.

**Filter used:**

```
http contains "pastes"
```

The attacker executed commands to download a script from `pastes.io`, indicating external payload delivery.

---

### Task 8 – Persistence Mechanism

The attacker executed commands remotely to establish persistence.

**Filter used:**

```
http contains "cmd="
```

Analysis showed command execution involving file download and script execution, leading to SSH-based persistence.

---

### Task 9 – File Modification

The persistence mechanism involved modifying SSH configuration files.

Although not directly visible via a single filter, command execution traces revealed modification of:

```
/home/ubuntu/.ssh/authorized_keys
```

---

### Task 10 – MITRE ATT&CK Mapping

The persistence technique was mapped to MITRE ATT&CK.

Based on observed behavior (injection of SSH public keys), this corresponds to:

```
T1098.004 – Account Manipulation: SSH Authorized Keys
```
