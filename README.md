# Cloud security projects

Hands-on AWS security labs. Each one is documented in its own folder: the scenario, the steps, the screenshots, and what actually broke along the way.

## Projects

| Project | Covers | Link |
|---|---|---|
| Scoping intern access with IAM | Resource tags, a scoped JSON policy, IAM groups and users, an account alias | [aws-iam-intern-access](./aws-iam-intern-access) |
| Encrypting a DynamoDB table with KMS | Customer managed keys, key policies vs IAM policies, transparent encryption | [aws-kms-dynamodb-encryption](./aws-kms-dynamodb-encryption) |
| Blocking web attacks with WAF | Managed rule groups, a custom rate limit, XSS and SQL injection tests | [aws-waf-web-firewall](./aws-waf-web-firewall) |
| Testing AWS Security Agent's code review | GitHub integration, AI-assisted vulnerability findings, a scoped test repo | [aws-security-agent-code-review](./aws-security-agent-code-review) |
| Replacing hardcoded credentials with Secrets Manager | Runtime secret retrieval, GitHub push protection, an interactive rebase to drop a leaked credential from git history | [aws-secrets-manager](./aws-secrets-manager) |
| Alerting on secret access with CloudWatch and SNS | CloudTrail management events, a CloudWatch metric filter and alarm, an SNS topic with a confirmed subscription | [aws-cloudwatch-secret-access-alerts](./aws-cloudwatch-secret-access-alerts) |
| VPC traffic flow and security | Route tables and internet gateways, security groups vs. network ACLs, AWS Resource Explorer | [aws-vpc-traffic-flow-security](./aws-vpc-traffic-flow-security) |
| VPC monitoring with Flow Logs | A two-VPC architecture, VPC peering, Flow Logs, CloudWatch Logs Insights | [aws-vpc-flow-logs-monitoring](./aws-vpc-flow-logs-monitoring) |
| Cracking MD5 hashes with Hashcat | Dictionary attacks, wordlists, a real memory limit and the workaround for it | [hashcat-md5-cracking](./hashcat-md5-cracking) |
| Attacking and detecting with GuardDuty | SQL and command injection, stolen credentials, and catching it from the defender's side | [aws-guardduty-attack-detection](./aws-guardduty-attack-detection) |

## How this is organized

Each project gets its own folder with two things: a `README.md` that walks through what was built and why, and a `screenshots` folder with the console output that backs it up.

To add a new one: create a folder, drop in a README following the same shape (scenario, steps, a closing takeaway), add a row to the table above. That's it, no need to touch anything else.

## Why these ten together

IAM, KMS, and WAF cover three layers of the same problem: who can do what (IAM), whether the data itself is protected if access control fails (KMS), and what never reaches the app in the first place (WAF). The two VPC projects sit underneath all three: routes, security groups, and network ACLs decide whether traffic can reach a resource at all, before identity or encryption or a WAF rule ever gets a say, and Flow Logs go a step further, recording what actually happened on the wire rather than just what was configured to happen. AWS Security Agent is a different layer again, catching vulnerable code before it ships at all, rather than defending it once it's running. Secrets Manager sits next to that: the credentials never should have been in the code to begin with, and once they were, fixing config.py wasn't enough on its own, the leaked commit had to come out of git history too. The CloudWatch and SNS project builds directly on top of Secrets Manager: storing a secret safely is only half the problem, knowing the moment someone actually opens it is the other half, and that's the difference between an audit trail and real detection. Hashcat flips the perspective entirely, it's the attacker's side of the same story: what happens once a hash actually leaks. GuardDuty closes the loop, playing both attacker and defender in the same project to see whether detection tooling actually notices what the others were built to prevent. Posted individually these are small labs. Together they're a start on security across network, identity, encryption, application, code, secrets management, monitoring and alerting, the attacker's toolkit, and detection.
