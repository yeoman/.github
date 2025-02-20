# Security Policy

This policy applies to **all repositories under the [github.com/yeoman](https://github.com/yeoman) organization**.

## Reporting a Vulnerability

We take security vulnerabilities seriously and appreciate your efforts to responsibly disclose any issues. Please follow the guidelines below to report any security issues **privately**.

### Primary Reporting Method: GitHub Security Advisory

If you discover a security vulnerability, it is crucial that you **do not create a public issue** under any circumstances. Public issues can inadvertently expose the vulnerability, potentially leading to exploitation before a fix is available.

Instead, please report the vulnerability via the [GitHub Security Advisory](https://docs.github.com/en/code-security/security-advisories/guidance-on-reporting-and-writing/privately-reporting-a-security-vulnerability) for the relevant repository. This private channel ensures only maintainers can access the details, enabling a timely and secure resolution. We fully utilize the capabilities that the GitHub Security Advisory platform provides, including private communication channels and coordinated disclosure options.

### Secondary Reporting Option: Email

If you are unable to use the GitHub Security Advisory for any reason, you may report the issue via email to `ulises@linux.com`. If you would like to encrypt sensitive information, please use our PGP key, available at: [https://github.com/ulisesgascon.gpg](https://github.com/ulisesgascon.gpg)

When sending an email, please include:
- Steps to reproduce the issue.
- A description of the vulnerability and its potential impact.
- Any supporting information or proof of concept.

If a security vulnerability is deemed critical (e.g., actively exploited RCE), Yeoman maintainers may deploy an emergency fix outside the regular release cycle. If you identify an urgent issue, please flag it as "Critical - Immediate Attention Required" in your report.


### Coordinated Vulnerability Disclosure (CVD)

We also support [Coordinated Vulnerability Disclosure (CVD)](https://en.wikipedia.org/wiki/Coordinated_vulnerability_disclosure). By following this process, we can collaborate on fixes and ensure vulnerabilities are not publicly disclosed until they are properly addressed.

### Important Reminder

🚨**Do not create a public issue to report a security vulnerability.** This is to protect both the project and its users from potential exploitation before the issue is resolved.

### Disclosure Timeline and Public Announcement

- **Acknowledgment**: We will acknowledge receipt of your report within **2–5 working days**.
- **Progress Updates**: We will keep you informed as we investigate and work on a fix.
- **Resolution Time**: We aim to resolve confirmed vulnerabilities within **30 days**, but complex issues may take longer.
- **Public Disclosure**: We coordinate with you on public disclosure only after a fix or mitigation is in place.
- **Researcher Recognition**: With your permission, we acknowledge security researchers in the public advisory after the fix.


### Acknowledgements and Bug Bounty Program 💰

At this time, **Yeoman does not offer a bug bounty program** or monetary rewards for security vulnerability disclosures. However, we **greatly appreciate and recognize** the contributions of security researchers who responsibly disclose vulnerabilities.

If you report a valid security issue, we will acknowledge your contribution (with your permission) in public advisories once the fix is published. Alternatively, you may remain anonymous if you prefer.

Thank you for helping us maintain a secure environment across all [Yeoman](https://github.com/yeoman) repositories!

## Threat Model

The Yeoman threat model defines **trusted** and **untrusted** elements in the Yeoman ecosystem. All security vulnerabilities reported for any repository that the organization maintains are assessed within this context.

### Elements Yeoman Does NOT Trust
Yeoman **does not trust**:
- **User Input**: CLI arguments, interactive prompts, and configuration files.
- **Third-Party Generators**: Code from npm-installed generators unless officially maintained by Yeoman.
- **Remote Templates & Assets**: Files fetched from external sources during scaffolding.
- **Unvalidated Dependencies**: Packages installed dynamically during generator execution.

Since Yeoman does not inherently trust these elements, generator authors should ensure proper validation, sanitization, and security checks in their own implementations. This includes handling user input safely, verifying dependencies, and ensuring templates are fetched from trusted sources.

### Elements Yeoman Trusts
Yeoman **trusts**:
- **Node.js Runtime**: The JavaScript runtime and its security model.
- **Operating System**: The OS environment where Yeoman runs.
- **Official Yeoman Libraries**: Core packages such as `yo`, `generator-generator`, etc.
- **npm Registry**: We rely on the npm registry for package distribution but do not inherently trust all packages. Each generator should be carefully evaluated before use. Users are encouraged to audit dependencies and be aware of potential supply-chain risks.
- **Filesystem Integrity**: When reading/writing project templates, Yeoman assumes normal OS security constraints.

## Examples of Security Vulnerabilities

**Yeoman assesses vulnerabilities based on its threat model**. 

If an issue arises due to insecure usage of untrusted components (e.g., an insecure third-party generator), it is generally the responsibility of the generator's maintainer. However, if Yeoman itself fails to properly mitigate risks when handling untrusted inputs, it may be considered a valid security issue.

If a generator creates an insecure project by default, it is the responsibility of the generator author. However, if Yeoman itself facilitates or encourages insecure practices in its official generators, it will be treated as a security concern.

Additionally, Yeoman expects users to review and customize the generated project according to their security requirements. While Yeoman provides a starting point, it is ultimately the responsibility of developers to assess and adjust configurations, dependencies, and security settings before deployment.

### Vulnerabilities
✅ **Remote Code Execution (CWE-94)**  
- If Yeoman executes arbitrary untrusted code from generators without proper sandboxing.

✅ **Path Traversal (CWE-35)**  
- If a generator allows users to write files outside the intended project directory via manipulated file paths.

✅ **Command Injection (CWE-78)**  
- If user input can execute arbitrary shell commands inside a generator.

✅ **Insecure Template Handling (CWE-116)**  
- If a generator fails to escape user input, leading to cross-site scripting (XSS) or HTML injection.

✅ **Prototype Pollution (CWE-1321)**  
- If a generator allows modification of global JavaScript objects via user-controlled input.

✅ **Insecure Defaults in Core Libraries**  
- If Yeoman itself ships with insecure defaults that expose users to vulnerabilities.

## Examples of Non-Vulnerabilities

### Not Considered Vulnerabilities
🚫 **Malicious Third-Party Generators (CWE-1357)**  
- Yeoman does not vet all npm generators. If a third-party generator has malicious code, it is the responsibility of the generator’s maintainer.

🚫 **Insecure Default Templates**  
- If a generator creates an insecure project (e.g., open CORS settings), this is not a Yeoman vulnerability, but an issue with the generator.
- Users are responsible for reviewing and securing generated projects. Yeoman provides scaffolding as a foundation, but security configurations should be assessed and customized before deployment.

🚫 **User-Defined Environment Variables**  
- If a user sets insecure values in environment variables (e.g., modifying `PATH` to include malicious binaries), it is not a security issue in Yeoman.

🚫 **Dependencies with Known Vulnerabilities**  
- If a generator uses a vulnerable npm package, the issue lies with the package maintainer, not Yeoman.

🚫 **Misconfigurations in User Code**  
- If a developer misconfigures a generator leading to an insecure project setup, this is not considered a security vulnerability.

