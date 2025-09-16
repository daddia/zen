---
status: Accepted
date: 2025-09-12
decision-makers: Development Team, Architecture Team
consulted: DevOps Team, Security Team
informed: Product Team, Contributors
---

# ADR-0003 - Project Structure and Organization

## Context and Problem Statement

The Zen CLI project requires a well-organized directory structure that supports maintainability, testability, security, and future extensibility. The structure must follow Go best practices while accommodating the specific needs of a complex CLI application with multiple components, plugins, and integrations.

Key requirements:
- Clear separation between public APIs and internal implementation
- Support for plugin architecture and extensibility
- Organized test structure with different test types
- Security through private package isolation
- Developer-friendly navigation and understanding
- CI/CD pipeline integration
- Documentation and configuration organization

## Decision Drivers

* **Go Best Practices**: Follow established Go project layout conventions
* **Security**: Isolate internal implementation from public APIs
* **Maintainability**: Clear component boundaries and responsibilities
* **Testability**: Organized test structure supporting different test types
* **Extensibility**: Plugin system and future component additions
* **Developer Experience**: Intuitive navigation and clear module boundaries
* **CI/CD Integration**: Support for automated build and deployment processes
* **Documentation**: Comprehensive documentation organization

## Considered Options

* **Standard Go Project Layout** - Following golang-standards/project-layout
* **Flat Structure** - All packages in root directory
* **Domain-Driven Structure** - Organized by business domains
* **Layer-Based Structure** - Organized by technical layers
* **Microservice Structure** - Separate repositories per component

## Decision Outcome

Chosen option: **Standard Go Project Layout** with domain-specific adaptations, because it provides the best balance of Go conventions, security, and maintainability for a complex CLI application.

### Consequences

**Good:**
- Follows established Go community standards and best practices
- Clear separation between public (`pkg/`) and private (`internal/`) APIs
- Organized structure supports team collaboration and onboarding
- Plugin system naturally fits into the layout
- CI/CD pipelines can easily target specific components
- Security through package visibility controls
- Scalable structure supporting future growth

**Bad:**
- More complex than flat structure for simple components
- Requires discipline to maintain boundaries
- Some redundancy in directory nesting

**Neutral:**
- Opinionated structure may not fit all use cases
- Requires understanding of Go package visibility rules

### Confirmation

The decision has been validated through:
- Successful implementation of ZEN-001 with clean component boundaries
- Clear separation between CLI commands and business logic
- Plugin architecture foundation established
- Developer feedback on code navigation and understanding
- CI/CD pipeline successfully targeting specific components
- Security review confirming proper API isolation

## Enhanced Project Structure

```
zen/
├── cmd/                        # Main Applications
│   └── zen/                   # CLI Binary
│       └── main.go           # Ultra-lightweight entry point (delegates to zencmd)
├── internal/                   # Private Application Code
│   ├── zencmd/               # Command Orchestration
│   │   ├── cmd.go           # Main command handler with error management
│   │   └── cmd_test.go      # Command orchestration tests
│   ├── config/               # Configuration Management
│   │   ├── config.go        # Loading, validation, defaults
│   │   └── config_test.go   # Configuration tests
│   ├── logging/              # Logging Infrastructure
│   │   ├── logger.go        # Structured logging interface
│   │   └── logger_test.go   # Logging tests
│   ├── agents/               # AI Agent Orchestration (future)
│   ├── workflow/             # Workflow State Management (future)
│   ├── integrations/         # External System Clients (future)
│   ├── templates/            # Template Engine (future)
│   ├── quality/              # Quality Gates (future)
│   └── storage/              # Data Persistence (future)
├── pkg/                        # Public Library Code
│   ├── cmd/                  # Command Implementations
│   │   ├── factory/         # Dependency injection factory
│   │   │   ├── default.go   # Default factory implementation
│   │   │   └── default_test.go # Factory tests
│   │   ├── root/            # Root command
│   │   │   ├── root.go      # Root command implementation
│   │   │   └── root_test.go # Root command tests
│   │   ├── version/         # Version command
│   │   │   ├── version.go   # Version command implementation
│   │   │   └── version_test.go # Version tests
│   │   ├── init/            # Initialization command
│   │   │   ├── init.go      # Init command implementation
│   │   │   └── init_test.go # Init tests
│   │   ├── config/          # Configuration command
│   │   │   ├── config.go    # Config command implementation
│   │   │   └── config_test.go # Config tests
│   │   └── status/          # Status command
│   │       ├── status.go    # Status command implementation
│   │       └── status_test.go # Status tests
│   ├── cmdutil/             # Command utilities
│   │   ├── factory.go       # Factory interface and types
│   │   ├── factory_test.go  # Factory tests
│   │   ├── errors.go        # Error types and exit codes
│   │   └── errors_test.go   # Error tests
│   ├── iostreams/           # I/O abstraction
│   │   ├── iostreams.go     # I/O streams management
│   │   └── iostreams_test.go # I/O tests
│   ├── types/               # Common Type Definitions
│   │   ├── common.go        # Shared types and constants
│   │   └── common_test.go   # Type tests
│   └── errors/              # Error Handling
│       ├── errors.go        # Error types and utilities
│       └── errors_test.go   # Error tests
├── plugins/                    # Plugin System (future)
│   ├── agents/               # Custom AI Agents
│   ├── integrations/         # External Integrations
│   └── templates/            # Template Extensions
├── configs/                    # Configuration Files
│   ├── examples/            # Example configurations
│   │   └── zen.example.yaml # Example configuration
│   └── schemas/             # Configuration schemas
├── docs/                       # Documentation
│   ├── architecture/         # Architecture Documentation
│   │   ├── decisions/       # Architecture Decision Records
│   │   └── overview.md      # Architecture overview
│   ├── cli-structure.md      # Project structure guide
│   └── roadmap.md            # Development roadmap
├── script/                   # Build and Development Scripts
├── test/                     # Test Data
│   ├── fixtures/             # Test fixtures
│   ├── integration/          # Integration tests
│   └── e2e/                  # End-to-end tests
├── .github/                    # GitHub-specific Files
│   └── workflows/            # CI/CD Workflows
├── build/                      # Build Artifacts
├── tools/                      # Development Tools
├── Makefile                    # Build Automation
├── Dockerfile                  # Container Definition
├── go.mod                      # Go Module Definition
├── go.sum                      # Go Module Checksums
└── README.md                   # Project Documentation
```

## Component Organization Principles

### 🎯 **`cmd/` Directory**
- Contains main applications and their entry points
- Ultra-lightweight entry point (main.go reduced to ~10 lines)
- All logic delegated to `internal/zencmd/` for orchestration
- Build-time information injected at compile time

### 🔒 **`internal/` Directory**
- Private application code not importable by external packages
- `zencmd/` package handles command orchestration and error management
- Core business logic and implementation details
- Organized by functional domains (config, logging, agents, etc.)
- Security boundary preventing external access

### 📦 **`pkg/` Directory**
- Public library code that can be imported by external applications
- `pkg/cmd/` contains all command implementations with factory pattern
- `pkg/cmdutil/` provides factory interface and command utilities
- `pkg/iostreams/` provides I/O abstraction layer
- Stable APIs with backward compatibility considerations
- Well-documented public interfaces

### 🔌 **`plugins/` Directory**
- Plugin system for extensibility
- Organized by plugin types (agents, integrations, templates)
- Isolated from core application code
- Dynamic loading and registration support

### 📚 **`docs/` Directory**
- Comprehensive project documentation
- Architecture decisions and technical specifications
- User guides and API documentation
- Organized by audience (users, developers, operators)

## Pros and Cons of the Options

### Standard Go Project Layout

**Good:**
- Follows established Go community conventions
- Clear separation of public and private APIs
- Security through package visibility
- Scalable structure for complex applications
- CI/CD pipeline integration
- Plugin system support
- Developer familiarity

**Bad:**
- More complex than simple flat structure
- Requires understanding of Go package rules
- Some directory nesting overhead

**Neutral:**
- Opinionated about organization
- May be overkill for very simple projects

### Flat Structure

**Good:**
- Simple and easy to understand
- Minimal directory nesting
- Fast navigation for small projects

**Bad:**
- No API visibility controls
- Poor scalability for complex projects
- Mixing of concerns
- No plugin system support
- Security concerns with exposed internals

**Neutral:**
- Suitable only for simple applications
- Requires external documentation for organization

### Domain-Driven Structure

**Good:**
- Organized by business domains
- Clear feature boundaries
- Good for large teams

**Bad:**
- May not align with Go package conventions
- Cross-cutting concerns difficult to organize
- Potential for circular dependencies

**Neutral:**
- Requires deep domain understanding
- May change as business evolves

### Layer-Based Structure

**Good:**
- Clear technical separation
- Easy to understand for developers
- Good for traditional architectures

**Bad:**
- Can lead to anemic domain models
- Cross-layer dependencies
- Not idiomatic for Go
- Difficult to maintain boundaries

**Neutral:**
- Familiar to developers from other languages
- May not leverage Go's strengths

## More Information

**Implementation Guidelines:**
- Each `internal/` package should have a single, well-defined responsibility
- Public APIs in `pkg/` must maintain backward compatibility
- Tests should be co-located with the code they test
- Plugin interfaces should be defined in `pkg/` for external use
- Configuration and documentation should be easily discoverable

**Security Considerations:**
- Internal packages are not accessible to external importers
- Sensitive logic is protected by package boundaries
- Plugin system provides controlled extension points
- Configuration files exclude sensitive defaults

**Related ADRs:**
- ADR-0001: Go Language Choice
- ADR-0002: Cobra CLI Framework Selection
- ADR-0004: Configuration Management Strategy

**References:**
- [Standard Go Project Layout](https://github.com/golang-standards/project-layout)
- [Go Package Organization](https://go.dev/doc/code)
- [Effective Go](https://go.dev/doc/effective_go)
- [Go Project Structure Best Practices](https://peter.bourgon.org/go-best-practices-2016/)
