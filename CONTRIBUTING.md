# Contributing

## Supported Config
Treat [arri-CG.ocio](/Users/j7s/Documents/ARRI-Houdini/arri-CG.ocio) as the supported config for this repository.

The original ARRI studio config is kept as a reference artifact only. Do not treat it as the primary target when making workflow-facing changes.

## Validation Requirement
Before submitting changes, run:

```bash
ociocheck --iconfig arri-CG.ocio
```

If your change touches the reference file or documentation comparing both files, also run:

```bash
ociocheck --iconfig arri-studio-combined-config-v1.0.0_aces-v1.3_ocio-v2.1.ocio
```

## Documentation Requirements
If you change roles, file rules, or the supported workflow assumptions:

- update [README.md](/Users/j7s/Documents/ARRI-Houdini/README.md)
- update [docs/CONFIG_COMPARISON.md](/Users/j7s/Documents/ARRI-Houdini/docs/CONFIG_COMPARISON.md)

If you add, replace, or remove LUT dependencies:

- update [THIRD_PARTY.md](/Users/j7s/Documents/ARRI-Houdini/THIRD_PARTY.md)

## Scope Guardrails
- Keep `arri-CG.ocio` as the Houdini-first config unless there is a deliberate project decision to change support scope.
- Do not add a repository-wide license file until the maintainer explicitly chooses one.
- Do not claim redistribution rights for bundled third-party assets unless the maintainer has verified them.
