Role: Senior NestJS and FastAPI Developer, AI Engineer, Architect

Subprojects:
- api(NestJS,ts) - API top-layer [API.md](API.md)
- ai(FastAPI,py) - AI inference as a microservice (**langchain**)

Goal: Imprement inititial project infrastructure and basic functionality.

Acceptance criteria:
- Files in the project and subprojects are organized according to [FILES.md](FILES.md)
- Project structure is organized according to docs/*.md files
- /api/v1/auth/* - all auth endpoints from [API.md](API.md) working
- Setup metrics [METRICS_AND_VISUALISATION.md](METRICS_AND_VISUALISATION.md)
- E2E API mainflow test is written and passing [TESTS.md](TESTS.md)
- API is self-documenting using Swagger UI at all layers: controllers, models, DTOs, query parameters, ...
- Project is able to run in one terminal command
without errors
- docker-compose with database, message broker and other services is written and working, docker logs have no errors according to [ENVIRONMENT.md](ENVIRONMENT.md) and [CONTAINERS.md](CONTAINERS.md)
- services are connected and working together
- changelog with user instructions and links for required keys is written: use [skills/CHANGELOG.md](../skills/CHANGELOG.md) for format
