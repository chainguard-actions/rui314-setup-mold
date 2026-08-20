This action downloads and installs [the mold
linker](https://github.com/rui314/mold) as the default linker.

# Usage

Just add the following line to the `steps:` list in your GitHub
Action YAML file:

```yaml
- uses: rui314/setup-mold@v1
```

You can optionally pass parameters to the action as follows:

```yaml
- uses: rui314/setup-mold@v1
  with:
    mold-version: 1.1.1
    make-default: false
```

`mold-version` specifies the mold version to install. By default,
the most recent version will be installed.

If `make-default: false` is specified, mold will still be installed as
`/usr/local/bin/mold` but it won't replace the default linker.
If you choose not to install mold as the default linker, you need to
pass an appropriate option such as `-fuse-ld=mold` to the compiler
to use mold.

## Privacy

This Action contacts Chainguard's licensing server to verify authorization. Connection metadata (IP address, GitHub repository identifier, timestamp, and any metadata encoded in the auth token) is transmitted to Chainguard, Inc. even if authorization is denied in accordance with our [Privacy Notice](https://www.chainguard.dev/legal/privacy-notice)
