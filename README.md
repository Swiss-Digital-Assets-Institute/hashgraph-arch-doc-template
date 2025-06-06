
# Architecture documentation template

This template repo is meant to be used as a basis for writing architecture documentation. You can follow these steps to add the documentation to your repo:

In the root folder of your repo

```shell
git clone git@github.com:Swiss-Digital-Assets-Institute/hashgraph-arch-doc-template.git docs
rm -rf docs/.git
cd docs
git add .
git commit -m "documentation template"
```

## The above section can be deleted after the merge

---

# Architecture Documentation

## Overview

This repository contains the architecture documentation for the Managed Transaction gateway. The documentation is written in AsciiDoc and can be converted to HTML or PDF using Docker and docker-compose.

## Documentation Structure

The `docs` directory contains the architecture documentation for the project, following the THA template:

- **Introduction and Goals**: Overview, requirements, and stakeholders.
- **Architecture Constraints**: Technical and organizational constraints.
- **System Scope and Context**: Business and technical context.
- **Solution Strategy**: Fundamental decisions and solution strategies.
- **Building Block View**: Static decomposition of the system.
- **Runtime View**: Dynamic aspects and key scenarios.
- **Deployment View**: Technical infrastructure and deployment.
- **Cross-cutting Concepts**: Overarching concepts and patterns.
- **Architecture Decisions**: Key architectural decisions (ADRs).
- **Quality Requirements**: Quality tree and quality scenarios.
- **Risks and Technical Debt**: Known risks and technical debt.
- **Glossary**: Domain and technical terms.

## Requirements

To build the documentation, you need:

1. **Docker**: Install Docker from [docker.com](https://www.docker.com/get-started)
2. **Docker Compose**: Usually comes with Docker Desktop for Windows and Mac. For Linux, you might need to install it separately.

## Building the Documentation

We use a `docker-compose-arch.yml` file to define the build process for both HTML and PDF outputs.

### To generate HTML:

#### For Docker desktop version for Windows and Mac

```sh
docker compose -f docker-compose-arch.yml up --no-recreate build-html
```

#### For Docker on Linux

```sh
docker-compose -f docker-compose-arch.yml up --no-recreate build-html
```

This will generate `index.html` in the `docs/architecture/build-html` directory.

### To generate PDF:

#### For Docker desktop version for Windows and Mac

```sh
docker compose -f docker-compose-arch.yml up --no-recreate build-pdf
```

#### For Docker on Linux

```sh
docker-compose -f docker-compose-arch.yml up --no-recreate build-pdf
```

This will generate `index.pdf` in the `docs/architecture/build-pdf` directory.

### Publishing to Confluence:

We use Confluence Publisher to automatically publish the AsciiDoc documentation to a Confluence space.

You can manually publish the documentation to Confluence by running the following command:
```sh
docker-compose -f docker-compose-arch.yml run publish-confluence
```

### Required Confluence Environment Variables

Before running the publish confluence command, ensure the following environment variables are set:

- `ROOT_CONFLUENCE_URL`: Base URL for your Confluence instance.
- `USERNAME`: Confluence username (or API token for cloud instances).
- `PASSWORD`: Confluence password (or API token for cloud instances).
- `SPACE_KEY`: Confluence space key where the documentation will be published.
- `ANCESTOR_ID`: ID of the parent (ancestor) page under which the documentation will be published.
- `PAGE_TITLE_PREFIX`: (Optional) Prefix for page titles.
- `PAGE_TITLE_SUFFIX`: (Optional) Suffix for page titles.
- `PROJECT_VERSION`: Version of the project, included in the version message.

### Example Environment Setup

```sh
export ROOT_CONFLUENCE_URL="https://your-confluence-instance.com"
export USERNAME="your-username"
export PASSWORD="your-api-token"
export SPACE_KEY="YOUR_SPACE_KEY"
export ANCESTOR_ID="12345678"
export PAGE_TITLE_PREFIX="Draft - "
export PAGE_TITLE_SUFFIX=" - Architecture"
export PROJECT_VERSION="0.0.1"

docker-compose -f docker-compose-arch.yml run publish-confluence
```

## Customization

The `docker-compose-arch.yml` file is set up to use the `asciidoctor/docker-asciidoctor` image, which includes support for diagrams. If you need to customize the build process:

1. Modify the `command` section in the `docker-compose-arch.yml` file.
2. Add any necessary volumes or environment variables.

## Publishing

We use GitHub Actions to automatically build and publish the documentation to GitHub Pages:

1. The documentation is built automatically when changes are pushed to the main branch.
2. GitHub Actions uses the `docker-compose-arch.yml` file to generate the HTML and PDF versions.
3. The generated files are then published to GitHub Pages.

Note: There's no need to commit the generated `index.html` or `index.pdf` files. The GitHub Action handles the build and publish process.

## Contributing

When contributing to the documentation:

1. Write your content in the appropriate `.adoc` files within the `docs/architecture` directory.
2. Use AsciiDoc syntax for formatting.
3. If adding diagrams, ensure they are in a format supported by asciidoctor-diagram.
4. You can build the documentation locally using the docker-compose commands to verify your changes before pushing.
5. Submit a pull request with your updates.
