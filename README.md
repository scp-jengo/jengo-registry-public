# Jengo Registry Public

**Workspace Organization Schemas & Templates**

---

## Purpose

Open-source schemas and templates for organizing AI agent workspaces:
- Project configuration schemas
- Workspace structure templates
- Service registry patterns
- Repository organization schemas
- Task management integrations

**PUBLIC:** Shareable templates for the AI development community.

---

## Core Schemas

### Project Schema

**Standard project definition:**

```yaml
# project.schema.yaml

schema:
  id:
    type: string
    required: true
    pattern: "^[a-z0-9-]+$"
    description: "Unique project identifier (lowercase, alphanumeric, hyphens)"

  name:
    type: string
    required: true
    description: "Human-readable project name"

  type:
    type: enum
    required: true
    values:
      - framework      # Reusable library/framework
      - saas          # Software-as-a-Service application
      - wordpress     # WordPress site/plugin
      - library       # Code library/package
      - tool          # Utility/tool
      - service       # Background service/daemon
    description: "Project category"

  location:
    type: path
    required: true
    description: "Absolute path to project directory"

  repository:
    type: object
    properties:
      url:
        type: string
        pattern: "^https://github\\.com/[^/]+/[^/]+$"
        description: "GitHub repository URL"

      branch:
        type: string
        default: "main"
        description: "Default branch name"

  tech_stack:
    type: array
    items: string
    description: "Technologies used (e.g., .NET 9.0, React, PostgreSQL)"

  status:
    type: enum
    values: [active, maintenance, archived]
    default: active
    description: "Project lifecycle status"

  task_management:
    type: object
    properties:
      system:
        type: enum
        values: [clickup, jira, github, linear, asana]

      board_id:
        type: string
        description: "Board/list identifier in task system"
```

### Workspace Schema

**Multi-project workspace structure:**

```yaml
# workspace.schema.yaml

schema:
  workspace:
    type: object
    required: true
    properties:
      id:
        type: string
        required: true
        description: "Unique workspace identifier"

      name:
        type: string
        required: true
        description: "Workspace display name"

      projects:
        type: array
        items:
          $ref: "#/schemas/project"
        description: "All projects in workspace"

      environments:
        type: array
        items:
          type: enum
          values: [development, staging, production]

  task_management:
    type: object
    properties:
      system:
        type: string
        description: "Task management system (ClickUp, Jira, etc.)"

      boards:
        type: array
        items:
          type: object
          properties:
            id: string
            name: string
            type: enum [personal, team, client]
            projects: array
```

### Service Registry Schema

**Running services and endpoints:**

```yaml
# service.schema.yaml

schema:
  name:
    type: string
    required: true
    description: "Service display name"

  type:
    type: enum
    required: true
    values:
      - api           # HTTP API service
      - frontend      # Web frontend
      - worker        # Background worker
      - orchestration # Orchestration engine
      - database      # Database server
      - cache         # Cache service (Redis, etc.)
    description: "Service category"

  port:
    type: integer
    required: true
    minimum: 1024
    maximum: 65535
    description: "Port number (1024-65535)"

  url:
    type: string
    required: true
    pattern: "^https?://"
    description: "Full service URL (http:// or https://)"

  protocol:
    type: enum
    values: [http, https, tcp, udp, websocket]
    default: http

  auto_start:
    type: boolean
    default: false
    description: "Start service automatically on system boot"

  health_check:
    type: object
    properties:
      endpoint:
        type: string
        description: "Health check URL"

      interval:
        type: integer
        description: "Check interval in seconds"

      timeout:
        type: integer
        description: "Request timeout in seconds"

  dependencies:
    type: array
    items: string
    description: "Other services this depends on (by name)"
```

### Task Integration Schema

**Task management system integration:**

```yaml
# task-integration.schema.yaml

schema:
  system:
    type: enum
    required: true
    values: [clickup, jira, github, linear, asana, trello]

  authentication:
    type: object
    required: true
    properties:
      type:
        type: enum
        values: [api_key, oauth, token]

      credential_key:
        type: string
        description: "Key in credential vault"

  boards:
    type: array
    items:
      type: object
      properties:
        id:
          type: string
          required: true

        name:
          type: string

        type:
          type: enum
          values: [personal, team, client, backlog]

        statuses:
          type: array
          items:
            type: object
            properties:
              id: string
              name: string
              type: enum [open, in_progress, review, testing, done, blocked]

  automation:
    type: object
    properties:
      sync_frequency:
        type: integer
        description: "Sync interval in minutes"

      auto_status_update:
        type: boolean
        description: "Auto-update task status based on git events"

      pr_linking:
        type: boolean
        description: "Auto-link PRs to tasks"
```

---

## Template Examples

### Minimal Project Configuration

```yaml
# project-config.yaml

id: "my-project"
name: "My Project"
type: saas
location: "C:\\Projects\\my-project"

repository:
  url: "https://github.com/username/my-project"
  branch: "main"

tech_stack:
  - "ASP.NET Core 9.0"
  - "React"
  - "PostgreSQL"

status: active

task_management:
  system: clickup
  board_id: "901234567890"
```

### Complete Workspace Template

```yaml
# workspace-template.yaml

workspace:
  id: "my-workspace"
  name: "My Development Workspace"

  projects:
    - id: "project-a"
      name: "Project A"
      type: saas
      location: "C:\\Projects\\project-a"
      status: active

    - id: "project-b"
      name: "Project B"
      type: library
      location: "C:\\Projects\\project-b"
      status: maintenance

  environments:
    - development
    - staging
    - production

task_management:
  system: clickup
  boards:
    - id: "90123"
      name: "Internal Projects"
      type: team
      projects: ["project-a"]

    - id: "90124"
      name: "Client Projects"
      type: client
      projects: ["project-b"]
```

### Service Registry Template

```yaml
# services-template.yaml

services:
  - name: "Main API"
    type: api
    port: 5000
    url: "http://localhost:5000"
    protocol: https
    auto_start: true
    health_check:
      endpoint: "http://localhost:5000/health"
      interval: 30
      timeout: 5

  - name: "Frontend Dev Server"
    type: frontend
    port: 3000
    url: "http://localhost:3000"
    protocol: http
    auto_start: false
    dependencies: ["Main API"]

  - name: "Background Worker"
    type: worker
    port: 5123
    url: "https://localhost:5123"
    protocol: https
    auto_start: true
    dependencies: ["Main API"]
```

---

## Usage Patterns

### Project Registration

**Adding a new project to workspace:**

1. Copy `project-template.yaml`
2. Fill in project details
3. Validate against `project.schema.yaml`
4. Add to workspace registry
5. Initialize task management integration

### Service Discovery

**Auto-discovering running services:**

```yaml
# Service discovery pattern

discovery:
  method: port_scan
  range:
    start: 3000
    end: 9000

  validation:
    - check_http_response
    - validate_health_endpoint

  registration:
    - extract_service_name
    - determine_service_type
    - register_in_catalog
```

### Task Synchronization

**Bi-directional sync between git and task management:**

```yaml
# Sync configuration

sync:
  git_to_tasks:
    triggers:
      - commit_pushed
      - pr_created
      - pr_merged

    actions:
      - link_commit_to_task
      - update_task_status
      - post_comment_with_changes

  tasks_to_git:
    triggers:
      - task_status_changed
      - task_assigned
      - task_commented

    actions:
      - create_branch
      - notify_assigned_agent
```

---

## Validation

### Schema Validation

**Validating configuration files:**

```python
# Python example using jsonschema

import yaml
import jsonschema

# Load schema
with open('project.schema.yaml') as f:
    schema = yaml.safe_load(f)

# Load project config
with open('my-project.yaml') as f:
    project = yaml.safe_load(f)

# Validate
try:
    jsonschema.validate(instance=project, schema=schema)
    print("✅ Valid configuration")
except jsonschema.ValidationError as e:
    print(f"❌ Validation error: {e.message}")
```

### Common Validation Rules

1. **Project ID**: Lowercase, alphanumeric, hyphens only
2. **Paths**: Must be absolute, must exist
3. **URLs**: Must be valid HTTP/HTTPS
4. **Ports**: Must be in valid range (1024-65535)
5. **Status**: Must be one of predefined values
6. **Tech Stack**: Non-empty array
7. **Dependencies**: All referenced services must exist

---

## Integration Patterns

### ClickUp Integration

```yaml
# ClickUp workspace mapping

clickup:
  workspace_id: "90110135099"

  boards:
    - id: "901201459094"
      name: "Internal Projects"
      lists:
        - id: "901215559249"
          name: "Project A"
          project_id: "project-a"

        - id: "901215559250"
          name: "Project B"
          project_id: "project-b"

  statuses:
    backlog: {color: "gray", type: "open"}
    refined: {color: "blue", type: "open"}
    todo: {color: "yellow", type: "in_progress"}
    review: {color: "purple", type: "review"}
    testing: {color: "orange", type: "testing"}
    done: {color: "green", type: "done"}

  automation:
    pr_created: "→ review"
    pr_merged: "→ testing"
    tests_passing: "→ done"
```

### GitHub Integration

```yaml
# GitHub repository mapping

github:
  organization: "my-org"

  repositories:
    - name: "project-a"
      project_id: "project-a"
      default_branch: "main"

      webhooks:
        - event: "pull_request"
          action: "update_clickup_status"

        - event: "push"
          action: "link_commit_to_task"

  automation:
    branch_naming: "agent-{id}-{task-title}"
    pr_title_format: "{task-id}: {description}"
    auto_link_tasks: true
```

---

## Best Practices

### 1. Consistent Naming

```yaml
# Good
id: "my-project"
name: "My Project"

# Bad
id: "MyProject"   # No uppercase
name: "my_proj"   # Not human-readable
```

### 2. Absolute Paths

```yaml
# Good
location: "C:\\Projects\\my-project"

# Bad
location: "../my-project"  # Relative paths break
```

### 3. Explicit Dependencies

```yaml
# Good
services:
  - name: "API"
  - name: "Frontend"
    dependencies: ["API"]

# Bad
services:
  - name: "Frontend"
    # Implicit dependency not documented
```

### 4. Status Hierarchy

```yaml
# Well-defined progression
statuses:
  backlog → refined → todo → in_progress → review → testing → done

# Avoid ambiguous states
statuses:
  pending, maybe, started, almost, finished  # Too vague
```

### 5. Validation First

```yaml
# Always validate before using

validate_schema(project_config)
validate_dependencies(services)
validate_paths_exist(workspace)
```

---

## Migration Guide

### From Manual to Automated

**Step 1: Inventory**
```yaml
# Document current state
projects:
  - manually_tracked: true
    location: "C:\\Dev\\project1"
    task_tracking: "spreadsheet"
```

**Step 2: Schema Compliance**
```yaml
# Convert to standard schema
id: "project1"
name: "Project 1"
type: saas
location: "C:\\Dev\\project1"
task_management:
  system: clickup
  board_id: "123456"
```

**Step 3: Automation**
```yaml
# Enable automation
automation:
  sync_enabled: true
  auto_status_update: true
  pr_linking: true
```

### From Monolithic to Modular

1. **Extract project definitions** from single config
2. **Validate each project** against schema
3. **Create workspace** grouping related projects
4. **Link task management** per project
5. **Enable service discovery** for dependencies

---

## Advanced Patterns

### Multi-Environment Management

```yaml
environments:
  development:
    projects:
      - my-project:
          location: "C:\\Projects\\my-project"
          url: "http://localhost:5000"

  staging:
    projects:
      - my-project:
          location: "/var/www/my-project"
          url: "https://staging.example.com"

  production:
    projects:
      - my-project:
          location: "/var/www/my-project"
          url: "https://example.com"
```

### Dynamic Service Discovery

```yaml
discovery:
  strategies:
    - type: port_scan
      priority: 1

    - type: process_inspection
      priority: 2

    - type: registry_lookup
      priority: 3

  registration:
    auto_register: true
    validation_required: true
    conflict_resolution: "newest_wins"
```

### Cross-Project Dependencies

```yaml
dependencies:
  - project: "framework-a"
    type: library
    version: ">=2.0.0"
    required: true

  - project: "service-b"
    type: runtime
    status: running
    required: true
```

---

## Open Source Contribution

These schemas and templates are PUBLIC and freely shareable.

**Use cases:**
- Organizing your AI agent workspace
- Standardizing project configurations
- Building task management integrations
- Creating automated workflows

**License:** MIT (when published)

**Contributing:**
- Share your workspace schemas
- Suggest schema improvements
- Report validation edge cases
- Submit integration patterns

---

## Further Reading

- `jengo-identity-public` - AI agent identity patterns
- `jengo-system-public` - Core frameworks and architectures
- `jengo-knowledge-public` - Learning and memory patterns
- `jengo-world-public` - World modeling and relationships

---

**Version:** 1.0.0
**Status:** ACTIVE - Open-source workspace schemas
**License:** MIT (when published) - currently PRIVATE during development
