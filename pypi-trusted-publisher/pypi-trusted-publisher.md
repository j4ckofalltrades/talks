---
theme: night
---
# Trusted Publishers in PyPI :snake: :package:

---

## About Me

Jordan Duabe -- `@j4ckofalltrades`

- Software Engineer @ Anaqua
- Command-line ninja :ninja:
- Has one too many MKBs :keyboard:

---

## Agenda

- Intro to PyPI
- Packaging (and Publishing) Flow
- Trusted Publishing

---

## The Python Package Index (PyPI)

---

### What is PyPI

- The official third-party software repository for Python hosted at [pypi.org](https://pypi.org)
- aka the Cheese Shop
- Launched in 2003

---

> [!info] PyPI Stats
> - 525,341 projects
> - 5,513,294 releases
> - 10,633,210 files
> - 797,720 users

---

### TestPyPI

A separate (test) instance of the [Python Package Index (PyPI)](https://packaging.python.org/en/latest/glossary/#term-Python-Package-Index-PyPI) accessible at [test.pypi.org](https://test.pypi.org)

---

## Packaging (and Publishing) Flow

---

> [!info] A sample Python project
> - [github.com/pypa/sampleproject](https://github.com/pypa/sampleproject)
> - [packaging.python.org/tutorials/packaging-projects/](https://packaging.python.org/tutorials/packaging-projects/ "https://packaging.python.org/tutorials/packaging-projects/")
> - [pypi.org/p/sampleproject](https://pypi.org/p/sampleproject)

---
### Package structure

```bash
sampleproject/
├── LICENSE
├── pyproject.toml
├── README.md
├── src/
│   └── sampleproject/
│       ├── __init__.py
│       └── example.py
└── tests/
```

---

### Configuration file

![pyproject.toml|620x400](https://res.cloudinary.com/j4ckofalltrades/image/upload/v1711294223/talks/pypi-trusted-publisher/config_file_lfbvoy.png)

---

### Generating distribution archives

```bash
python3 -m pip install --upgrade build
python3 -m build

# output
ls dist
dist/
├── sampleproject-3.0.0-py3-none-any.whl
└── sampleproject-3.0.0.tar.gz
```

---

### Built Distribution (`wheel`)

![sampleproject wheel|400](https://res.cloudinary.com/j4ckofalltrades/image/upload/v1711295571/talks/pypi-trusted-publisher/sampleproject-wheel_dat1hf.png)

---
### Source Distribution (`sdist`)

![sampleproject sdist|410](https://res.cloudinary.com/j4ckofalltrades/image/upload/v1711295571/talks/pypi-trusted-publisher/sampleproject-sdist_u6tqob.png)

---

### Uploading the distribution archives

This requires a (1) PyPI account and an (2) API token.

```bash
python3 -m pip install --upgrade twine
python3 -m twine upload dist/*

# prompt for PyPI credentials
Enter your username: __token__
Uploading sampleproject-3.0.0-py3-none-any.whl
100% ━━━━━━━━━━━━━━━━━━━━━━━━━ 8.2/8.2 kB • 00:01 • ?
Uploading sampleproject-3.0.0.tar.gz
100% ━━━━━━━━━━━━━━━━━━━━━━━━━ 6.8/6.8 kB • 00:00 • ?
```

---

### Automation with GitHub Actions

![PyPI publish with api token workflow|450](https://res.cloudinary.com/j4ckofalltrades/image/upload/v1711296062/talks/pypi-trusted-publisher/pypi_publish_api_token_orw9z1.png)

---

## Trusted Publishing

---

> [!info]
> GitHub is currently the only OIDC identity provider that is supported.

---

### Publishing with OpenID Connect (OIDC)

![GitHub OIDC integration](https://docs.github.com/assets/cb-63262/mw-1440/images/help/actions/oidc-architecture.webp)

Diagram from <cite>[OpenID Connect Overview](https://docs.github.com/en/actions/deployment/security-hardening-your-deployments/about-security-hardening-with-openid-connect#overview-of-openid-connect)</cite> on GitHub

Note:

1. Configure a project on PyPI to trust a GitHub Actions workflow
2. GitHub Actions workflow submits an OIDC token to PyPI
3. PyPI verifies the OIDC token against configured *trusted publisher(s)*
4. PyPI mints and responds with a *short-lived API token* (valid for 15 minutes)
5. GitHub Actions workflow publishes to PyPI using the *short-lived API token*

---

### Configuring a Trusted Publisher in PyPI

![PyPI Trusted Publisher config|300x470](https://docs.pypi.org/assets/project-publishing-form.png)
 
---

### JWT (Encoded)

![Sample JWT|380](https://res.cloudinary.com/j4ckofalltrades/image/upload/v1711379109/talks/pypi-trusted-publisher/jwt-encoded_clrohx.png)

---
### JWT (Decoded)

![OIDC token|550x480](https://res.cloudinary.com/j4ckofalltrades/image/upload/v1711294223/talks/pypi-trusted-publisher/oidc_token_kpwy6b.png)

---

### Verifying the JWT

The [GitHub OIDC provider configuration](https://token.actions.githubusercontent.com/.well-known/openid-configuration) contains the following information:

- A `claims_supported` JSON array that lists all supported JWT claims
- A `jwks_uri` that contains the [JSON Web Key Set](https://token.actions.githubusercontent.com/.well-known/jwks) used to verify the JWT

---

### Using Trusted Publishing with GitHub Actions

![Trusted Publisher action|500](https://res.cloudinary.com/j4ckofalltrades/image/upload/v1711294224/talks/pypi-trusted-publisher/trusted_publisher_diff_grwfxj.png)

---

### Putting it all together

> [!info] Trusted Publisher workflow in action
> 
> [github.com/j4ckofalltrades/powerline-k8s](https://github.com/j4ckofalltrades/powerline-k8s)

---

### Why use Trusted Publishers?

1) **Usability.** With a trusted publisher, no manual API token management is necessary: configuring the publisher is a one-time action for each project.
2) **Security**. Short-lived (effectively ephemeral) tokens reduces potential damage (if the token gets "leaked") as it expires automatically.

---

## Resources

- [PyPI Docs](https://docs.pypi.org/)
- [PyPA Packaging User Guide](https://packaging.python.org/en/latest/)
- [GitHub Overview of OpenID Connect](https://docs.github.com/en/actions/deployment/security-hardening-your-deployments/about-security-hardening-with-openid-connect#overview-of-openid-connect)
- [JWT.io](https://jwt.io/)

---

## Slides

![slides|200](data:image/png;base64,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)
