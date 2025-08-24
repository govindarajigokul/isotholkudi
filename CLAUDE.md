# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is **DSpace Angular** - the user interface for DSpace, an open source repository application for digital content. Built with Angular 17, TypeScript, and Angular Universal for server-side rendering. The application provides a frontend for the DSpace REST API backend.

## Essential Commands

### Development
```bash
# Install dependencies
yarn install

# Start development server (watches for changes, hot reload)
yarn start:dev

# Start production build and server
yarn start

# Build for production
yarn run build:prod

# Serve production build
yarn run serve:ssr
```

### Testing
```bash
# Run unit tests
yarn test

# Run unit tests in watch mode
yarn test:watch

# Run unit tests with code coverage
yarn test:headless

# Run e2e tests (requires backend running locally)
ng e2e
```

### Code Quality
```bash
# Lint the codebase
yarn lint

# Lint with auto-fix
yarn lint-fix

# Build linting tools first (required before running lint)
yarn build:lint
```

### Maintenance
```bash
# Clean all generated files and node_modules
yarn clean

# Clean production build files only
yarn clean:prod

# Clean dist directory
yarn clean:dist
```

## Architecture & Key Components

### Application Structure

The application follows Angular's modular architecture with lazy-loaded themes and components:

- **`/src/app/`** - Core application modules organized by feature
  - `core/` - Core services including auth, data services, cache, and REST client
  - `shared/` - Shared components and utilities used across the application
  - `*-page/` - Feature page modules (community, collection, item, search, etc.)
  
- **`/src/themes/`** - Theme system for customizable UI
  - `dspace/` - Default theme
  - `custom/` - Template for custom themes
  - Each theme contains overrides for components

- **`/config/`** - Runtime configuration
  - Configuration can be overridden via environment variables or config files
  - See `config.example.yml` for configuration template

### Data Flow Architecture

1. **Data Services** (`/src/app/core/data/`)
   - Services for each entity type (Item, Collection, Community, etc.)
   - All extend from base data services with caching and request handling
   - Use RxJS observables for reactive data flow

2. **NgRx Store** 
   - State management for auth, cache, requests, and UI state
   - Located in various `.reducer.ts` and `.effects.ts` files
   - Selectors in `selectors.ts` files

3. **REST Communication**
   - `DSpaceRESTv2Service` handles all API communication
   - Request/response interceptors for auth, locale, and logging
   - Response parsing services for different content types

### Theme System

The application supports multiple themes through Angular's component inheritance:
- Base components in `/src/app/`
- Theme overrides in `/src/themes/[theme-name]/`
- Components use `themed-*.component.ts` wrappers for theme switching
- Lazy loading of theme modules for performance

### Key Services

- **AuthService** - Authentication and authorization
- **ObjectCacheService** - Client-side caching of API responses
- **RequestService** - HTTP request queuing and management
- **ConfigurationDataService** - Application configuration management
- **LocaleService** - Internationalization support

## Configuration

### Environment Variables
Use prefix `DSPACE_` for all environment variables:
- `DSPACE_REST_HOST` - REST API host
- `DSPACE_REST_PORT` - REST API port  
- `DSPACE_REST_SSL` - Use HTTPS for REST API
- `DSPACE_HOST` - UI host
- `DSPACE_PORT` - UI port

### Config Files
- Development: `config/config.dev.yml`
- Production: `config/config.prod.yml`

Priority: Environment variables > `.env` file > config files

## Testing Requirements

### E2E Testing Setup
1. Backend must be running locally (not demo server)
2. Backend must include Entities Test Data set
3. Configure REST API connection in config files

### Unit Testing
- Tests located alongside components (`*.spec.ts`)
- Uses Jasmine/Karma
- Mock services provided in test files

## Development Notes

- Node.js v18.x or v20.x required
- Yarn v1.x for dependency management (yarn.lock file present)
- TypeScript strict mode enabled
- Angular Universal for SSR - be aware of browser-only APIs
- Custom ESLint rules in `/lint/` directory
- Webpack custom configuration in `/webpack/`

## Common Tasks

### Adding a New Component
1. Create component in appropriate module
2. If themeable, create `themed-*.component.ts` wrapper
3. Add to module declarations
4. Override in themes if needed

### Modifying REST API Integration
1. Update or create data service in `/src/app/core/data/`
2. Add response parsing service if new response type
3. Update models in `/src/app/core/shared/`

### Working with Themes
1. Copy component to theme directory maintaining path structure
2. Extend base component class
3. Override template/styles as needed
4. Register in theme's module

## Important Files

- `angular.json` - Angular CLI configuration
- `server.ts` - Express server for SSR
- `webpack/*.ts` - Webpack configurations
- `src/app/app-routes.ts` - Main application routes
- `src/config/default-app-config.ts` - Default configuration values