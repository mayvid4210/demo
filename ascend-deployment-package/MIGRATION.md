# Migration review guide

Framework: Flask
Startup target: `app:app` (application_object)
Selected analysis root: "."
Python image version: 3.13
Executable configuration available: True

## Setup and validation
1. Apply reviewed files to the selected application root (.); build context is that directory, not the complete repository.
2. Exception: place .github/workflows/ascend-migration.yml and render.yaml at the repository root. Their paths refer to the selected application root; adjust them if extracting the application into a new standalone repository.
3. Keep existing files unchanged and merge missing configuration manually. Read the generated migration guide before using any template.
4. For generated Dockerfile: docker build -t ascend-app . ; run with docker run --rm -p 10000:10000 -e PORT=10000 ascend-app after adding required runtime environment configuration.
5. Smoke-test http://localhost:10000/health; configure SMOKE_TEST_PATH and application environment in CI before relying on that workflow.
6. Review render.yaml manually against Render's current official schema and free-plan limits. Set required variables in the dashboard; leave optional defaulted variables unset. No cloud resources have been created.

## Warnings and manual review
- Generated files require manual review. No build, startup, or cloud deployment has been validated.
- Python version unknown: using configurable default 3.13; verify compatibility before building.
- Review persistent/local storage and database files before building. Add explicit exclusions for development/private data; database and build-asset exclusions are commented suggestions so required runtime assets are preserved. Free Render instances do not provide persistent disks.
- Review the HTTP smoke-test path and authentication/environment requirements; detected routes are not verified health checks.

## Scanner limitations


## Environment names (never values)
- SECRET_KEY: no_default_detected

## Existing files preserved
- ".env.example"

## Unresolved readiness findings
- NET001: Loopback-only server binding. Configure the production server to bind to 0.0.0.0 or the platform-required interface.
- NET002: Hardcoded application port. Read the platform port from an environment variable, with an appropriate local default.
- STO002: Potential local persistent storage. Review persistence needs; configure a durable volume or external storage and backups when necessary.
- CTR001: No Dockerfile detected. For a container-based deployment, add and review a Dockerfile in a later configuration step.
- CTR002: No .dockerignore detected. Add a .dockerignore before container builds to exclude environment files, caches, and development artifacts.
- RUN002: Python runtime version is unknown. Declare a supported Python version in .python-version or pyproject.toml project.requires-python.

No generated files have been built or deployed by Ascend. A generic Python image cannot guarantee runtime compatibility. No databases, disks, credentials, or paid plans are provisioned. Review source before any build; pip/build systems may execute project build code during your manual build.

## Complete package review

See `ASCEND-PACKAGING.json` for the source scope, included files, exclusions, and security limitations. Uploaded files were not changed; the export may exclude unsafe files listed there. Configuration review, CI validation, and successful deployment are still required.
