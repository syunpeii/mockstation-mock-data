# testCase Directory

This directory contains mock responses for Mockstation. Each folder represents one test case.

## Directory Structure

```
testCase/
├── default/          # Default test case
│   └── api/
│       ├── users/
│       │   ├── GET.res
│       │   └── POST.res
│       └── test/
│           └── GET.res
└── sample-case/      # Sample test case
    └── api/
        └── users/
            ├── GET@limit__10.res
            ├── GET@limit__0.res
            └── POST.res
```

## .res File Format

### METHOD_SUFFIX Format (Default)

File name: `{apiPath}/{METHOD}.res`

Examples: `api/users/GET.res`, `api/users/POST.res`

File content:

```
200 OK
Content-Type: application/json

[{"id":"1","name":"User 1"}]
```

**Structure:**

- **Line 1**: `<HTTP Status Code> <Status Text>`
- **Lines 2+ (until blank line)**: Headers (Content-Type, etc.)
- **Blank line**: Separator between headers and body
- **Remaining lines**: Response body (JSON, HTML, text, etc.)

### SIMPLE Format

File name: `{apiPath}.res`

Examples: `api/users.get.res`, `api/users.post.res`

Format is the same as METHOD_SUFFIX.

## Query Parameter Conditional Branching

To return different responses for the same endpoint based on query parameters:

```
GET@limit__10.res    # GET /api/users?limit=10
GET@limit__0.res     # GET /api/users?limit=0 (error response)
GET@page__1.res      # GET /api/users?page=1
```

For multiple parameters:

```
GET@page__1--status__active.res   # GET /api/users?page=1&status=active
```

**Matching priority:**

1. Exact match (all parameters)
2. Partial match
3. No parameters (`GET.res`)

## Switching Test Cases

Multiple test cases can be managed on the server. Default is `default`.

```bash
# Switch test case (via Desktop UI or API)
curl -X POST http://localhost:8080/api/testcases/activate \
  -H "Content-Type: application/json" \
  -d '{"testCaseId":"sample-case"}'
```

## Custom testCase Directory

Point the server to a custom testCase directory:

```bash
TESTCASE_DIR=/path/to/custom/testcase ./gradlew :server:run

# Or with Docker
docker run -e TESTCASE_DIR=/custom/path -v /path/to/custom:/custom mockstation
```
