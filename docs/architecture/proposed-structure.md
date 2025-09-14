# Zen CLI: Proposed Project Structure

## Root Directory Structure

```
zen/
├── .github/                    # GitHub workflows and templates
├── .vscode/                    # VS Code workspace configuration
├── build/                      # Build artifacts and packaging
├── cmd/                        # CLI entry points and commands
├── docs/                       # Documentation
├── examples/                   # Usage examples and tutorials
├── internal/                   # Private Go packages
├── pkg/                        # Public Go packages (APIs for plugins)
├── plugins/                    # Official plugins
├── scripts/                    # Build, development, and utility scripts
├── templates/                  # Built-in templates (migrated from existing)
├── test/                       # Integration and end-to-end tests
├── tools/                      # Development tools and generators
├── web/                        # Web UI assets (if needed)
├── .gitignore
├── .golangci.yml              # Linting configuration
├── CHANGELOG.md
├── CODE_OF_CONDUCT.md
├── CONTRIBUTING.md
├── Dockerfile
├── LICENSE
├── Makefile
├── README.md
├── go.mod
├── go.sum
└── goreleaser.yml             # Release automation
```

## 🎯 **Detailed Structure Breakdown**

### **1. Command Layer (`cmd/`)**

```
cmd/
├── zen/                       # Main CLI binary
│   ├── main.go               # Entry point
│   └── version.go            # Version information
├── plugins/                   # Plugin binaries (if separate)
│   ├── agent-custom/
│   └── integration-slack/
└── tools/                     # Development tools
    ├── template-validator/
    ├── prompt-optimizer/
    └── config-migrator/
```

### **2. Internal Packages (`internal/`)**

```
internal/
├── cli/                       # CLI command implementations
│   ├── root.go               # Root command setup
│   ├── global/               # Global commands
│   │   ├── init.go
│   │   ├── config.go
│   │   ├── status.go
│   │   └── version.go
│   ├── product/              # Product management commands
│   │   ├── strategy.go       # Product strategy and roadmapping
│   │   ├── research.go       # Market research and user insights
│   │   ├── requirements.go   # Requirements and user story management
│   │   ├── analytics.go      # Product analytics and metrics
│   │   └── validation.go     # Product validation and testing
│   ├── workflow/             # 12-stage engineering workflow commands
│   │   ├── discover.go       # Stage 01: Requirements analysis
│   │   ├── prioritize.go     # Stage 02: Backlog prioritization
│   │   ├── design.go         # Stage 03: Technical design
│   │   ├── architect.go      # Stage 04: Architecture review
│   │   ├── plan.go           # Stage 05: Implementation planning
│   │   ├── build.go          # Stage 06: Code generation and development
│   │   ├── review.go         # Stage 07: Code review and quality
│   │   ├── test.go           # Stage 08: Testing and QA
│   │   ├── secure.go         # Stage 09: Security and compliance
│   │   ├── release.go        # Stage 10: Deployment and release
│   │   ├── verify.go         # Stage 11: Post-deployment verification
│   │   └── feedback.go       # Stage 12: Analytics and feedback
│   ├── integrations/         # External system commands
│   │   ├── jira.go           # Jira project management
│   │   ├── confluence.go     # Documentation publishing
│   │   ├── git.go            # Version control
│   │   ├── ci.go             # CI/CD systems
│   │   ├── analytics.go      # Analytics platforms (GA, Mixpanel)
│   │   ├── design.go         # Design tools (Figma, Sketch)
│   │   └── communication.go  # Slack, Teams, Discord
│   └── utilities/            # Utility commands
│       ├── template.go
│       ├── agent.go
│       └── workflow.go
├── agents/                    # AI agent orchestration
│   ├── client.go             # Multi-provider LLM client
│   ├── manager.go            # Agent lifecycle management
│   ├── registry.go           # Agent registry and discovery
│   ├── context.go            # Conversation context management
│   ├── cost.go               # Cost tracking and optimization
│   ├── providers/            # LLM provider implementations
│   │   ├── openai.go
│   │   ├── anthropic.go
│   │   ├── azure.go
│   │   └── local.go          # Local model support
│   └── prompts/              # Prompt management
│       ├── loader.go         # Template loading
│       ├── renderer.go       # Template rendering
│       ├── optimizer.go      # Prompt optimization
│       └── cache.go          # Prompt caching
├── config/                    # Configuration management
│   ├── config.go             # Main configuration struct
│   ├── loader.go             # Configuration loading
│   ├── validator.go          # Configuration validation
│   ├── workspace.go          # Workspace detection and setup
│   ├── integrations.go       # External system configuration
│   ├── agents.go             # AI agent configuration
│   └── migration.go          # Configuration migration
├── integrations/             # External system clients
│   ├── product/              # Product management integrations
│   │   ├── analytics/        # Analytics platforms
│   │   │   ├── google.go     # Google Analytics
│   │   │   ├── mixpanel.go   # Mixpanel
│   │   │   ├── amplitude.go  # Amplitude
│   │   │   └── segment.go    # Segment
│   │   ├── design/           # Design tool integrations
│   │   │   ├── figma.go      # Figma API
│   │   │   ├── sketch.go     # Sketch integration
│   │   │   └── invision.go   # InVision
│   │   ├── research/         # User research tools
│   │   │   ├── surveys.go    # Survey platforms
│   │   │   ├── feedback.go   # Feedback collection
│   │   │   └── usertesting.go # User testing platforms
│   │   └── crm/              # CRM integrations
│   │       ├── salesforce.go # Salesforce
│   │       ├── hubspot.go    # HubSpot
│   │       └── pipedrive.go  # Pipedrive
│   ├── engineering/          # Engineering platform integrations
│   │   ├── jira/             # Jira integration
│   │   │   ├── client.go     # API client
│   │   │   ├── types.go      # Data types
│   │   │   ├── operations.go # CRUD operations
│   │   │   ├── workflow.go   # Workflow automation
│   │   │   └── sync.go       # Synchronization
│   │   ├── confluence/       # Confluence integration
│   │   │   ├── client.go
│   │   │   ├── publisher.go  # Document publishing
│   │   │   ├── layout.go     # Layout management
│   │   │   └── converter.go  # Markdown conversion
│   │   ├── git/              # Git integration
│   │   │   ├── client.go
│   │   │   ├── hooks.go      # Git hooks
│   │   │   ├── workflow.go   # Git workflow automation
│   │   │   └── analysis.go   # Repository analysis
│   │   └── ci/               # CI/CD integrations
│   │       ├── github.go     # GitHub Actions
│   │       ├── gitlab.go     # GitLab CI
│   │       ├── jenkins.go    # Jenkins
│   │       └── generic.go    # Generic CI/CD
│   ├── communication/        # Communication platform integrations
│   │   ├── slack.go          # Slack integration
│   │   ├── teams.go          # Microsoft Teams
│   │   ├── discord.go        # Discord
│   │   └── email.go          # Email notifications
│   └── common/               # Common integration utilities
│       ├── auth.go           # Authentication
│       ├── retry.go          # Retry logic
│       ├── cache.go          # Response caching
│       └── rate_limit.go     # Rate limiting
├── quality/                   # Quality gates and enforcement
│   ├── gates.go              # Quality gate definitions
│   ├── enforcement.go        # Automated enforcement
│   ├── metrics.go            # Quality metrics collection
│   ├── rules/                # Quality rules
│   │   ├── code.go           # Code quality rules
│   │   ├── security.go       # Security rules
│   │   ├── performance.go    # Performance rules
│   │   └── documentation.go  # Documentation rules
│   └── reporters/            # Quality reporters
│       ├── console.go
│       ├── json.go
│       └── html.go
├── templates/                 # Template engine and management
│   ├── engine.go             # Template rendering engine
│   ├── registry.go           # Template registry and discovery
│   ├── validator.go          # Template validation
│   ├── functions.go          # Custom template functions
│   ├── loader.go             # Template loading
│   ├── cache.go              # Template caching
│   └── product.go            # Product-specific template functions
├── workflow/                  # Workflow state management
│   ├── state.go              # Workflow state tracking
│   ├── orchestrator.go       # Multi-stage orchestration
│   ├── hooks.go              # Pre/post stage hooks
│   ├── persistence.go        # State persistence
│   ├── visualization.go      # Workflow visualization
│   └── recovery.go           # Error recovery
├── storage/                   # Data storage layer
│   ├── sqlite.go             # SQLite implementation
│   ├── postgres.go           # PostgreSQL implementation
│   ├── memory.go             # In-memory storage
│   ├── migrations/           # Database migrations
│   └── models/               # Data models
├── logging/                   # Structured logging
│   ├── logger.go             # Main logger
│   ├── formatters.go         # Log formatters
│   ├── levels.go             # Log levels
│   └── outputs.go            # Output destinations
├── security/                  # Security utilities
│   ├── encryption.go         # Encryption/decryption
│   ├── secrets.go            # Secret management
│   ├── auth.go               # Authentication
│   └── audit.go              # Audit logging
└── utils/                     # Common utilities
    ├── files.go              # File operations
    ├── http.go               # HTTP utilities
    ├── json.go               # JSON utilities
    ├── strings.go            # String utilities
    └── validation.go         # Input validation
```

### **3. Public APIs (`pkg/`)**

```
pkg/
├── client/                    # Go client library
│   ├── client.go             # Main client
│   ├── workflow.go           # Workflow client
│   ├── agents.go             # Agent client
│   └── types.go              # Public types
├── plugin/                    # Plugin SDK
│   ├── sdk.go                # Plugin SDK interface
│   ├── agent.go              # Agent plugin interface
│   ├── integration.go        # Integration plugin interface
│   ├── template.go           # Template plugin interface
│   └── examples/             # Plugin examples
├── types/                     # Shared types and interfaces
│   ├── workflow.go           # Workflow types
│   ├── agent.go              # Agent types
│   ├── integration.go        # Integration types
│   ├── template.go           # Template types
│   ├── quality.go            # Quality types
│   └── common.go             # Common types
└── errors/                    # Error types and handling
    ├── errors.go             # Error definitions
    ├── codes.go              # Error codes
    └── handlers.go           # Error handlers
```

### **4. Plugin System (`plugins/`)**

```
plugins/
├── agents/                    # Custom agent plugins
│   ├── domain-expert/        # Domain-specific agents
│   │   ├── main.go
│   │   ├── plugin.yaml
│   │   └── README.md
│   ├── code-reviewer/        # Specialized code reviewers
│   └── security-scanner/     # Security-focused agents
├── integrations/             # Integration plugins
│   ├── slack/                # Slack integration
│   ├── teams/                # Microsoft Teams
│   ├── datadog/              # Datadog monitoring
│   └── pagerduty/            # PagerDuty alerting
├── templates/                # Template plugins
│   ├── enterprise/           # Enterprise templates
│   ├── microservices/        # Microservice templates
│   └── mobile/               # Mobile app templates
└── quality/                  # Quality gate plugins
    ├── sonarqube/            # SonarQube integration
    ├── veracode/             # Veracode security
    └── lighthouse/           # Lighthouse performance
```

### **5. Templates (`templates/`)**

```
templates/
├── workflows/                 # Workflow templates
│   ├── story-definition.yaml # Migrated story template
│   ├── adr-template.yaml     # Architecture Decision Record
│   ├── feature-design.yaml   # Feature design document
│   ├── runbook.yaml          # Operational runbook
│   └── security-policy.yaml  # Security policy
├── prompts/                  # AI prompt templates (migrated)
│   ├── 01-discover/
│   │   ├── discover.yaml
│   │   ├── overview.yaml
│   │   └── define-story.yaml
│   ├── 02-prioritize/
│   │   └── prioritize.yaml
│   ├── 03-design/
│   │   ├── design.yaml
│   │   └── technical-design.yaml
│   ├── 04-architect/
│   │   ├── architect-review.yaml
│   │   ├── architect-adr.yaml
│   │   └── architect-solution.yaml
│   ├── 05-plan/
│   │   ├── plan-scaffold.yaml
│   │   └── sprint-planning.yaml
│   ├── 06-build/
│   │   ├── code-generation.yaml
│   │   ├── refactoring.yaml
│   │   └── performance-optimization.yaml
│   ├── 07-review/
│   │   └── code-review.yaml
│   ├── 08-test/
│   │   ├── qa-test.yaml
│   │   ├── qa-solution-review.yaml
│   │   └── unit-tests.yaml
│   ├── 09-secure/
│   │   └── security-compliance.yaml
│   ├── 10-release/
│   │   └── release-management.yaml
│   ├── 11-verify/
│   │   └── post-deploy-verification.yaml
│   ├── 12-feedback/
│   │   └── roadmap-feedback.yaml
│   └── documentation/
│       ├── doc-writer.yaml
│       └── knowledge-management.yaml
├── scaffolds/                # Project scaffolding templates
│   ├── go-service/
│   ├── react-app/
│   ├── python-api/
│   └── terraform-module/
```


### **6. Documentation (`docs/`)**

```
docs/
├── architecture/             # Architecture documentation
│   ├── overview.md
│   ├── decisions/            # ADRs
│   │   ├── 001-monorepo.md
│   │   ├── 002-go-choice.md
│   │   └── 003-plugin-system.md
│   └── diagrams/             # Architecture diagrams
├── user/                     # User documentation
│   ├── installation.md
│   ├── quick-start.md
│   ├── configuration.md
│   ├── workflows/            # Workflow guides
│   │   ├── discover.md
│   │   ├── design.md
│   │   └── ...
│   └── integrations/         # Integration guides
│       ├── jira.md
│       ├── confluence.md
│       └── git.md
├── developer/                # Developer documentation
│   ├── contributing.md
│   ├── plugin-development.md
│   ├── api-reference.md
│   └── testing.md
├── operations/               # Operations documentation
│   ├── deployment.md
│   ├── monitoring.md
│   ├── troubleshooting.md
│   └── security.md
└── examples/                 # Example usage
    ├── basic-workflow.md
    ├── enterprise-setup.md
    └── custom-agents.md
```

### **7. Testing (`test/`)**

```
test/
├── integration/              # Integration tests
│   ├── workflow_test.go
│   ├── agents_test.go
│   ├── jira_test.go
│   └── confluence_test.go
├── e2e/                     # End-to-end tests
│   ├── full_workflow_test.go
│   ├── plugin_system_test.go
│   └── migration_test.go
├── fixtures/                # Test fixtures
│   ├── configs/
│   ├── templates/
│   └── data/
├── mocks/                   # Mock implementations
│   ├── agent_mock.go
│   ├── jira_mock.go
│   └── git_mock.go
└── utils/                   # Test utilities
    ├── helpers.go
    ├── containers.go        # Testcontainers setup
    └── fixtures.go
```

### **8. Build & Deployment (`build/`)**

```
build/
├── ci/                      # CI/CD configurations
│   ├── github-actions/
│   ├── gitlab-ci/
│   └── jenkins/
├── docker/                  # Docker configurations
│   ├── Dockerfile
│   ├── Dockerfile.dev
│   └── docker-compose.yml
├── packages/                # Package configurations
│   ├── deb/                 # Debian packages
│   ├── rpm/                 # RPM packages
│   ├── msi/                 # Windows installer
│   └── homebrew/            # Homebrew formula
└── scripts/                 # Build scripts
    ├── build.sh
    ├── test.sh
    ├── release.sh
    └── package.sh
```

### **9. Development Tools (`tools/`)**

```
tools/
├── generators/              # Code generators
│   ├── plugin-scaffold/
│   ├── integration-client/
│   └── template-validator/
├── migrators/              # Migration tools
│   ├── config-migrator/
│   ├── template-migrator/
│   └── data-migrator/
└── dev-setup/              # Development setup
    ├── install-deps.sh
    ├── setup-env.sh
    └── pre-commit-hooks/
```

## 🏗️ **Enhanced Architecture with Factory Pattern**

The Zen CLI now implements an enhanced architecture inspired by industry best practices:

### **Key Architectural Changes**

1. **Ultra-Lightweight Entry Point**
   - `cmd/zen/main.go` reduced to ~10 lines
   - All logic delegated to `internal/zencmd/`

2. **Factory Pattern Implementation**
   - Dependency injection via `pkg/cmd/factory/`
   - Lazy initialization of components
   - Better testability and modularity

3. **Commands in Public API**
   - All commands moved from `internal/cli/` to `pkg/cmd/`
   - Each command in its own package
   - Consistent structure and testing

4. **Centralized Command Orchestration**
   - `internal/zencmd/` handles all orchestration
   - Unified error handling
   - Structured exit codes

5. **Enhanced I/O Abstraction**
   - `pkg/iostreams/` for I/O management
   - TTY detection and color support
   - Progress writer abstraction
