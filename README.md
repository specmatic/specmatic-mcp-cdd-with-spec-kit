# Spec Driven Development with GitHub Spec Kit and Specmatic 

This project demonstrates **contract-first development** using **[spec-kit](https://github.com/github/spec-kit)** where OpenAPI specifications evolve organically through feature development. Each feature analyzes existing contracts for reuse before extending the API, using **Specmatic MCP** as intelligent guardrails throughout the process. The focus is on two key aspects:
1. **Going Beyond the First Feature** – showing how the system can evolve without regression or drift, especially critical when using coding agents that optimize for speed but may introduce breaking changes.
2. **Parallel Execution with Subagents** – enabling backend and frontend to develop independently once the API contract is in place, mirroring real-world Contract-Driven Development.

## 🚀 Quick Start

```bash
# Configure MCP servers
claude mcp add specmatic npx "specmatic-mcp"
claude mcp add playwright npx "@playwright/mcp@latest"

# Open project
claude
```

### Feature 1: Product Listing Page

**Try the spec-kit workflow:**
```
/specify product listing page
- each product has name, description, price and category, all of these properties are mandatory
- category can be only "food", "gadget", "book" or "other"
- default sort order is alphabetical by name
- users should be able to filter by category
- no pagination required, just show all available products as per category filter
```

```
/plan
```

Observe the OpenAPI specification that gets created as part of `/plan` and this serves as the API Contract between Frontend and Backend.

```
/tasks
```

```
implement the tasks and update the task list as you complete them
```

Observe the Frontend and Backend being built in parallel using [frontend-react-engineer](.claude/agents/frontend-react-engineer.md) and [backend-api-engineer](.claude/agents/backend-api-engineer.md) respectively. This is possible because we have OpenAPI spec as the API Contract and both these agents are able to use Specmatic MCP to help them build the components independently.

### Feature 2: Product Display Page

With Feature 1 complete, we merge the branch (001-product-listing-page) into main and move to the build other features.

```
/specify product display page
- Users can navigate from product listing page to product display page
- Or they directly launch the product display page using URL with product id
There's no need to handle error conditions. Focus on happy path.
```

```
/plan
```

**IMPORTANT**: Observe how the [openapi-spec-author](.claude/agents/openapi-spec-author.md) using Specmatic MCP to run **backward compatibility check** to make sure there are no breaking changes during API Design stage itself so that we can prevent such issues from being detected much later after code changes have been made in the process avoiding rework.

```
/tasks
```

```
implement the tasks and update the task list as you complete them
```

Again the Frontend and Backend being built in parallel which helps us save a significant amount of time.

## 📦 Specmatic MCP Resources

- 📦 **NPM Package**: [specmatic-mcp](https://www.npmjs.com/package/specmatic-mcp) - Easy NPM installation
- 📂 **Source Code**: [specmatic/specmatic-mcp-server](https://github.com/specmatic/specmatic-mcp-server) - GitHub repository

> **Note:** This project uses Claude Code for demo purposes, however you can use any coding agent of your choice and make necessary changes accordingly.

## 🎯 What This Template Demonstrates

This template shows **API Design First methodology** in action. Starting from **zero** (no pre-existing contracts), observe how:

- **Contract-first development** works using the spec-kit workflow: `/specify` → `/plan` → `/tasks` → implement
- OpenAPI specifications **evolve organically** through feature development
- Existing API operations are **analyzed for reuse** before extending contracts
- Teams can work **independently** after agreeing on the contract
- **Specmatic MCP** provides intelligent guardrails throughout the process
- Frontend and backend stay **synchronized** with the evolving API specification

### ✅ Prerequisites

**Install Claude Code** (if not already installed):
Follow installation instructions at [https://docs.anthropic.com/claude/docs/claude-code](https://docs.anthropic.com/claude/docs/claude-code)

### 🔄 Optional Reset
**Reset the project to try again** (optional - Claude Code command available):
```
/reset-sample-project
```


## 📁 Project Structure

```
specmatic-mcp-sample-with-spec-kit/
├── api_spec.yaml         # OpenAPI specification (evolves with each feature)
├── specs/               # Feature specifications and plans
│   ├── 001-product-listing/
│   │   ├── spec.md     # Feature specification
│   │   ├── plan.md     # Implementation plan
│   │   └── tasks.md    # Generated tasks
│   └── 002-next-feature/
├── backend/             # Node.js/Express API (created during Backend Phase)
│   ├── src/
│   │   ├── models/     # Data models
│   │   ├── services/   # Business logic
│   │   ├── cli/        # CLI utilities
│   │   └── routes/     # API endpoints
│   └── package.json
├── frontend/           # React application (created during Frontend Phase)
│   ├── src/
│   │   ├── components/ # React components
│   │   ├── services/   # API integration
│   │   └── App.js
│   └── package.json
├── memory/             # Project constitution and guidelines
│   └── constitution.md
├── templates/          # Spec-kit templates for feature development
│   ├── spec-template.md
│   ├── plan-template.md
│   ├── tasks-template.md
│   └── agent-file-template.md
├── .claude/           # Spec-kit commands and specialized agents
│   ├── commands/      # /specify, /plan, /tasks commands
│   └── agents/        # Specialized development agents
│       ├── backend-api-engineer.md
│       ├── frontend-react-engineer.md
│       ├── integration-tester.md
│       └── openapi-spec-author.md
└── scripts/           # Utility scripts for project management
```

## 🔄 Three-Phase Parallel Development

This project demonstrates true parallel development where frontend and backend teams can work independently after contract agreement:

### Phase 1: Contract Design
- Use `/specify` to define feature requirements
- Use `/plan` to analyze existing contracts and design new endpoints
- OpenAPI specification evolves organically through feature analysis

### Phase 2: Parallel Implementation (Backend + Frontend)

**Backend Phase** (Contract-Driven TDD):
```
Contract Tests (RED) → Backend Implementation (GREEN) → Resiliency Tests → Integration
```
- **backend-api-engineer** drives RED-GREEN-REFACTOR cycle
- Contract tests MUST fail before any implementation exists
- JUnit test reports guide all implementation decisions
- Resiliency tests validate error handling and boundary conditions

**Frontend Phase** (Mock-Driven Development):
```
Mock Server Setup → Component Development → Happy Path Verification
```
- **frontend-react-engineer** builds UI against Specmatic mock server (port 9001)
- No waiting for backend - immediate development with realistic API responses
- Playwright MCP used for basic functional verification

### Phase 3: Integration Verification
- **integration-tester** shuts down mocks and verifies real systems integration
- Frontend reconfigured to connect to real backend (port 3000)
- End-to-end workflow validation with actual API responses

### Key Parallel Development Benefits:
- **No Coordination Overhead**: Contract serves as the agreement between teams
- **Independent Velocity**: Frontend and backend proceed at their own pace
- **Early Issue Detection**: Contract tests catch integration problems immediately
- **Realistic Development**: Mock server provides contract-compliant responses

## 🎯 Key Benefits Demonstrated

### Contract-First Development
- OpenAPI specifications evolve organically through feature development
- Each feature analyzes existing contracts for reuse before extending
- Both frontend and backend are built to conform to the evolving contract
- Smart contract analysis prevents API bloat and promotes reuse

### True Test-Driven Development
- Contract tests MUST fail before any backend implementation exists
- Backend routes/endpoints implemented only to make failing tests pass
- Resiliency tests MUST fail before input validation is added
- Follows strict RED-GREEN-REFACTOR cycle throughout

### Independent Development
- Backend and frontend developed in parallel after contract agreement
- Frontend developed using Specmatic's mock server (port 9001) during parallel development
- No coordination needed - contract serves as the agreement between teams

### Automatic Validation
- Specmatic MCP automatically validates backend implementations
- Contract tests ensure API compliance
- Resiliency tests verify error handling and boundary conditions

### Zero Dependencies
- Specmatic runs as an MCP server through NPX
- No need to add Specmatic dependencies to your project
- Clean project structure with minimal tooling overhead

## 🔧 How It Works

1. **Spec-Kit Workflow**: Features developed through `specify` → `plan` → `tasks` → `implement` cycles

2. **Smart Contract Evolution**:
   - First feature creates initial OpenAPI specification at repository root
   - Subsequent features analyze existing contracts for reuse opportunities
   - Only extends API when existing operations cannot support new features
   - Prevents API bloat through intelligent contract analysis

3. **Specmatic MCP Integration**:
   - Provides contract testing capabilities for evolving specifications
   - Offers mock server functionality for isolated frontend development
   - Validates implementations against the current contract state
   - Runs resiliency tests for error scenarios

4. **Constitutional Governance**:
   - All development follows constitutional principles in `/memory/constitution.md` (v1.1.0)
   - This sample project serves as an experiment demonstrating API Design First approach using Spec-Kit
   - The constitution embodies parallel development workflow and contract-first discipline
   - Ensures consistent, high-quality feature development throughout the process

## 🏗️ Architecture Diagrams

### Production Setup
```
┌─────────────────┐                                   ┌─────────────────┐
│                 │ ────── HTTP Requests ───────────► │                 │
│    Frontend     │                                   │    Backend      │
│   (React App)   │        ┌──────────────────┐       │ (Express API)   │
│                 │        │  api_spec.yaml   │       │                 │
│   Port: 4000    │        │ (Evolved OpenAPI)│       │   Port: 3000    │
│                 │        └──────────────────┘       │                 │
|                 |                                   |                 |
│                 │ ◄────── HTTP Responses ────────── │                 │
└─────────────────┘                                   └─────────────────┘
```

### Development Setup - Frontend
```
┌─────────────────┐                     ┌─────────────────┐
│                 │                     │                 │
│    Frontend     │     Mock Requests   │ Specmatic Mock  │
│   (React App)   │ ◄─────────────────► │    Server       │
│                 │                     │                 │
│   Port: 4000    │                     │   Port: 9001    │
└─────────────────┘                     └─────────┬───────┘
                                                  │
                                                  │ Based on
                                                  │
                                                  ▼
                                     ┌─────────────────────┐
                                     │                     │
                                     │   api_spec.yaml     │
                                     │  (Evolved OpenAPI)  │
                                     │                     │
                                     └─────────────────────┘
```

### Development Setup - Backend
```
┌─────────────────┐                       ┌─────────────────┐
│                 │                       │                 │
│ Specmatic MCP   │ ───► Contract   ───►  │    Backend      │
│    Tools        │      Testing          │ (Express API)   │
│                 │                       │                 │
│ (via NPX)       │ ───► Resiliency ───►  │   Port: 3000    │
└─────────┬───────┘      Testing          └─────────────────┘
          │
          │ Validates against
          │
          ▼
┌─────────────────────┐
│                     │
│   api_spec.yaml     │
│  (Evolved OpenAPI)  │
│                     │
└─────────────────────┘
```

## 🎨 Contract Evolution Example

As features are developed, the API evolves organically:

**Feature 1** (`001-product-listing`):
- Creates initial `api_spec.yaml` with GET /products endpoint
- Includes product filtering by type (book, food, gadget, other)

**Feature 2** (`002-product-creation`):
- Analyzes existing contract → No POST endpoint exists
- Extends `api_spec.yaml` with POST /products and validation schemas

**Feature 3** (`003-product-search`):
- Analyzes existing contract → GET /products with query params might suffice
- **Reuses existing endpoint** instead of creating new ones

The complete API specification emerges through this thoughtful, feature-driven evolution.

## 🏗️ API Design First Workflow with Specialized Agents

### Phase 1: Specification & Planning
1. **Feature Specification**: Use `/specify` to create feature requirements and user stories
2. **Contract Analysis**: Use `/plan` with **openapi-spec-author** to analyze existing contracts and extend only if needed
3. **Task Generation**: Use `/tasks` to break down implementation into concrete, parallel-executable steps

### Phase 2: Parallel Implementation (Run Simultaneously)

**Backend Track** (Contract-Driven TDD with **backend-api-engineer**):
```
implement T001-T010   # Backend tasks executed by specialized agent
```
- **RED**: Contract tests MUST FAIL (no routes/endpoints exist yet)
- **GREEN**: Implement just enough backend code to make contract tests pass
- **RED**: Resiliency tests MUST FAIL (no input validation exists yet)
- **GREEN**: Add input validation to make resiliency tests pass
- **REFACTOR**: Clean up implementation while keeping tests green

**Frontend Track** (Mock-Driven Development with **frontend-react-engineer**):
```
implement T023-T033   # Frontend tasks executed in parallel
```
- Start Specmatic mock server (port 9001) for immediate development
- Build React components against realistic API responses
- Use Playwright MCP for basic functional verification
- No waiting for backend - full development velocity

### Phase 3: Integration Verification
**Integration Track** (Real System Verification with **integration-tester**):
```
implement T034-T040   # Integration tasks after both phases complete
```
- Shutdown all mock servers to prevent interference
- Reconfigure frontend to connect to real backend (port 3000)
- Verify end-to-end workflows with actual API integration
- Validate complete system functionality

### Agent Coordination Benefits:
- **Specialized Expertise**: Each agent optimized for their specific domain
- **Parallel Execution**: Backend and frontend proceed independently
- **Quality Gates**: Integration only begins after both phases complete
- **Seamless Handoffs**: Agents coordinate through shared contract specification

## 📚 Learn More

- [Spec-Kit](https://github.com/github/spec-kit) - Methodology for API Design First development
- [Specmatic MCP Server](https://github.com/specmatic/specmatic-mcp-server) - MCP server for contract testing and service virtualization

---

**Ready to see spec-kit contract evolution in action? Just ask Claude to build your first feature!**