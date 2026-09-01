# Contributing

Thank you for helping complete the project archive. The goal is to preserve the real engineering work, make every artifact reproducible, and give each contributor clear authorship.

## Before contributing

1. Accept the GitHub collaborator invitation.
2. Make sure the email used by Git is connected to your GitHub account.
3. Read the relevant folder README before adding files.
4. Do not commit passwords, access tokens, private Wi-Fi credentials, or unrelated personal information.

```bash
git config user.name "Your Name"
git config user.email "your-github-email@example.com"
```

## Branch workflow

Do not push project changes directly to `main`.

```bash
git switch main
git pull --ff-only
git switch -c firmware/ads1115-driver

# Add and review your files.
git add firmware/
git diff --cached
git commit -m "Add ADS1115 acquisition firmware"
git push -u origin firmware/ads1115-driver
```

Open a pull request and describe what was added, how it was tested, and any limitations.

Recommended branch prefixes:

- `firmware/` for STM32 or companion-device code
- `hardware/` for schematic, PCB, enclosure, or BOM work
- `docs/` for documentation and diagrams
- `tests/` for calibration data and validation scripts
- `fix/` for corrections

## Where files belong

| Contribution | Destination |
|---|---|
| Importable STM32CubeIDE project and `.ioc` file | `firmware/stm32/` |
| Arduino/ESP companion experiments | `firmware/experiments/` |
| KiCad project, schematic, and PCB | `hardware/kicad/` |
| BOM and sourcing information | `hardware/bom/` |
| Enclosure CAD or fabrication drawings | `hardware/enclosure/` |
| Calibration CSV files and scripts | `tests/calibration/` |
| Serial captures and test reports | `tests/results/` |
| Technical explanations | `docs/` |

## Contribution quality

- Prefer source files over screenshots.
- Include a short README beside specialized assets.
- Export a human-readable preview for EDA or CAD files when practical.
- State the hardware revision, tool version, and test conditions.
- Keep generated build directories and temporary files out of Git.
- Do not rewrite another person's authorship or backdate commits.
- Use `Co-authored-by` only when the named person genuinely worked on that commit and has agreed to the attribution.

## Pull-request checklist

- [ ] Files are placed in the correct directory.
- [ ] No secrets or unrelated personal data are included.
- [ ] Build/generated files are excluded.
- [ ] Documentation explains how the contribution was created or tested.
- [ ] Claims are supported by measurements, source files, or clearly labeled as targets.
- [ ] Images have meaningful names and captions.

## Large files

Avoid committing raw videos, duplicate archives, and generated build outputs. Discuss Git LFS or an external release asset before adding any individual file approaching GitHub's file-size limit.
