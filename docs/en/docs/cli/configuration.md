# Configuration

Every project you build must declare a [tool.walkai] section inside its pyproject.toml.

```toml
[tool.walkai]
entrypoint = "python -m app.main"
os_dependencies = ["git", "gettext", "cron"]
ignore = ["datasets/sample.csv"]
```

- entrypoint (required) is the command that will run when the container starts.
- os_dependencies (optional) is a list of Debian packages to install in the image. The default Heroku builder synthesises a project.toml describing these dependencies so the deb-packages buildpack can install them.
- ignore (optional) is a list of files or directories that walkai should exclude from the container image (useful for large datasets you plan to mount separately).
