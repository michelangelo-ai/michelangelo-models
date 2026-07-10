# michelangelo-models

Pluggable, community-contributed model architectures for
[Michelangelo](https://github.com/michelangelo-ai/michelangelo) — installable
as a standalone Python package and integrated with core Michelangelo's
training, serving, and registry infrastructure via a plugin contract rather
than a hard dependency in either direction.

```bash
pip install michelangelo-models[<family>-<arch>]
```

## Why a separate repo

Model architectures evolve independently of the platform that trains and
serves them, and different architectures often need conflicting library
versions (PyTorch, Ray, CUDA). Splitting architectures out of core
Michelangelo into their own repo, installed via optional extras, lets each
one pin what it needs without forcing a single environment on every user of
the platform.

## Model families

"Foundation model" is one model family among others here — not the whole
scope of the repo. The repo is organized by family and architecture, e.g.:

```
architectures/
├── foundation/
│   └── <arch>/
├── embedding/
│   └── <arch>/
└── ranking/
    └── <arch>/
```

New families are welcome; each is its own top-level directory under
`architectures/`, not partitioned by contributing company. See
[Contributing](#contributing) for how ownership works.

## Repo structure

```
michelangelo-models/
├── core/
│   ├── interfaces.py   # ABCs: Backbone, Tokenizer/Processor, Head, Adapter
│   ├── registry.py      # entry-point-based self-registration
│   └── model_spec.py    # ModelSpec (pydantic) — architecture metadata contract
├── architectures/
│   └── <family>/
│       └── <arch>/
│           ├── model.py
│           ├── OWNERS
│           └── pyproject.toml
├── training/     # shared training entrypoints/utilities
├── serving/      # adapters mapping a ModelSpec to core Michelangelo's serving/packaging infra
└── evaluation/   # shared eval harnesses
```

`core/interfaces.py` defines the contract every architecture implements.
`core/registry.py` uses Python entry points so `architectures/<family>/<arch>/`
packages self-register without `core/` ever importing them directly.

## Installing an architecture

Each architecture is installed via its own extra:

```bash
pip install michelangelo-models[foundation-<arch>]
pip install michelangelo-models[embedding-<arch>]
```

Only that architecture's dependencies are pulled in.

## Contributing

- New architectures live under `architectures/<family>/<arch>/` with their
  own `OWNERS` file listing individual maintainers (and affiliation) — not a
  company-owned directory.
- `core/` changes require review from maintainers across at least two
  different affiliations, since changes here affect every architecture.
- New model families require an RFC first.

See [`CONTRIBUTING.md`](CONTRIBUTING.md) for the full process (coming soon).

## License

[Apache License 2.0](LICENSE)
