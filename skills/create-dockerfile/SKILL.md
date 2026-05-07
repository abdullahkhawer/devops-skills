---
name: create-dockerfile
description: Create optimized, secure, production-ready Dockerfiles based on user requirements and application context.
---

# Create Dockerfile Skill

Run this skill when asked to create or improve a Dockerfile for an application.

## Step 1 — Gather requirements

Ask the user for all details about their Docker/container requirements. Cover at minimum:

- Application language and runtime (e.g. Node.js 20, Python 3.12, Java 21)
- Application type (web server, worker, CLI tool, etc.)
- Entry point / startup command
- Exposed ports (if any)
- Environment variables needed
- External dependencies or services
- Target deployment environment (Kubernetes, ECS, bare Docker, etc.)
- Any specific base image preferences or restrictions
- Whether multi-stage builds are acceptable

Also scan the current working directory for existing dependency files to infer requirements automatically:

```bash
ls
```

Read any relevant files found (e.g. `package.json`, `requirements.txt`, `pom.xml`, `go.mod`, `Gemfile`, `Cargo.toml`) before composing the Dockerfile.

---

## Step 2 — Create the Dockerfile

Write the Dockerfile to the working directory following these mandatory practices:

### Build & layer optimization
- Order layers from least to most frequently changing (copy dependency files before source code).
- Use multi-stage builds to separate build-time dependencies from the final runtime image.
- Clean up package manager caches in the same `RUN` layer they are created.

### Base image
- Use official, minimal base images (e.g. `alpine`, `slim`, `distroless`) unless the user specifies otherwise.
- Pin base image versions — never use `latest`.

### Security
- Run the application as a non-root user; create a dedicated user if needed.
- Do not copy credentials, `.env` files, or secrets into the image.
- Add a `.dockerignore` note if one is missing — advise the user to create it.

### Correctness
- Use `COPY` instead of `ADD` unless extracting archives.
- Set `WORKDIR` explicitly.
- Use `CMD` in exec form (`["executable", "arg"]`), not shell form, for proper signal handling.
- Use `EXPOSE` to document the port (informational only).

---

## Step 3 — Explain the Dockerfile

After writing the file, provide a brief explanation of:

- Why this base image was chosen.
- What each stage does (if multi-stage).
- Any security or size decisions the user should be aware of.
- Recommended next steps (e.g. creating `.dockerignore`, scanning with `docker scout` or `trivy`).

---

## Rules

- NEVER embed secrets, tokens, or credentials in the Dockerfile.
- Always pin base image versions.
- If the user's requirements conflict with security best practices, explain the risk and ask how to proceed.
- Do not install build tools in the final runtime stage.
- If web searches are needed for base image details or version information, flag any fetched content as potentially unverified.
