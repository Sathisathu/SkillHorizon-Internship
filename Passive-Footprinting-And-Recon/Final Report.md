# Passive Reconnaissance Report for projectdiscovery.io

## 1. Target Domain

**projectdiscovery.io**

---

## 2. WHOIS & DNS Findings

### WHOIS Information

A `whois` lookup was performed to gather domain registration details.

- **Registrar:** GoDaddy.com, LLC
- **Creation Date:** 2018-11-14
- **Expiration Date:** 2025-11-14
- **Name Servers:** `otto.ns.cloudflare.com`, `alexandra.ns.cloudflare.com`


<img src="img/whois.png" alt="WHOIS Lookup Results" width="75%">

### DNS Records

`dig` and `nslookup` were used to enumerate DNS records.

- **A Record:** The root domain `projectdiscovery.io` resolves to the IP address `76.76.21.21`.
- **NS Records:** The domain uses Cloudflare for its DNS services.
- **MX Records:** Mail records point to Google's servers (`aspmx.l.google.com`), indicating the use of Google Workspace for email.
- **TXT Records:**
  - An SPF record was found: `v=spf1 include:_spf.google.com include:_spf.salesforce.com ~all`, authorizing email sending from Google and Salesforce.
  - Several verification codes for services like Google Site Verification, HackerOne, and Slack were also present, confirming their use.


<img src="img/dig.png" alt="DIG Results" width="75%">
<img src="img/nslookup.png" alt="NSLOOKUP Results" width="75%">

---

## 3. Subdomain Enumeration

Passive subdomain enumeration was conducted using `subfinder`, `assetfinder` and `amass`.

**Key Subdomains Discovered:**

- `cloud.projectdiscovery.io`
- `chaos.projectdiscovery.io`
- `blog.projectdiscovery.io`
- `docs.projectdiscovery.io`
- `api.projectdiscovery.io`
- `auth.projectdiscovery.io`
- `status.projectdiscovery.io`
- `security.projectdiscovery.io`
  [View more](files/activeSubDomains.txt)

<img src="img/subDomainLookups.png" alt="SUB-DOMAIN LOOKUP Results" width="75%">

---

## 4. Email & Employee Information

The `theHarvester` tool was used to gather publicly available email addresses.

**Discovered Email Addresses:**

- `jane.doe@projectdiscovery.io`
- `jane@projectdiscovery.io`
- `marketing@projectdiscovery.io`
- `rishiraj@projectdiscovery.io`
- `security@projectdiscovery.io`
- `templates@projectdiscovery.io`

<img src="img/harvesterReport.png" alt="Harvester Report" width="75%">

## 5. Metadata Extraction

An attempt was made to find and analyze metadata from publicly available documents using `metagoofil`. The tool did not find any downloadable files of types PDF, DOCX, or XLSX. Therefore, no metadata could be extracted from documents.

<img src="img/metaDataExtraction.png" alt="Meta Data Report" width="75%">

---

## 6. Google Dorking

Several Google Dorks were executed to find indexed files and potentially sensitive information.

- **`site:projectdiscovery.io filetype:pdf`**: No results.
- **`site:projectdiscovery.io filetype:docx`**: No results.
- **`site:projectdiscovery.io filetype:xlsx`**: No results.
- **`site:projectdiscovery.io inurl:admin`**: Returned several URLs related to administrative documentation for their cloud platform, including settings and audit logs.
- **`site:projectdiscovery.io intitle:admin`**: Discovered a Netlify CMS admin login panel and links to administration guides.

While no sensitive documents were found, the dorks revealed the locations of administrative interfaces and documentation.

<img src="img/googleDorking.png" alt="Google Dorking Results-1" width="75%">
<img src="img/googleDorking2.png" alt="Google Dorking Results-2" width="75%">

---

## 7. OSINT Summary

Open Source Intelligence gathering confirms that ProjectDiscovery has a significant public presence.

- **GitHub:** The company is a verified organization on GitHub and maintains a large number of popular open-source security tool repositories. This is a core part of their brand and community engagement.
- **LinkedIn:** The company's LinkedIn profile identifies them as a Computer and Network Security firm with 11-50 employees, headquartered in San Francisco. Key employees and investors are publicly listed.
  ![OSINT Results](img/osint.png)
<img src="img/osint.png" alt="OSINT Results" width="75%">

---

## 8. URLs & Leak Data in JS Files

Tools like `katana` and `gau` were used to collect a comprehensive list of URLs from the target domain. These URLs provide insights into the application's structure and endpoints.

The collected JavaScript files were then analyzed for secrets using `JSleak`.

**Key Findings:**

- A potential **Google API Key** was found in `https://feedback.projectdiscovery.io/_next/static/chunks/pages/_app-dfeb87707fe0183d.js`:
  ```
  AIzaSyC4PYsflrurmmeMEgSfVyqfjc41POCQFOE
  ```
  [More findings here](files/leakedJS.txt)

---

## 9. Conclusion: Potential Attack Surfaces

This passive reconnaissance has identified several potential attack surfaces:

1.  **Expanded Attack Surface:** The large number of subdomains increases the risk of forgotten or unmaintained applications and infrastructure that could be vulnerable.
2.  **Information Leakage:** The discovery of API keys and service DSNs in public JavaScript files suggests a potential weakness in secrets management.
3.  **Third-Party Risk:** The reliance on multiple third-party services (Cloudflare, Google, Salesforce, Vercel) introduces potential risk from misconfigurations or vulnerabilities in those platforms.
4.  **Social Engineering:** The availability of employee email addresses and names on public platforms provides a clear vector for targeted phishing or social engineering attacks.
