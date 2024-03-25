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
- Hosts > 450,000 packages (as of May 2023)

---

### TestPyPI

A separate (test) instance of the [Python Package Index (PyPI)](https://packaging.python.org/en/latest/glossary/#term-Python-Package-Index-PyPI) accessible at [test.pypi.org](https://test.pypi.org)

---

## Packaging (and Publishing) Flow

---

### Package types

![Python tools and libs](https://packaging.python.org/en/latest/_images/py_pkg_tools_and_libs.png)

Excerpted from <cite>[The Packaging Gradient (2017)](https://www.youtube.com/watch?v=iLVNWfPWAC8)</cite>

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
├── sampleproject-0.0.1-py3-none-any.whl
└── sampleproject-0.0.1.tar.gz
```

---

### Built Distribution (wheel)

![sampleproject wheel|400](https://res.cloudinary.com/j4ckofalltrades/image/upload/v1711295571/talks/pypi-trusted-publisher/sampleproject-wheel_dat1hf.png)

---
### Source Distribution (sdist)

![sampleproject sdist|410](https://res.cloudinary.com/j4ckofalltrades/image/upload/v1711295571/talks/pypi-trusted-publisher/sampleproject-sdist_u6tqob.png)

---

### Uploading the distribution archives

This requires a (1) PyPI account and an (2) API token.

```bash
python3 -m pip install --upgrade twine
python3 -m twine upload dist/*

# prompt for PyPI credentials
Enter your username: __token__
Uploading sampleproject-0.0.1-py3-none-any.whl
100% ━━━━━━━━━━━━━━━━━━━━━━━━━ 8.2/8.2 kB • 00:01 • ?
Uploading sampleproject-0.0.1.tar.gz
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

Diagram from <cite>[OpenID Overview](https://docs.github.com/en/actions/deployment/security-hardening-your-deployments/about-security-hardening-with-openid-connect#overview-of-openid-connect)</cite> on GitHub

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

![slides|200](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAUgAAAFICAYAAAAyFGczAAAgAElEQVR4Xu3d0ZYbNnMDYPv9H9p1/p6eXjTWtysYJSUhtyNgMOAQS21i5+ev3//82D9zYA7MgTnwfxz4uYDcVsyBOTAH/t2BBeQ2Yw7MgTnwBwcWkFuNOTAH5sACcjswB+bAHPieA3tBfs+vfXoOzIEPcmAB+UGHvVHnwBz4ngMLyO/5tU/PgTnwQQ4sID/osDfqHJgD33NgAfk9v/bpOTAHPsiBBeQHHfZGnQNz4HsOLCC/59c+PQfmwAc5sID8oMPeqHNgDnzPgQXk9/zap+fAHPggBxaQH3TYG3UOzIHvObCA/J5f+/QcmAMf5MAC8oMOe6POgTnwPQcWkN/za5+eA3PggxxYQH7QYW/UOTAHvufAAvJ7fu3Tc2AOfJADC8gPOuyNOgfmwPccWEB+z699eg7MgQ9yYAH5QYe9UefAHPieAwvI7/m1T8+BOfBBDiwgP+iwN+ocmAPfc2AB+T2/9uk5MAc+yIEF5Acd9kadA3Pgew4sIL/n1z49B+bABzmwgPygw96oc2AOfM+BBeT3/Nqn58Ac+CAHFpAfdNgbdQ7Mge85sID8nl/79ByYAx/kwPGA/Pnz51vb/evXr2g++ZPyR+J+g1N9wqf6Unzqr+Zr86fzn8an/qT6F5Cpg8CnB9y+YOn4qT7hU30p/vbzu92/0/6n/ReQqYMLyIcOKGBuv+DSr/XRfG1+6bu9nvqTzreATB1cQC4gHziwgMwu2AJyv4N8uEHtC5at734HKf/a5yd+6bu9voBcQC4gL76l6QVVgLX5L7b2S9JSf77U5NE3gN8Csn/NGirQAoX0x+GpvfIn5U8NSvUJn+pL8am/mq/Nn85/Gp/6k+rf7yBTB/c7yP0Ocr+DrN2iBSS+Yp82SCefvhCEV3/50+aXvnZ/8bf90fxp/zZe+tv19Pzq+m7/iq0FaRsk/vSAhVd/+dPml752f/G3/dH8af82Xvrb9fT86voWkJnF6QELL3XpBUr5hT89X9sfzZ/2b+Olv13Xfmj+ur4FZGZxesDCS50WqM0vfe3+4m/7o/nT/m289Lfr6fnV9S0gM4vTAxZe6tILlPILf3q+tj+aP+3fxkt/u6790Px1fQvIzOL0gIWXOi1Qm1/62v3F3/ZH86f923jpb9fT86vrW0BmFqcHLLzUpRco5Rf+9HxtfzR/2r+Nl/52Xfuh+ev6FpCZxekBCy91WqA2v/S1+4u/7Y/mT/u38dLfrqfnV9f36gEpg1MDTy/o6flS/1L98j/VJ7z0v7o+zSd/VJc/6i+8+qf16/8kjQySwalBaX/hpe/0fNKneqo/9U/6VJf+V9en+eSP6vJH/YVX/7S+gISDOqD2AYs/XQDNl/Kn+tv6NJ/0v7o+zSd/VJc/6i+8+qf1BeQCMt2hh3hdADU/fkEu/6Ow8lf+Ca/zUT3tL7z6p/UF5AIy3aEFZNXBx+QKOAWM8OloaX/hU33CLyAXkNqRqJ5ewOMXZC/I6Px1ftoP4SNxXwAvIBeQX1iT5z+iCyDm4xdkAakjeljX+Wk/hI/EfQG8gFxAfmFNnv+ILoCYj1+QBaSOaAEZOQSwLpAuiPCp9rR/ipf+lD/Fp/qE1/lKv/jTuvSl/JpP/VP87fpTfcLvBXn4BakF1wGmFyDFp/qElz/SL/60Ln0pv+ZT/xR/u/5Un/ALyAXkQwd0AbVguqDCq3/Kr/6qS5/wqms+9U/x0qd62l949U/rC8gF5AIyuEUKqID6P1AFhPqn+Nv1p/qEX0AuIBeQuiUP6gqogHoB+YUfEKm/wi8gF5ALSN2SBeTTDqUvWOGfFvZF4AJyAbmA/OJl+beP7QX52DwFnPwTPji6L0EXkAvIBeSXrsq/f0gXPKDeV+x9xf7xQwumnyDC376gqT7Nn/rXxmt+zSf87XX5K/3yR/zCq7/qaX/h1T+t7wV5+AUZH2D4Jz10QbSgKV7zi1/42+vyV/rlj/iFV3/V0/7Cq39aX0AuIB86oAXVBRNeCyx+4W+vt/0Rf9vftL/w7fNdQC4gF5DtW/aAPw0ABZz4hU+tSfsLn+oTfgG5gFxA6pYU62kAKODEL3w6etpf+FSf8AvIBeQCUrekWE8DQAEnfuHT0dP+wqf6hF9ALiAXkLolxXoaAAo48Qufjp72Fz7VJ/wCcgG5gNQtKdbTAFDAiV/4dPS0v/CpPuFfPiA1YLuuBdMBn8an/ki/+Nv+qH+qX/yaT3jVpb/dX/pUv13/AlIniHp6wKfx4fj8D/3Frwuc+qP+4hdedc0nvOrS3+4vfarfrn8BqRNcQD50QAsue3WBxS+8+otfeNVTfeKX/nZ/6VP9dv0LSJ3gAnIBGexIO6BuDxhZd7v+BaROcAG5gAx2ZAH52LwFZDlggt39K9D0gE/jUxOkX/wKEPELr/7iF171VJ/4pb/dX/pUv13/XpA6wXLApwuS4sPx9y9pYGA7oE6ff3t/2v5J/wJSDi0g9xU72JH2BV9ABofzBej1AfmFGa7+iC5IuuDD/3x4/p/u/9WX4wvidH5foIg+soCM7DNYB7yAW8A92qJ0f7yhd39C87fVLyDLDuuAF5ALyAXknx3Q/Slf3x8LyLLDOuAF5AJyAbmA/KMDCohyftXpF5C/on/JM/8y/+oLXm6g8y+33wuybbAOWD8ghs8C4tP9a+93m1/n1+6/r9hlh3XAC8h9xd5X7H3F3lfsPziwgFxALiAXkOV32uifdaAd0M/q+h/c6Re49Euf8Kn/4l89c+D4V+xM/tCpA+kFFT7VpwBS/xQv/eIXPtUv/tUzBxaQmX8vj04vqPCpQQog9U/x0i9+4VP94l89c2ABmfn38uj0ggqfGqQAUv8UL/3iFz7VL/7VMwcWkJl/L49OL6jwqUEKIPVP8dIvfuFT/eJfPXNgAZn59/Lo9IIKnxqkAFL/FC/94hc+1S/+1TMHFpCZfy+PTi+o8KlBCiD1T/HSL37hU/3iXz1zYAGZ+ffy6PSCCp8apABS/xQv/eIXPtUv/tUzB44HpBYkGy9H6wJMf+bxaX/VX9Pp/MUvvPqn/LfjNX+7voCEw+kCtQ9Q/LfrP61P/eWvAk78wqt/yn87XvO36wvIBWR7xx7ypxc0Fa/+4lfAiV949U/5b8dr/nZ9AbmAbO/YAvKBAwvIx39b09Hl/N18AbmAPLqD6QsmFa/+4lfAiV949U/5b8dr/nZ9AbmAbO/YXpB7Qf7RAQX00eXcC9L26wDTF4AVZJ+4Xf9pfeov93X+4hde/VP+2/Gav13fC3IvyPaO7QW5F+RekM/esvQn6LN9/xYu/Qmc6lD/lL+Nf/XzT/1Jz0/+pfyaT/2FV72tX/33gpRDl78wTy9QaN+P9gVL9bXx6fnJv5Rf86u/8Kq39av/AlIOLSBDhx7D2xesKv4vkKcBIP9Sfo2o/sKr3tav/gtIObSADB1aQD5yIA0ABVTKr8NXf+FVb+tX/wWkHFpAhg4tIBeQz6/QAvLn4/+r3fPW/v8gdYDv/hM2dbntT6qvjdf+qL/8S/nT/sKr3tav/ntByqG9IEOH9oLcC/L5FVpA7gX5/Pb8Rp5eoEj8b7BeQCn/7fj0/ORfyi//1F941dv61f/4C5ICywGaHkB7QeSP9Lf13d5f/skfzSd+1dv9xS99qssf9Rde/dv1BeSv7G8T0QK0D1AL1tZ3e3/5L380n/hVb/cXv/SpLn/UX3j1b9cXkAvIaMe04LogUfO/8CsG6dN8qf52f/Gn+uWP+guf6kvxC8gFZLRDWnBdkKj5ApL2nfZf/bU/HLD8gQXkAjJaMS24LkjUfAFJ+077r/7aHw5Y/sACcgEZrZgWXBckar6ApH2n/Vd/7Q8HLH9gAbmAjFZMC64LEjVfQNK+0/6rv/aHA5Y/sIBcQEYrpgXXBYmaLyBp32n/1V/7wwHLH7g+IDV/egApXvrSuvSl/O0FTfWn+tL+8lf61D/F365P80n/6foCEv8h+ukD1gVLF6g9X6o/1Zf2l7/Sp/4p/nZ9mk/6T9cXkAvI6g4qINQ8vWBp/1Sf+ms+4W/Xp/mk/3R9AbmArO5g+4JLfNpf/AoA9U/xt+vTfNJ/ur6AXEBWd1ABoebpBUv7p/rUX/MJf7s+zSf9p+sLyAVkdQfbF1zi0/7iVwCof4q/XZ/mk/7T9QXkArK6gwoINU8vWNo/1af+mk/42/VpPuk/XV9ALiCrO9i+4BKf9he/AkD9U/zt+jSf9J+uHw9ILZAMOn0Abf3i1/xtvM5H9dP61V/6T/sr/dKn+VRXf+Fvry8gwxNKF1ALJv7T+NA+/o3op+fXfG196p+ev/hVV3/hb68vIMMT0gURvRZM/Kfxmk/10/rVX/rb56P+0i994ldd/YW/vb6ADE8oXUAtmPhP40P79oIM/5ci6fm3zy/lP41fQIYnoAATfbrgp/GaT/XT+tVf+nX+4hde/dv8aX/hb68vIMMTOr3g6QVJ8aF9e0HuBZmuUBW/gAztXUBmBqYB3cZrOp1/qk/92/xpf+Fvry8gwxPSBRF9uuCn8ZpP9dP61V/6df7iF1792/xpf+Fvr798QKYGpwsmvPSlF0T80pf2F7/0qf9p/lRfik/9Ez71V/yaX/i2PvVfQOJvFNcBpwcofh2g6tKX9he/9Kn/af5UX4pP/RM+9Vf8ml/4tj71X0AuILUjD+vpAusCneZP9aV4HY74hU/9Ff/t+qj/t0G/9KFmPTUw1abxpU946RO/8KpLX9pf/NKn/qf5U30pPvVP+NRf8Wt+4dv61H8vyL0gtSN7QT5wQBdYASG8Dkf8wqf9xX+7PurfC/LxA1oHnC6Y+HWAqktf2l/80qf+p/lTfSk+9U/41F/xa37h2/rUfy/IvSC1I3tB7gX59I4sIJ+27r+BqYFh+/qf5JC+9vz6CZz2F386/2l++SN9KT71T3jpF151zS98W5/6X/+ClEHtA0j5dQDpfMKrf1pP/Un1q7/4hZc/4hde9VSf+FVP55P+lF/60/oCMvyKnR6AFuT2BZM++aP5hVd/8Quv/uIXXvVUn/hVT+eT/pRf+tP6AnIBGe2QLoDI0wui/uIXvq1f/Kk+8asu/4SX/pRf/dP6AnIBGe2QLoDI0wui/uIXvq1f/Kk+8asu/4SX/pRf/dP6AnIBGe2QLoDI0wui/uIXvq1f/Kk+8asu/4SX/pRf/dP6AnIBGe2QLoDI0wui/uIXvq1f/Kk+8asu/4SX/pRf/dP6AnIBGe2QLoDI0wui/uIXvq1f/Kk+8asu/4SX/pRf/dP6AnIBGe2QLoDI0wui/uIXvq1f/Kk+8asu/4SX/pRf/dP68YDUADJYeB2A+IVXf/ELn9alX/qElz7xC6/+t/Ofni/tL7z81/mJ/3R9AYn/J0h6wFqg9gJIv/QJL/3iF179b+c/PV/aX3j5r/MT/+n6AnIB+XAH0wXXBdIFUP/b+U/Pl/YXXv7r/MR/ur6AXEAuIB84oABIL7AC5HR/zSd9mk/8p+sLyAXkAnIB+XQOLSCftu7vAHUA6qKfYOIXXv3FL3xal37pE176xC+8+t/Of3q+tL/w8l/nJ/7T9b0g94LcC3IvyKdzaAH5tHV/B6gDUBf9BBO/8OovfuHTuvRLn/DSJ37h1f92/tPzpf2Fl/86P/Gfrh9/QcpgGaQDeHV+zZ/WU3/S/ile5y9+zS9+4dU/rbf1iV/65U/Kr/5pfQEJB3WAWgAdkPiFT+up/rR/ik/90/ziFz6dT/i2PvFLn/xJ+dU/rS8gF5DpDh3FpxcsvcDCt83R/Kk+8Ws+9U/51T+tLyAXkOkOHcWnFyy9wMK3zdH8qT7xaz71T/nVP60vIBeQ6Q4dxacXLL3AwrfN0fypPvFrPvVP+dU/rS8gF5DpDh3FpxcsvcDCt83R/Kk+8Ws+9U/51T+tLyAXkOkOHcWnFyy9wMK3zdH8qT7xaz71T/nVP60vIBeQ6Q4dxacXLL3AwrfN0fypPvFrPvVP+dU/rb98QKYGCN8+QC2Q9Kme6pe+Nn86n/SLX3XNr/7Cq//t/NKn+VJ/xK/6AjJ8Qcpg1dMFEn+6YNLX5k/nk37xq6751V949b+dX/o0X+qP+FVfQC4gHzqgBU8XWPxaYPVP+dv9pV/9Nd9pfunTfKl+8au+gFxALiB1Sx7UdYEVEMJL2u380qf5Un/Er/oCcgG5gNQtWUD+0QEF2AIyWK5/oKmBYXvCtQAkwAfa86f6pa/NL3/VX/rFr3raX3j113yn+aVP86X6xa/6XpB7Qe4FqVuyF+RekMGORND0J0zU/Avg9k+w9vypfulr8+uI1F/6xa962l949dd8p/mlT/Ol+sWv+vEXpASqnh6A+HVA6i+8+qd16RN/qr/dX/zSL7z8UV39hU/1tfuLX/qFlz/t+gIy/Ip9+wJInxYsXeB2f/FLv/DyR3X1Fz7V1+4vfukXXv606wvIBeRDB9IF1gXRgqu/+FO89Kmu/sJrPuHb/cUv/cJrvnZ9AbmAXEAWb1kaAAoYSW/3F7/0C6/52vUF5AJyAVm8ZWkAKGAkvd1f/NIvvOZr1xeQC8gFZPGWpQGggJH0dn/xS7/wmq9dX0AuIBeQxVuWBoACRtLb/cUv/cJrvnZ9AbmAXEAWb1kaAAoYSW/3F7/0C6/52vXjAZkamOJlcJu/3b+tv80vf1RP9bXx0q/66YBJ/dF8p+sLSJzA6QVI+6d4LWibX/1VT/W18dKv+gJSDmX1BeQCMtqgNECi5l8Ap/ra+C+MUP0VSNo/9Sft38YvIBeQ0Y7dfkFSfW18ZP5v8F6QqYOP8QvIBWS0YWmARM2/AE71tfFfGGEvyNSkAL+AXEAG6+O/z/PVXzgLSLywfv68OsCj5f4NXkAuIKMdSgMkav4FcKqvjf/CCFcHUOpPOn8bv4BcQEY7dvsFSfW18ZH5+x1kah/xLx+QmjBd8Nv5pU91fQWWf+Jv11P9wkt/259UX6o/7S9/Un7Nl9YXkL9+RR62F0D8kfgvvEDa/U/rTy9o259Un/yV/rR/m1/zpfUF5ALy4Q5pwdMFTPG6wNIvvPSJX3jVU33il/60f5tf86X1BeQCcgEZ3CIFQED9H2gaUOov/Wn/Nr/mS+sLyAXkAjK4RQqAgHoBmZr3F/ALyAXkAjK4SAvIx+bJn/SFGhzdl6ALyAXkAvJLV+XfP6QACKj3gkzN+wv4BeQCcgEZXKQF5F6QwfoY+uoL5gmzBRJ/+hVF/ov/NF7+SJ/wqrf9UX/NJ31tfulT/1S/+FW//gWpAVQ/bbD0nV4g9Zd/p/Ftf8Xf9kf9U//b/NKn/vJX+LS+gEwdDPGnF0j9taCn8bJf+oRXve2P+ms+6WvzS5/6p/rFr/oCUg6V66cXSP21oKfxOh7pE171tj/qr/mkr80vfeqf6he/6gtIOVSun14g9deCnsbreKRPeNXb/qi/5pO+Nr/0qX+qX/yqLyDlULl+eoHUXwt6Gq/jkT7hVW/7o/6aT/ra/NKn/ql+8au+gJRD5frpBVJ/LehpvI5H+oRXve2P+ms+6WvzS5/6p/rFr/oCUg6V66cXSP21oKfxOh7pE171tj/qr/mkr80vfeqf6he/6tcHpAxKD0AGteuar91f/PJX+oVX/5RfePVv62/3F7/80fxtvPS36wvItsPg14IdlvejfUE0n/xJ9am/+IWXfuHT/uKXPvVv46W/XV9Ath1eQEYOn76ACggNJ/3Cp/3FL33q38ZLf7u+gGw7vICMHD59ARUQGk76hU/7i1/61L+Nl/52fQHZdngBGTl8+gIqIDSc9Auf9he/9Kl/Gy/97foCsu3wAjJy+PQFVEBoOOkXPu0vfulT/zZe+tv1BWTb4QVk5PDpC6iA0HDSL3zaX/zSp/5tvPS36wvItsMLyMjh0xdQAaHhpF/4tL/4pU/923jpb9dfPiDrBv38+bCFFkT6tIDCt/uLP9Wfzqf+p/Vrvnevy//b519A4oTSC6gFEL/w6QKqv/iFl37V0/4pXvpWf+yA/L/dvwXkAjJ6IS8gb7/iZ/UtIEP/dcFOG9zWJ37Zm/qj/uIXXvpVT/uneOlbfS/I6g7ogmnBq+J+k7f1iV/zpf6ov/iFl37V0/4pXvpWX0BWd0AXTAteFbeA/CH/dX7p+aT9U3yq/9Px8v92f/Y7yP0Ocr+DvP2WvrC+BWR4eHqBnDa4rU/8sjf1R/3FL7z0q572T/HSt/q+Yld3QBcsXfAUnw6v/uJP/Un5hT9dl7+n/Uv13e5vqq99PrG+3wf4KyVJ8KlBbXwy2z/Y1N50PukXv/Cn6/JX8wmv+VJ+4dW/XU/9kT7N3+5PfQvIx39SRgaqnh5we4HEr/lO1+Wv5hNe86X8wqt/u576I32av92f+haQC0gtyc11XaD2BUz5hT/tvfxN9Wn+dn/pf/l/i50aLLwMVD09YOlr82u+03XNf9q/VN/t/qb62ucT69sLci/IdIlO4tMAEl6zpRdcePVv11N/pE/zt/tT3wJyAaklubmuC9S+gCm/8Ke9l7+pPs3f7i/9+4qNv85MBqqeHnB7gcSv+U7X5a/mE17zpfzCq3+7nvojfZq/3Z/6Tr8gJfDd61oQzd9eIOlL+4tf899elz+aX/h0fvVP+aVf/YVP9Ql//AUpge9e14Jo/vYCSV/aX/ya//a6/NH8wqfzq3/KL/3qL3yqT/gFpBwq17Ugat9eIOlL+4tf899elz+aX/h0fvVP+aVf/YVP9Qm/gJRD5boWRO3bCyR9aX/xa/7b6/JH8wufzq/+Kb/0q7/wqT7hF5ByqFzXgqh9e4GkL+0vfs1/e13+aH7h0/nVP+WXfvUXPtUn/AJSDpXrWhC1by+Q9KX9xa/5b6/LH80vfDq/+qf80q/+wqf6hF9AyqFyXQui9u0Fkr60v/g1/+11+aP5hU/nV/+UX/rVX/hUn/ALSDlUrmtB1L69QNKX9he/5r+9Ln80v/Dp/Oqf8ku/+guf6hP+eEDKIA1we10HfPv80i//Nd9p/tP65J/8SfULf1qf+rfrC8iyw+mCl+WRXvpFoAt4mv+0Pvknf1L9wp/Wp/7t+gKy7HC64GV5pJd+EegCnuY/rU/+yZ9Uv/Cn9al/u76ALDucLnhZHumlXwS6gKf5T+uTf/In1S/8aX3q364vIMsOpwtelkd66ReBLuBp/tP65J/8SfULf1qf+rfrC8iyw+mCl+WRXvpFoAt4mv+0Pvknf1L9wp/Wp/7t+gKy7HC64GV5pJd+EegCnuY/rU/+yZ9Uv/Cn9al/u76ALDucLnhZHumlXwS6gKf5T+uTf/In1S/8aX3q365fH5BakLZB4teCpfpT/hSv+d+9Lv/a86f7I33t+dr6NV9aX0CGDmrB0gVJ+VN8aM/Lw+Vfe8B0f6SvPV9bv+ZL6wvI0EEtWLogKX+KD+15ebj8aw+Y7o/0tedr69d8aX0BGTqoBUsXJOVP8aE9Lw+Xf+0B0/2RvvZ8bf2aL60vIEMHtWDpgqT8KT605+Xh8q89YLo/0teer61f86X1BWTooBYsXZCUP8WH9rw8XP61B0z3R/ra87X1a760voAMHdSCpQuS8qf40J6Xh8u/9oDp/khfe762fs2X1heQoYNasHRBUv4UH9rz8nD51x4w3R/pa8/X1q/50vrLB+TpA1Z/LUiK1wKIX/hXr8v/0/Pdfj6pf5ov5W+f3wISDusA0wVI8VoQ8Qv/6nWd3+n5bj+f1D/Nl/K3z28BuYBs79hR/usv4M+fR/1R89S/BaQcRj01UPhQ3g8tiPq38ZpP+oR/9br8Pz3f7eeT+qf5Uv72+e0FuRdke8eO8l9/AfeCPLofar6AXEBqR166voDMji/1by/IzP8fqYHCh/L2FTs18DA+veBt+e39TfWn/mm+lD+dT/i9IPeC1I68dP36C7iv2Ffv1wLy8oBMt0cBcfonvPqn86d4+Zfyp/NLX5s/nf92/AJyAfnQAV3AdMHTC5z2F/72+aUv9Vf88u/V6wvIBeQC8oED7YBoB1ib/9UDUPoXkAvIBeQC8o8OtH9AKKBO1xeQC8gF5AJyAfkHBxaQC8gF5AJyAbmAfO6xrq8Y+h1Pin9O9f+i0v7Cp/rkX8qf4m+fX/pSf8Wf+ns7fi/IvSD3gtwLci/IvSCf+1mln6D6Cf3peLkuf4Q/Xdf5p/rkj/oLL33iF171VJ/40/pekJe/ILVAWuDTeC2o9Al/ui7/U33yR/2Flz7xC696qk/8aX0BuYCMvmKnF+j2C6ILls4vfvmj/sKrv/iFVz3VJ/60voBcQC4gg1t0OkDUPw0g8QfW/Qea6kv7C7+AXEAuIHVLHtRPB4j6pwEk/sC6BeRXzNMB6ICF/4qGR59J+386Xv7LH+FP1199/+Tf6fmkr13fC3IvyL0gg1t2OkDUP/0BJP7Aur0gv2KeDkAHLPxXNOwF+WcH2v6LPz2/Nv7V90/+nJ5P+tr1l39Btg0SvxZIASC8+rfrp/Wrfzr/af/T+VL96t/mT8+vjV9Ahg5rgdoLGMon/LR+9ecA+IDOL+UXPp0v1a/+bX75c7q+gAxPQAvUXsBQPuGn9as/B1hAPnRA/mq/5b/4hT9dX0CGJ6AF0oIIH8qL4af1q3864Gn/0/lS/erf5k/Pr41fQIYOa4HaCxjKJ/y0fvXnAHtB7gUZLMkCMjDvH+gC8mfo4GP4AvKxP9o/HY78bfNL3+n6AjI8AS1QewFD+YSf1q/+HGAvyL0ggyVZQAbm7QXpF3Rob/3P6uoHXKpf+PQHQKpf/dv88ud0/fqAPG1Q2j9dQOFTfcKnF0T8aT31R/OJ/9Xx8v/0fNLXri8gyw63L1hZPn/H2u4vfvkr/OkAON3/dn+kr11fQJYd1gVOL0hZ/gLy16+HFqfndxqv/bldn/Sn9QVk6iDwC8iuwfJX3U8HwOn+t/sjfe36ArLssC5wekHK8veC3Avy4Yq9+n7r/iwg5VBYf/UFUoCH9sRw+asGmk/8r7rsGnQAAAG+SURBVI6/3R/pa9cXkGWH2xesLH8vyL0g94JsX7JH/PoJfFLb3+i9gPwbLv6ZQ/6qu/ZP/K+Ov90f6WvXj78g2wOOfw7MgTnwrAMLyGedG24OzIG3d2AB+fZHvAHnwBx41oEF5LPODTcH5sDbO7CAfPsj3oBzYA4868AC8lnnhpsDc+DtHVhAvv0Rb8A5MAeedWAB+axzw82BOfD2Diwg3/6IN+AcmAPPOrCAfNa54ebAHHh7BxaQb3/EG3AOzIFnHVhAPuvccHNgDry9AwvItz/iDTgH5sCzDiwgn3VuuDkwB97egQXk2x/xBpwDc+BZBxaQzzo33ByYA2/vwALy7Y94A86BOfCsAwvIZ50bbg7Mgbd3YAH59ke8AefAHHjWgQXks84NNwfmwNs7sIB8+yPegHNgDjzrwALyWeeGmwNz4O0dWEC+/RFvwDkwB551YAH5rHPDzYE58PYOLCDf/og34ByYA886sIB81rnh5sAceHsHFpBvf8QbcA7MgWcdWEA+69xwc2AOvL0DC8i3P+INOAfmwLMOLCCfdW64OTAH3t6BBeTbH/EGnANz4FkHFpDPOjfcHJgDb+/AfwGIPo36Pg3y5QAAAABJRU5ErkJggg==)