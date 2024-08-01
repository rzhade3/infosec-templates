# Code Review Checklist

Here's a list of things to do to review an application thoroughly:

### Recon

In the first pass of a code review, use as many automated tools as possible to find as many findings quickly. These tools are also a good way to understand the structure of the codebase without needing to review as much of the code.

- [ ] **Secret Scanner:** Trufflehog
- [ ] **Static Analysis:** CodeQL/ Semgrep
- [ ] **Dependency Checker:** Dependency Checker

### Code Review

Most static analysis checkers are insufficient at reviewing contextual security features. These should be manually grep'd for and understood as part of the threat model.

- [ ] Authentication
  * How are users authenticated? SSO or password auth?
  * If the former, check for common flaws in SSO logic, such as non-validation of OAuth state params, etc.
  * If the latter, you'll likely have more to dig into in the next step
- [ ] Encryption
  * What's the threat model for encryption? Have we selected the correct encryption primitives for the right use cases?
    * Think through failure modes (key exhaustion, etc.) 
- [ ] Authorization
  * What are we using for our authorization framework? 
  * Has it been applied consistently across the codebase? Check for exceptions/ places where the default authorization behavior isn't being respected
- [ ] Filesystem Read/ Writes
  * Are files being written or read to or from the filesystem with the path coming from an attacker controlled environment?
- [ ] Network calls
  * What network calls are being initiated from the application? How are they authenticating to the endpoint in question?
  * Are network calls being made to user controlled endpoints?
- [ ] Database calls
  * SQL Injection is king here, common enough and easy to find. What are we persisting, and how are we generating queries for it?