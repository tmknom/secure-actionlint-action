# secure-actionlint-action

Run actionlint in an isolated Docker container to securely lint workflow files.

<!-- actdocs start -->

## Description

This action securely runs actionlint in a Docker container to lint GitHub Actions workflow files.
It reduces security risks from compromised or malicious third-party tools.
To achieve this, it enforces strict container isolation, disables network connections, and drops unnecessary privileges.

## Usage

```yaml
  steps:
    - name: Secure actionlint
      uses: tmknom/secure-actionlint-action@v0
```

## Inputs

N/A

## Outputs

N/A

<!-- actdocs end -->

## Permissions

N/A

## FAQ

N/A

## Related projects

N/A

## Release notes

See [GitHub Releases][releases].

[releases]: https://github.com/tmknom/secure-actionlint-action/releases
