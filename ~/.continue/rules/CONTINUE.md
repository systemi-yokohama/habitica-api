# Habitica API OpenAPI Specification Project

## Project Overview

This project provides a comprehensive OpenAPI 3.1.1 specification for the Habitica API, displayed through an interactive Swagger UI interface. Habitica is a gamified task management application that helps users build healthy habits by treating their life like a role-playing game.

### Key Technologies
- **OpenAPI 3.1.1**: API specification standard
- **Swagger UI**: Interactive API documentation interface
- **GitHub Pages**: Static site hosting
- **GitHub Actions**: Automated Swagger UI updates
- **YAML**: API specification format

### High-Level Architecture
This is a static documentation site that:
1. Defines the Habitica API using OpenAPI specification in `openapi.yaml`
2. Renders the specification using Swagger UI for interactive exploration
3. Automatically updates Swagger UI to the latest version via GitHub Actions
4. Hosts the documentation on GitHub Pages

## Getting Started

### Prerequisites
- A web browser (for viewing the documentation)
- Git (for contributing to the project)
- Text editor or IDE (for editing the OpenAPI specification)
- Basic understanding of OpenAPI/Swagger specifications
- (Optional) Node.js for local testing

### Installation Instructions

1. **Clone the repository**:
   ```bash
   git clone <repository-url>
   cd <repository-name>
   ```

2. **Open the documentation locally**:
   Simply open `index.html` in your web browser:
   ```bash
   open index.html  # macOS
   # or
   xdg-open index.html  # Linux
   # or
   start index.html  # Windows
   ```

3. **For local development with live reload** (optional):
   ```bash
   # Using Python's built-in server
   python3 -m http.server 8080
   # Then open http://localhost:8080 in your browser
   
   # Or using Node.js http-server
   npx http-server -p 8080
   ```

### Basic Usage Examples

1. **View the API documentation**: Open `index.html` in a browser or visit the GitHub Pages URL
2. **Explore endpoints**: Click on any endpoint in the Swagger UI to see details
3. **Test API calls**: Use the "Try it out" feature in Swagger UI (requires valid API credentials)
4. **Search endpoints**: Use Ctrl+F or the search functionality to find specific endpoints

### Running Tests
This project doesn't have traditional tests, but you can validate the OpenAPI specification:

```bash
# Install swagger-cli globally
npm install -g @apidevtools/swagger-cli

# Validate the OpenAPI specification
swagger-cli validate openapi.yaml

# Bundle the specification (if needed)
swagger-cli bundle openapi.yaml --outfile bundled-spec.yaml
```

## Project Structure

```
.
├── .github/
│   └── workflows/
│       └── update-swagger.yml    # Automated Swagger UI updates
├── dist/                          # Swagger UI distribution files
│   ├── swagger-ui.css            # Swagger UI styles
│   ├── swagger-ui.js             # Swagger UI JavaScript
│   ├── swagger-initializer.js   # Configuration for API spec
│   └── ...                       # Other Swagger UI assets
├── openapi.yaml                  # Main API specification file
├── index.html                    # Entry point for documentation
├── _config.yml                   # Jekyll configuration (for GitHub Pages)
├── CNAME                         # Custom domain configuration (if any)
├── swagger-ui.version            # Tracks current Swagger UI version
└── README.md                     # Project readme
```

### Key Files and Their Roles

- **`openapi.yaml`**: The heart of the project - contains the complete Habitica API specification including:
  - API endpoints (paths)
  - Request/response schemas
  - Authentication methods
  - Component definitions
  - Tags and descriptions

- **`index.html`**: The main entry point that loads Swagger UI and points it to the OpenAPI specification

- **`dist/swagger-initializer.js`**: Configures Swagger UI to load `openapi.yaml` instead of the default Petstore API

- **`.github/workflows/update-swagger.yml`**: GitHub Actions workflow that:
  - Runs weekly (every Saturday at 10:00 AM)
  - Checks for new Swagger UI releases
  - Automatically updates the Swagger UI distribution
  - Creates a pull request with the updates

## Development Workflow

### Coding Standards and Conventions

1. **OpenAPI Specification**:
   - Follow OpenAPI 3.1.1 standards
   - Use consistent indentation (2 spaces)
   - Include comprehensive descriptions for all endpoints
   - Provide examples for request/response bodies
   - Use `$ref` for reusable components

2. **YAML Formatting**:
   - Maintain consistent formatting
   - Use comments to explain complex structures
   - Keep line length reasonable for readability

3. **Documentation**:
   - Write clear, concise endpoint descriptions
   - Include all required and optional parameters
   - Document all possible response codes
   - Provide realistic examples

### Testing Approach

1. **Specification Validation**:
   ```bash
   swagger-cli validate openapi.yaml
   ```

2. **Visual Inspection**:
   - Open the Swagger UI locally
   - Review each endpoint for accuracy
   - Test the "Try it out" functionality with valid credentials

3. **Schema Validation**:
   - Ensure all schemas are properly defined
   - Verify `$ref` references resolve correctly
   - Check that examples match schemas

### Build and Deployment Process

1. **Automatic Deployment** (GitHub Pages):
   - Push changes to the main branch
   - GitHub Pages automatically serves the updated documentation
   - No build step required for the documentation itself

2. **Swagger UI Updates**:
   - Automated via GitHub Actions workflow
   - Runs weekly and on manual trigger
   - Creates a PR for review before merging

3. **Manual Swagger UI Update**:
   ```bash
   # Download latest Swagger UI release
   curl -sL -o swagger-ui.tar.gz https://api.github.com/repos/swagger-api/swagger-ui/tarball/latest
   
   # Extract dist directory
   tar -xzf swagger-ui.tar.gz --strip-components=1 swagger-api-swagger-ui-*/dist
   
   # Update references in swagger-initializer.js
   sed -i "s|https://petstore.swagger.io/v2/swagger.json|openapi.yaml|g" dist/swagger-initializer.js
   
   # Update index.html references
   sed -i "s|href=\"./|href=\"dist/|g" index.html
   sed -i "s|src=\"./|src=\"dist/|g" index.html
   ```

### Contribution Guidelines

1. **Adding New Endpoints**:
   - Place in appropriate section under `paths:`
   - Include all HTTP methods (GET, POST, PUT, DELETE)
   - Add appropriate tags for categorization
   - Define request body schemas
   - Document all response codes

2. **Updating Existing Endpoints**:
   - Maintain backward compatibility when possible
   - Update the version number if breaking changes are made
   - Document changes in commit messages

3. **Adding Schemas**:
   - Define reusable schemas under `components.schemas`
   - Use descriptive names
   - Include all properties with types and descriptions
   - Mark required fields

4. **Security Definitions**:
   - All security requirements are defined under `components.securitySchemes`
   - Habitica uses three headers: `x-api-user`, `x-api-key`, and `x-client`

## Key Concepts

### Domain-Specific Terminology

- **Habitica**: A gamified productivity app where users' real-life tasks affect their in-game character
- **User**: A Habitica account holder with stats, tasks, and inventory
- **Tasks**: The core unit of work - can be Habits, Dailies, To-Dos, or Rewards
- **Party**: A group of users working together on quests
- **Guild**: A community group for users with shared interests
- **Challenge**: A set of tasks that multiple users can participate in
- **Cron**: The daily reset mechanism that evaluates Dailies and applies effects
- **Stats**: Character attributes (STR, INT, CON, PER) that affect gameplay
- **Gold/Gems**: In-game currencies
- **Quest**: Collaborative boss battles that parties undertake

### Core Abstractions

1. **Authentication**:
   - Three-part authentication: User ID, API Token, and Client ID
   - Defined in `securitySchemes` component
   - Applied per-endpoint via `security` property

2. **Response Structure**:
   - Standard response format with `success`, `data`, `notifications`, `userV`, and `appVersion`
   - Error responses include `error` and `message` fields

3. **Resource Hierarchy**:
   ```
   User (authenticated)
   ├── Tasks (habits, dailies, todos, rewards)
   ├── Inventory (items, gear, pets, mounts)
   ├── Party/Groups
   ├── Challenges
   └── Messages/Inbox
   ```

4. **Tags System**:
   - Used to categorize and organize API endpoints
   - Examples: user, tasks, party, groups, challenges, chat

### Design Patterns Used

1. **RESTful API Design**:
   - Resource-based URLs
   - Standard HTTP methods
   - Proper status codes

2. **Component Reusability**:
   - Shared schemas in `components.schemas`
   - Reusable security schemes
   - Common response patterns

3. **Convention over Configuration**:
   - Consistent parameter naming
   - Standard response structures
   - Predictable endpoint patterns

## Common Tasks

### 1. Adding a New API Endpoint

```yaml
# In openapi.yaml under paths:
/v3/your-new-endpoint:
  get:
    summary: Short description
    description: Detailed description
    tags:
      - appropriate-tag
    security:
      - userId: []
        apiToken: []
        xClient: []
    parameters:
      - name: paramName
        in: query/path
        required: true/false
        schema:
          type: string
        description: Parameter description
    responses:
      "200":
        description: Success
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/DataResponse"
      "401":
        description: Unauthorized
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/ErrorMessageResponse"
```

### 2. Adding a New Schema Component

```yaml
# In openapi.yaml under components.schemas:
YourNewSchema:
  type: object
  properties:
    propertyName:
      type: string
      description: Property description
      example: Example value
    requiredProperty:
      type: string
  required:
    - requiredProperty
```

### 3. Testing Changes Locally

```bash
# 1. Make changes to openapi.yaml
# 2. Validate the specification
swagger-cli validate openapi.yaml

# 3. Start a local server
python3 -m http.server 8080

# 4. Open http://localhost:8080 in your browser
# 5. Review the changes in Swagger UI
```

### 4. Updating Swagger UI Version

```bash
# Trigger the GitHub Action manually:
# 1. Go to Actions tab in GitHub
# 2. Select "Update Swagger UI" workflow
# 3. Click "Run workflow"

# Or update manually:
# 1. Check latest version
curl -s https://api.github.com/repos/swagger-api/swagger-ui/releases/latest | jq -r ".tag_name"

# 2. Follow the manual update steps in the workflow file
```

### 5. Finding Specific Endpoints

```bash
# Search the YAML file for specific patterns
grep -n "summary:" openapi.yaml | grep -i "task"
grep -n "POST" openapi.yaml
grep -A 5 "/v3/user" openapi.yaml
```

### 6. Validating Authentication Requirements

Check which endpoints require authentication:
```bash
grep -B 5 "security:" openapi.yaml
```

### 7. Finding All Endpoints for a Specific Tag

```bash
grep -A 2 "tags:" openapi.yaml | grep -A 1 "- user"
```

## Troubleshooting

### Common Issues and Solutions

#### Issue: Swagger UI not loading the specification
**Symptoms**: Swagger UI shows the default Petstore API instead of Habitica API

**Solutions**:
1. Check `dist/swagger-initializer.js` - ensure it points to `openapi.yaml`
2. Verify `openapi.yaml` is in the root directory
3. Check browser console for errors
4. Ensure the file is valid YAML (no syntax errors)

#### Issue: "Error downloading" message in Swagger UI
**Symptoms**: Swagger UI shows an error message about failing to download the spec

**Solutions**:
1. Validate YAML syntax: `swagger-cli validate openapi.yaml`
2. Check for special characters that might break YAML parsing
3. Ensure proper indentation (2 spaces, no tabs)
4. Verify all `$ref` references are valid

#### Issue: Endpoints not showing up in Swagger UI
**Symptoms**: Some or all endpoints are missing from the UI

**Solutions**:
1. Check that paths are properly indented under `paths:`
2. Verify there are no YAML syntax errors above the missing endpoints
3. Ensure the path starts with `/` 
4. Validate the specification: `swagger-cli validate openapi.yaml`

#### Issue: "Try it out" feature not working
**Symptoms**: Can't test API calls from Swagger UI

**Solutions**:
1. CORS may be blocked - this is expected for cross-origin requests
2. Ensure you have valid API credentials (x-api-user, x-api-key)
3. Check if the API endpoint requires specific headers
4. Try using a CORS proxy for testing (not for production)

#### Issue: GitHub Actions workflow failing
**Symptoms**: Automated Swagger UI update workflow fails

**Solutions**:
1. Check workflow logs in GitHub Actions tab
2. Verify the release tag format from Swagger UI releases
3. Ensure the workflow has proper permissions
4. Check if `swagger-ui.version` file exists and is readable

#### Issue: Changes not reflecting on GitHub Pages
**Symptoms**: Updates pushed to main branch don't appear on the live site

**Solutions**:
1. Wait a few minutes for GitHub Pages to rebuild (usually 1-5 minutes)
2. Check GitHub Pages settings in repository settings
3. Hard refresh browser (Ctrl+Shift+R or Cmd+Shift+R)
4. Clear browser cache
5. Verify GitHub Pages is enabled and pointed to the correct branch

### Debugging Tips

1. **YAML Validation**:
   ```bash
   # Use online validators for quick checks
   # Or use swagger-cli for comprehensive validation
   swagger-cli validate openapi.yaml
   ```

2. **Browser Developer Tools**:
   - Open DevTools (F12)
   - Check Console tab for JavaScript errors
   - Check Network tab to see if files are loading
   - Look for CORS errors or 404s

3. **Local Testing**:
   - Always test changes locally before pushing
   - Use a local HTTP server (not `file://` protocol)
   - Test in multiple browsers if issues persist

4. **Version Control**:
   ```bash
   # Compare current version with working version
   git diff HEAD~1 openapi.yaml
   
   # Revert problematic changes
   git checkout HEAD~1 -- openapi.yaml
   ```

5. **Specification Linting**:
   ```bash
   # Install spectral for advanced linting
   npm install -g @stoplight/spectral-cli
   
   # Lint the specification
   spectral lint openapi.yaml
   ```

## References

### Official Documentation
- [Habitica API Documentation](https://habitica.com/apidoc/) - Official API docs
- [OpenAPI Specification](https://spec.openapis.org/oas/v3.1.0) - OpenAPI 3.1 spec
- [Swagger UI Documentation](https://swagger.io/tools/swagger-ui/) - Swagger UI guide
- [GitHub Pages Documentation](https://docs.github.com/en/pages) - Hosting guide
- [GitHub Actions Documentation](https://docs.github.com/en/actions) - Workflow automation

### Tools and Resources
- [Swagger Editor](https://editor.swagger.io/) - Online OpenAPI editor
- [Swagger CLI](https://github.com/APIDevTools/swagger-cli) - Validation tool
- [Spectral](https://stoplight.io/open-source/spectral) - OpenAPI linter
- [YAML Validator](https://www.yamllint.com/) - Online YAML validation

### Related Projects
- [Habitica Main Repository](https://github.com/HabitRPG/habitica) - Main Habitica codebase
- [Habitica Mobile Apps](https://github.com/HabitRPG/habitica-ios) - iOS app
- [Swagger UI Repository](https://github.com/swagger-api/swagger-ui) - Swagger UI source

### Community Resources
- Habitica Wiki - For understanding the domain concepts
- Habitica API community - For API usage discussions
- OpenAPI community - For specification best practices

---

## Quick Reference

### Authentication Headers
```
x-api-user: <your-user-uuid>
x-api-key: <your-api-token>
x-client: <your-client-uuid>
```

### Common Response Codes
- `200` - Success
- `201` - Created
- `400` - Bad Request
- `401` - Unauthorized
- `403` - Forbidden
- `404` - Not Found
- `500` - Internal Server Error

### Main API Sections
- User Management (`/v3/user/*`)
- Tasks (`/v3/tasks/*`)
- Groups & Parties (`/v3/groups/*`, `/v3/looking-for-party`)
- Challenges (`/v3/challenges/*`)
- Social (`/v3/members/*`, `/v3/inbox/*`)
- Content & State (`/v3/content`, `/v3/world-state`)
- Data Export (`/v3/export/*`)

### File Modification Checklist
When modifying the project, ensure:
- [ ] YAML syntax is valid
- [ ] All `$ref` references are correct
- [ ] Examples match schema definitions
- [ ] Required fields are marked
- [ ] Descriptions are clear and complete
- [ ] Changes validated with `swagger-cli validate`
- [ ] Tested locally before committing
- [ ] Commit messages are descriptive

---

**Last Updated**: December 2024
**OpenAPI Version**: 3.1.1
**Swagger UI Version**: Check `swagger-ui.version` file
