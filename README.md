<p align="center">
    <a href="https://froxlor.org" target="_blank">
        <img src="https://raw.githubusercontent.com/froxlor/framework/refs/heads/main/packages/ui/resources/img/icon.png" width="80" alt="froxlor logo">
    </a>
</p>

# froxlor

> [!CAUTION]
> ## Very early development — no security reports, please
>
> The `main` branch of `froxlor/froxlor` is in a **very early stage of development**. It is incomplete, changes
> constantly and is **not meant to be used in production**.
>
> **Please do NOT open security reports, advisories or issues about security topics for this branch.** They will not
> be accepted or processed. Known gaps are expected at this stage and are being worked on.

> [!IMPORTANT]
> ## Looking for the stable version?
>
> The current stable release, **froxlor 2.3**, lives in the [`v2.3` branch](https://github.com/froxlor/froxlor/tree/v2.3)
> of this repository. If you want to run froxlor on a real server, use that branch or one of its releases — **not** `main`.
>
> - Releases: [github.com/froxlor/froxlor/releases](https://github.com/froxlor/froxlor/releases) or on the
>   [froxlor website](https://froxlor.org)
> - Documentation: [docs.froxlor.org](https://docs.froxlor.org/)

froxlor is a hosting control panel for managing servers, customers and their hosting resources (domains, mail, FTP,
databases, ...). This repository is the next generation of froxlor, built on [Laravel](https://laravel.com).

## How it fits together

This repository is the **application shell**: a Laravel application that wires the froxlor packages together. The
actual functionality lives in Composer packages, which are pulled in via `composer.json`.

```text
froxlor/froxlor            Laravel application (this repository)
└── froxlor/framework      Core framework, bundles the packages below
    ├── froxlor/core       Tenants, users, environments, nodes, roles/permissions,
    │                      plans/resources, settings, audit logging, shared services
    ├── froxlor/packages   Package management
    └── froxlor/ui         UI components (tables, forms, widgets, ...)
```

Feature modules (e.g. domains, mail, FTP, databases, web) are separate packages that build on `froxlor/core`.

### Concepts in a nutshell

- **Tenant**: a customer or managed hosting entity. There is one root tenant, all others are its descendants.
  The tenant is the primary authorization boundary.
- **User**: a person logging into froxlor, assigned to tenants and/or environments with individual roles and plans.
- **Environment**: an execution/deployment context (e.g. a jail on a node) in which resources such as domains,
  mailboxes or FTP accounts live.
- **Node**: a machine (local or remote) that hosts environments. Communication with a node happens through adapters.
- **Plan / Resource**: a plan is a reusable set of resource limits, resources describe what can be limited.

Infrastructure changes follow this flow: Controller → Service → Domain Action → Job/Event → Adapter → Infrastructure.

## Related repositories

| Repository                                                    | Purpose                                            |
|---------------------------------------------------------------|----------------------------------------------------|
| [froxlor/framework](https://github.com/froxlor/framework)     | Framework packages (`core`, `packages`, `ui`) |
| [froxlor/container](https://github.com/froxlor/container)     | Docker image and development setup                 |

## Getting started

The easiest way to try froxlor or to set up a development environment is the
[froxlor container](https://github.com/froxlor/container) (see its `DEVELOPERS.md`).

Running the application directly requires PHP 8.5+, Composer and Node.js:

```bash
composer install
cp .env.example .env
php artisan key:generate
php artisan migrate
php artisan froxlor:install
npm install && npm run build
```

Start the development services (web server, queue worker, log tail, Vite, scheduler):

```bash
composer dev
```

Run the tests:

```bash
composer test
```

## License

froxlor is licensed under the [LGPL-2.1-only](LICENSE).
