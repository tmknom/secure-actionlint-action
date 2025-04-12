# secure-actionlint-action

Run [actionlint][actionlint] in an isolated Docker container to securely lint workflow files.

<!-- actdocs start -->

## Description

This action securely runs actionlint in a Docker container to lint GitHub Actions workflow files.
It automatically checks all YAML files in the `.github/workflows` directory.

This action reduces security risks associated with software supply chain attacks, such as compromised third-party tools or tampered container images.
To achieve this, it enforces strict container isolation, disables network connections, and drops unnecessary privileges.

## Usage

### Default

```yaml
  steps:
    - name: Secure actionlint
      uses: tmknom/secure-actionlint-action@319c663fe7a559d2bf907b36a8edfd80b89508b6 # v0.3.0
```

### Custom

```yaml
  steps:
    - name: Secure actionlint
      uses: tmknom/secure-actionlint-action@319c663fe7a559d2bf907b36a8edfd80b89508b6 # v0.3.0
      with:
        configuration-path: actionlint.yml
        ignore: |-
          "jobs" section is missing in workflow
          "on" section is missing .+
```

## Inputs

| Name | Description | Default | Required |
| :--- | :---------- | :------ | :------: |
| configuration-path | The path for the actionlint configurations. | n/a | no |
| ignore | Specify regular expressions to ignore actionlint error messages, one per line. | n/a | no |

## Outputs

N/A

<!-- actdocs end -->

## Permissions

N/A

## FAQ

### What is actionlint?

[actionlint][actionlint] is a linter for GitHub Actions workflow files.
It detects errors, security issues, and best practice violations, helping you maintain safer and more reliable workflows.

### Why should I use this action instead of directly using actionlint?

Running third-party tools directly in your environment may expose your repository and credentials to compromised or malicious code.
This action significantly reduces such risks by strictly isolating the environment:

- Network access is completely disabled (`--network none`)
- All unnecessary Linux capabilities are dropped (`--cap-drop all`)
- Privilege escalation is explicitly disabled (`--security-opt no-new-privileges`)
- The action runs as a non-root, restricted user  (`--user guest`)
- The filesystem is strictly read-only (`--read-only`)
- The repository directory is mounted as read-only (`--volume "${PWD}:${PWD}:ro"`)

### What specific security risks does this action protect against?

This action specifically prevents threats related to software supply chain security (attacks targeting third-party software or tools used in workflows), such as:

- Unauthorized outbound connections from runners, significantly reducing the risk of data leakage
- Malicious updates or compromised tools exploiting elevated privileges or unrestricted network access

### How does this action ensure the Docker image used for actionlint is secure?

This action explicitly specifies the Docker image using its digest (SHA256).
Using a digest ensures that exactly the intended and verified image is used every time, eliminating the risk of malicious updates or image tampering.

### Are network connections permitted inside the Docker container used by this action?

No. Network connections are completely disabled within the container.
Even if the tool were compromised or contained malicious code, disabling network access effectively prevents communication with external attackers, significantly reducing the risk of data leaks.

### Does this action run with elevated (root) privileges?

No. The action runs as a non-root, restricted user without privilege escalation.

### Should I pin this action using a commit SHA?

**Yes, strongly recommended.**

To further protect your workflows from unintended or malicious modifications, it's a best practice to pin the action to a specific commit SHA (commit hash).
Doing so ensures the immutability of both the action’s code and any resources it references, such as Docker images, further reducing the risk of software supply chain attacks.

**Recommended (more secure):**

```yaml
- uses: tmknom/secure-actionlint-action@319c663fe7a559d2bf907b36a8edfd80b89508b6 # v0.3.0
```

**Not recommended:**

```yaml
- uses: tmknom/secure-actionlint-action@v0
```

### Can I customize the actionlint parameters?

Yes. You can specify a custom configuration file for actionlint with the `configuration-path` input.

**Example usage:**

```yaml
- uses: tmknom/secure-actionlint-action@v0
  with:
    configuration-path: path/to/your/actionlint.yaml
```

If this input is omitted, actionlint will run with its default settings.
For details on how to create a configuration file, see [actionlint's configuration documentation](https://github.com/rhysd/actionlint/blob/main/docs/config.md).

### Does using this action significantly impact my CI/CD performance?

No. The impact is minimal, primarily due to the overhead of pulling and executing a small Docker container.
Typically, this added overhead is negligible, and the security improvements provided by the action justify its use.

### Can I run this action without Docker?

No. This action strictly requires Docker to be installed on your GitHub Actions runner.
Without Docker, the action will fail to execute, as it relies on Docker’s isolation mechanisms to run securely.

## Related projects

N/A

## Release notes

See [GitHub Releases][releases].

[actionlint]: https://github.com/rhysd/actionlint
[releases]: https://github.com/tmknom/secure-actionlint-action/releases
