# Cloud security projects

Hands-on AWS security labs. Each one is documented in its own folder: the scenario, the steps, the screenshots, and what actually broke along the way.

## Projects

| Project | Covers | Link |
|---|---|---|
| Scoping intern access with IAM | Resource tags, a scoped JSON policy, IAM groups and users, an account alias | [aws-iam-intern-access](./aws-iam-intern-access) |
| Encrypting a DynamoDB table with KMS | Customer managed keys, key policies vs IAM policies, transparent encryption | [aws-kms-dynamodb-encryption](./aws-kms-dynamodb-encryption) |
| Blocking web attacks with WAF | Managed rule groups, a custom rate limit, XSS and SQL injection tests | [aws-waf-web-firewall](./aws-waf-web-firewall) |

## How this is organized

Each project gets its own folder with two things: a `README.md` that walks through what was built and why, and a `screenshots` folder with the console output that backs it up.

To add a new one: create a folder, drop in a README following the same shape (scenario, steps, a closing takeaway), add a row to the table above. That's it, no need to touch anything else.

## Why these three together

IAM, KMS, and WAF cover three different layers of the same problem: who can do what (IAM), whether the data itself is protected if access control fails (KMS), and what never reaches the app in the first place (WAF). Posted individually they're three small labs. Together they're a start on identity, encryption, and network security in one account.
