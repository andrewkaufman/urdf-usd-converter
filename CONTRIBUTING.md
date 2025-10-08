# Contributing to urdf-usd-converter

If you are interested in contributing to urdf-usd-converter, your contributions will fall
into three categories:
1. You want to report a bug, feature request, or documentation issue
2. You want to implement a feature or bug-fix for an outstanding issue
3. You want to propose a new Feature and implement it

In all cases, first search the existing [GitHub Issues](https://github.com/newton-physics/urdf-usd-converter/issues) to see if anyone has reported something similar.

If not, create a new [GitHub Issue](https://github.com/newton-physics/urdf-usd-converter/issues/new/choose) describing what you encountered or what you want to see changed. If you have feedback that is best explained in code, feel free to fork the repository on GitHub, create a branch demonstrating your intent, and either link it to the GitHub Issue or open a Pull Request back upstream. See [Code Contributions](#code-contributions) for more details.

Whether adding details to an existing issue or creating a new one, please let us know what companies are impacted.

## Code contributions

If you want to implement a feature, or change the logic of existing features, you are welcome to modify the code on a personal clone/mirror/fork. See [Building](#building) for more details.

If you want to contribute your changes back upstream, please first start a GitHub Issue as described above.

If you intend to submit a Pull Request:
- First, ensure alignment with the Code Owners on the associated Issue, to avoid redundant work or wasted iterations.
- Develop your changes on a well named [development branch](#development-branches) within your personal clone/mirror/fork.
- Run all test suites locally & ensure passing results in your dev environment.
- Be prepared to sign the Newton Contributor License Agreement, see below.

Please note that in some cases, we may not merge GitHub Pull Requests directly. We will take suggestions under advisement and discuss internally. We may rebase your commits to provide alignment with internal development branches.

### Contributor License Agreement

All contributions to this project require a signed Contributor License Agreement (CLA) on file, see [Legal Requirements](https://github.com/newton-physics/newton-governance/blob/main/CONTRIBUTING.md#legal-requirements) in the Newton contribution guidelines.

Newton repositories use [EasyCLA](https://lfcla.com/) for managing CLAs. You may sign the appropriate CLA in a pull-request workflow; see [the EasyCLA documentation](https://docs.linuxfoundation.org/lfx/easycla/v2-current/contributors) for instructions.

### Branches and Versions

The default branch is named `main` and it is a protected branch. Our internal CI Pipeline automatically builds & tests all changes from this branch on both Windows and Linux. However, all new features target the `main` branch, and we may merge code changes to this branch at any time; it is not guaranteed to be stable/usable and may break API/ABI regularly.

We advise to use an official published wheel of the urdf-usd-converter, or source from the GitHub tag associated with it, to ensure stability.

### Development Branches

For all development, changes are pushed into a branch in personal development forks, and code is submitted upstream for code review and CI verification before being merged into `main` or the target release branch.

We do not enforce any particular naming convention for development branches, other than avoiding the reserved branch patterns `main` and `production/*`. We recommend using legible branch names that imply the feature or fix being developed.

All code changes must contain either new unittests, or updates to existing tests, and we won't merge any code changes that have failing CI pipelines or sub-standard code coverage.

## Developing

This project uses [uv](https://docs.astral.sh/uv/) as the primary dev tool and [poethepoet](https://github.com/nat-n/poethepoet) for command automation.

If you do not have `uv` installed, see the [development requirements](#development-requirements).

### Quick Start

Once `uv` is installed, you can use the following commands:

```bash
# Build the project
uv build

# Run linting
uv run --group dev poe lint

# Run tests
uv run --group dev poe test

# Run the CLI
uv run urdf_usd_converter --help
```

### Persistent Environment

If you prefer a traditional workflow with a persistent virtual environment, use the build scripts. These will create a `.venv` which you can then activate and use `poe` commands directly.

- Build and setup environment: `./build.sh` or `.\build.bat`
- Activate the venv:
    - Linux: `source .venv/bin/activate`
    - Windows: `call .venv\Scripts\activate.bat`
- Run commands in activated environment: `poe lint`, `poe test`, `urdf_usd_converter --help`.

### Development Requirements

To use the build scripts locally, you need uv installed:

**Linux/macOS:**
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

**Windows:**
```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

### Common Commands

- Update dependencies: Edit `pyproject.toml` and then run `uv lock`
- Build the sdist and wheel: `uv build`
- Build just the wheel: `uv build --wheel`
- Run linting: `uv run --group dev poe lint`
- Run tests: `uv run --group dev poe test`
- Run auto-formatters: `uv run --group dev poe format`
- Run the CLI: `uv run urdf_usd_converter <args>`

## Testing

The unittests can be run in several ways:

```bash
# via uv
uv run --group dev poe test

# in an activated venv
poe test

# individual test discovery
poe test -k testLinks.TestLinks.test_inertia
```

The converter CLI can be used as well:

```bash
# Direct execution
uv run urdf_usd_converter --help

# In activated venv
urdf_usd_converter --help
```

## Changing OpenUSD Runtimes

By default the urdf-usd-converter uses OpenUSD v25.05 & OpenUSD Exchange compiled for this same flavor. OpenUSD Exchange SDK can be compiled for many flavors of OpenUSD and Python. You can switch to a different flavor of OpenUSD by changing the `usd-exchange` version metadata within the the pyproject.toml or sdist.

### Requesting new Build Flavors

If none of the existing USD flavors meet the requirements of your runtime, you have two options:
1. Build [OpenUSD Exchange SDK](https://github.com/NVIDIA-Omniverse/usd-exchange) from source as and when you need to & manage the build artifacts yourself
2. Submit an Feature Request to add a new flavor to our matrix
