# About

This repository will contain benchmark comparison between Snyk and npm audit.
These remarks and records of issues observed with npm audit in comparison to Snyk
are entirely based on my own personal experience.

## About Snyk and npm audit

|       Tool      | Background     
| :-------------: | -------------- 
| About Snyk      | [Snyk](https://snyk.io) is a developer-first security company, providing free and commercial developer tooling and platform to find and fix security vulnerabilities in code, dependencies, container images, and infrastructure as code
| About npm audit | [npm](https://docs.npmjs.com/about-npm) is the open source package manager and [public registry](https://www.npmjs.com) for JavaScript packages. The npm package manager includes a built-in security tool in the form of the `npm audit` command which submits a description of the dependencies configured in your project to the npm registry and asks for a report of known vulnerabilities. If any vulnerabilities are found, then the impact and appropriate remediation will be calculated.

Sources and references:
- [Snyk website](https://snyk.io)
- [npm-audit command](https://docs.npmjs.com/cli/v9/commands/npm-audit)

## Snyk vs npm audit Capabilities comparison

| Capability    |       Snyk        |    npm audit    |
| ------------- | :---------------: | :-------------: |
| Item | Item | Item
| Item | Item | Item


## Observed issues with npm audit

The following are a list of cases and experiences which have been observed with using npm audit
and are deemed problematic for a security tool:

- [1 npm audit reports false positives](#1-npm-audit-reports-false-positives)
- [2 npm audit reports false negatives](#2-npm-audit-reports-false-negatives)
- [3 npm audit doesnt report vulnerabilities for special versions](#3-npm-audit-doesnt-report-vulnerabilities-for-special-versions)

---

### 1 npm audit reports false positives

❌ **Case**: npm audit reports false positives in such a way that packages that were once vulnerable but later
in the future received a fix, are still reported as vulnerable across versions. 
npm fails to stay up to date with patches applied to libraries and ends up completely missing out on them.

👉 **Example**: [fs-path](https://github.com/pillys/fs-path/pull/5)

❌ **Case**: Dependabot & npm audit both reported a vulnerable `glob-parent@5.1.2` which isn't true, but due
to the large noise created with Dependabot also alerting on this, maintainers were frustrated to receive
alerts to their upstream projects, like `chokidar`.

Dependabot mis-classifying `glob-parent@5.1.2` as vulnerable:

<img src="https://user-images.githubusercontent.com/316371/211521032-02e582f0-3257-4344-95c5-505189615516.png" height="500" />

Snyk properly finding that version as not vulnerable:

<img src="https://user-images.githubusercontent.com/316371/211521208-96ec6acb-581d-4ff4-972c-8fcbeae1777e.png" height="500" />


✅ **The Snyk case**: Snyk’s security analysts are always monitoring vulnerable packages for new releases, 
and manually triage them for fixes or other updates that are significantly impacting the state of the package or vulnerability.



---

### 2 npm audit reports false negatives

❌ **Case**: npm audit reports false positives for packages, meaning that while a library has been detected to be vulnerable by Snyk,
npm audit hasn't caught up with this vulnerability and won't report it as vulnerable.

👉 **Example**: [react-json-pretty](https://snyk.io/advisor/npm-package/react-json-pretty)

✅ **The Snyk case**: [react-json-pretty has been vulnerable since 2019](https://security.snyk.io/package/npm/react-json-pretty) which
Snyk detected at that time, yet 4 years later both `npm audit` and `osv.dev` still don't report as vulnerable.

---

### 3 npm audit doesnt report vulnerabilities for special versions

❌ **Case**: npm audit won’t report vulnerabilities for versions which aren't semver, so a vulnerable or malicious version such as
`myPackage@1.2.3-pre` will not show up anything during an `npm audit` analysis. This is due to the fact that semver is strict `X.Y.Z`
numeric format.

👉 **Example**: See [evidence source](https://jfrog.com/blog/invisible-npm-malware-evading-security-checks-with-crafted-versions)

✅ **The Snyk case**: Snyk will report vulnerabilities, regardless of the version format used.


## Hidden benefits of using Snyk

| Snyk Capability       |    The Benefit    |    Description
| -------------         | :---------------: | :---------------: 
| ✅ Multi-language     | Teams of different languages and platforms can use the same security tool | Support for more than just JavaScript includes Java, Python, Go, Ruby, PHP, .NET and others.
| ✅ Full SDLC          | `npm audit` is a CLI command where-as Snyk is consumable with `snyk` CLI, native Git integration for webhooks and CI checks, and IDE plugins | Integrations exist across mutliple Git SCMs, IDEs, cloud vendors such as GCP, GKE, AWS and Azure's services included, Docker Hub, etc.


# Author

Liran Tal <liran.tal@gmail.com>
