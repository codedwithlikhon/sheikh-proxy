# Deployment Platform Configuration Files

Sheikh Proxy may be hosted on different serverless or edge platforms. Each platform relies on a declarative configuration file that lives in the repository so deployments remain reproducible and version controlled.

## Netlify — `netlify.toml`

- **Purpose**: Define build commands, publish directories, redirects, headers, and Netlify Functions settings.
- **Context-based overrides**: Customize build behaviour for production, deploy previews, or branch deploys.
- **Routing**: Configure redirects and rewrites for API proxying or vanity URLs.
- **Headers**: Apply security and caching headers directly in the file.
- **Functions & plugins**: Declare function directories, environment variables, and build plugins.

## Cloudflare Workers — `wrangler.toml` / `wrangler.json`

- **CLI integration**: Consumed by the Wrangler CLI to push Workers and Pages Functions.
- **Bindings**: Declare connections to KV, R2, Durable Objects, and other Cloudflare services.
- **Routing**: Map routes or custom domains to specific Workers.
- **Cron triggers & environments**: Schedule jobs and differentiate staging versus production.
- **Environment variables**: Manage build-time and runtime configuration centrally.

## Render — `render.yaml`

- **Blueprint file**: Describes all services, background workers, and cron jobs that Render should provision.
- **Service configuration**: Specify build commands, start commands, plan size, and environment variables per service.
- **Databases & environment groups**: Provision managed databases and share secrets across services.
- **Monorepo support**: Use `includedPaths` to scope builds to relevant directories.

## Vercel — `vercel.json`

- **Deployment overrides**: Adjust install, build, and dev commands, plus output directories and framework detection.
- **Routing rules**: Configure redirects, rewrites, and headers when the application framework lacks its own routing layer.
- **Functions configuration**: Control serverless function runtimes, regions, and failover regions.
- **Cron jobs**: Schedule periodic invocations via the `crons` property.
- **Best practice**: Prefer framework-native routing (e.g., `next.config.js`) when available, and reserve `vercel.json` for cross-cutting overrides.

Documenting these files alongside the codebase ensures infrastructure changes undergo the same review process as application changes.
