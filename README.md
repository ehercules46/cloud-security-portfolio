# Cloud security projects

Hands-on AWS security labs. Each one is documented in its own folder: the scenario, the steps, the screenshots, and what actually broke along the way.

## Projects

| Project | Covers | Link |
|---|---|---|
| Scoping intern access with IAM | Resource tags, a scoped JSON policy, IAM groups and users, an account alias | [aws-iam-intern-access](./aws-iam-intern-access) |
| Encrypting a DynamoDB table with KMS | Customer managed keys, key policies vs IAM policies, transparent encryption | [aws-kms-dynamodb-encryption](./aws-kms-dynamodb-encryption) |
| Blocking web attacks with WAF | Managed rule groups, a custom rate limit, XSS and SQL injection tests | [aws-waf-web-firewall](./aws-waf-web-firewall) |
| Testing AWS Security Agent's code review | GitHub integration, AI-assisted vulnerability findings, a scoped test repo | [aws-security-agent-code-review](./aws-security-agent-code-review) |
| Cracking MD5 hashes with Hashcat | Dictionary attacks, wordlists, a real memory limit and the workaround for it | [hashcat-md5-cracking](./hashcat-md5-cracking) |

## How this is organized

Each project gets its own folder with two things: a `README.md` that walks through what was built and why, and a `screenshots` folder with the console output that backs it up.

To add a new one: create a folder, drop in a README following the same shape (scenario, steps, a closing takeaway), add a row to the table above. That's it, no need to touch anything else.

## Why these five together

IAM, KMS, and WAF cover three layers of the same problem: who can do what (IAM), whether the data itself is protected if access control fails (KMS), and what never reaches the app in the first place (WAF). AWS Security Agent is a different layer again, catching vulnerable code before it ships at all, rather than defending it once it's running. Hashcat flips the perspective entirely, it's the attacker's side of the same story: what happens once a hash actually leaks, and why the hashing algorithm choice made earlier in KMS-style thinking matters in practice. Posted individually these are small labs. Together they're a start on security across identity, encryption, network, code, and the attacker's own toolkit.
