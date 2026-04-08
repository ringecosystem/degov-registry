# The DAO registry of DeGov Square

This repository is the public registry used by DeGov Square https://square.degov.ai.
It lists DAOs that use DeGov as their governance framework and provides the YAML
files consumed by the Square frontend.

DeGov is an open-source project that enables anyone to build their own DAO,
provided the DAO's contracts are based on the standard OpenZeppelin Governor
contract.

## Repository layout

- `config.yml`: the registry index. It maps a network to one or more DAO entries.
- `daos/*.yml`: one DAO configuration file per DAO.
- `schemas/config.schema.json`: validation rules for `config.yml`.
- `schemas/dao.schema.json`: validation rules for each DAO config file.
- `.github/workflows/check.yml`: CI validation for pull requests and pushes.

## Contributing a new DAO

External contributors can use either of these collaboration paths:

1. Open a GitHub issue with the DAO details if you want maintainers to help
   prepare the config files.
2. Open a pull request directly if you already know the target format.

For both paths, the recommended onboarding flow is:

1. Add a new DAO config file under `daos/<dao-code>.yml`.
2. Add the DAO to the correct network list in `config.yml`.
3. Start with `state: draft`.
4. After the DAO has been synced and verified, update the entry to
   `state: active`.

Some existing entries omit `state`, but new contributions should prefer writing
the state explicitly so the lifecycle stays easy to review.

## Contribution guide

Use the detailed guide for field-by-field documentation and examples:

- [Registry contribution guide](docs/spec/20260403__registry-contribution-guide.md)

## Validation

The repository validates both files in CI:

- `config.yml` is validated against `schemas/config.schema.json`
- `daos/*.yml` are validated against `schemas/dao.schema.json`

When opening a PR, make sure the YAML you add matches the schema and follows the
examples already present in this repository.
