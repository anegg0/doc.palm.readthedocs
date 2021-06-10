# Local tests helper scripts

Scripts in this directory are here to help documentation contributors to test their changes before
pushing them as a pull request.

## Requirements

For this, you need Circle CI CLI and Docker installed on your computer:

- [Circle CI CLI](https://circleci.com/docs/2.0/local-cli/)
- [Docker](https://docs.docker.com/get-docker/)

## Running the scripts

Scripts are expected to be ran from the project root.

Run the following commands:

- `CI/build_tools/scripts/test_build.sh` to test the doc build
- `CI/build_tools/scripts/test_guidelines.sh` to test the doc against our writing guidelines
- `CI/build_tools/scripts/test_links.sh` to test hyperlinks are not broken
- `CI/build_tools/scripts/test_lint.sh` to test that Markdown syntax is correct
- `CI/build_tools/scripts/test_all.sh` to test all tests

## Previewing the doc site

It's possible to render doc locally or preview on ReadTheDocs to check that everything is displayed correctly.

Refer to the [common repository wiki](https://github.com/Consensys/doc.common/wiki/MkDocs-And-Custom-Markdown-Guide#preview-the-documentation)
