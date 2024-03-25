# Trusted Publishers in PyPI

Trusted publishing is a "new" mechanism for uploading packages to the
Python Package Index (PyPI).

It leverages the OpenID Connect (OIDC) standard to facilitate the exchange
of short-lived identity tokens between a trusted third-party service and PyPI.
What's cool about this method is its applicability in automated environments
(like GitHub Actions), offering a streamlined process that removes the need
for manually generated API tokens for authentication for your publishing
workflows.

The slides are available in the following formats:

- [HTML](https://jduabe.dev/pypi-trusted-publisher)
- [Markdown](pypi-trusted-publisher.md)
- [PDF](pypi-trusted-publisher.pdf)

