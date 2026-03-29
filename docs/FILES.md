# Modular Monolith Monorepo

## Reason

We kept a modular monolith for the first stage because team size and delivery speed mattered more than independent service scaling, and we accepted tighter deploy coupling until domain boundaries stabilized.

## Subprojects

Project can be a single project or a monorepo with multiple subprojects with different languages.

For example:
- api - NestJS
-- modules
-- ...
- ai - FastAPI
-- modules
-- ...
- performance - Rust
-- modules
-- ...

Every subproject can have its own `.git` repository or be a part of the modular monolith monorepo.


EXT=[py/ts/rs/go] # one of

## File Structure for every subproject example:

```
%SUBPROJECT(NAME,EXT)%
- .env
- [alembic.ini]
- build
-- ...
- [main.tf]
- [pyproject.toml]
- [package.json]
- [package-lock.json]
- [Cargo.toml]
- [Cargo.lock]
- [config.toml/yaml/json]
- [VACANCY.md]
- src
-- common
--- decorators
---- swagger.%EXT%
--- deps.%EXT%
--- exceptions
---- handler.%EXT%
--- security.%EXT%
-- config
--- database.%EXT%
--- settings.%EXT%
-- main.%EXT%
-- migrations
--- %TIMESTAMP%_create_users_table.migration.%EXT%
-- modules
--- app.module.%EXT%
--- auth
---- [__init__.py]
---- [index.%EXT%]
---- auth.controller.%EXT%
---- auth.module.%EXT%
---- auth.service.%EXT%
---- dto
----- [__init__.py]
----- [index.%EXT%]
----- auth.dto.%EXT%
---- google-auth
----- google-auth.controller.%EXT%
----- google-auth.service.%EXT%
----- google-auth.module.%EXT%
--- user
---- user.controller.%EXT%
---- user.dto.%EXT%
---- user.model.%EXT%
---- user.service.%EXT%
---- user.module.%EXT%
--- payment
---- stripe
----- stripe.controller.%EXT%
----- stripe.service.%EXT%
----- stripe.module.%EXT%
---- revenuecat
----- revenuecat.controller.%EXT%
----- revenuecat.service.%EXT%
----- revenuecat.module.%EXT%
---- payment.module.%EXT%
```

## Project Structure Example

theproject
- .env
- .gitignore
- AGENTS.md
- GEMINI.md
- README.md
- docs/*
- skills/*
- scripts/*
- subproject1 [EXT=py]
- subproject2 [EXT=ts]
- ...
- subprojectN [EXT=rs]
