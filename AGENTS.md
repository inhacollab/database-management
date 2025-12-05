# AGENTS.md

## Build/Lint/Test Commands
- **Build**: No build command defined yet. Add to package.json or Makefile as needed.
- **Lint**: No lint command defined yet. Consider adding ESLint or similar.
- **Test**: No test command defined yet. Use `npm test` or `pytest` for running tests.
- **Run single test**: No specific command yet. For Jest: `npm test -- --testNamePattern="test name"`. For pytest: `pytest path/to/test.py::test_function`.

## Code Style Guidelines
- **Imports**: Use ES6 imports. Group by external libraries, then internal modules. Sort alphabetically.
- **Formatting**: Use Prettier with 2-space indentation. Max line length 80 chars.
- **Types**: Use TypeScript for type safety. Define interfaces for data structures.
- **Naming Conventions**: camelCase for variables/functions, PascalCase for classes/components, UPPER_SNAKE_CASE for constants.
- **Error Handling**: Use try-catch blocks. Log errors with context. Avoid silent failures.
- **Comments**: Add JSDoc for functions. Keep concise.
- **Database**: Use parameterized queries to prevent SQL injection. Validate inputs.
- **Security**: Never hardcode secrets. Use environment variables.
- **Commits**: Use conventional commits (feat:, fix:, etc.).

## Cursor Rules
None found in .cursor/rules/ or .cursorrules.

## Copilot Rules
None found in .github/copilot-instructions.md.