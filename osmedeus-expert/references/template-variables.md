# Template Variables & Functions

## Built-in Variables

### Execution Context

| Variable | Description |
|----------|-------------|
| `{{Target}}` | Target being scanned |
| `{{Output}}` | Workspace output directory |
| `{{TargetSpace}}` | Target-specific workspace |
| `{{Workspaces}}` | Root workspaces folder |
| `{{RunUUID}}` | Unique run identifier |
| `{{WorkflowName}}` | Executing workflow name |
| `{{Binaries}}` | Path to installed tool binaries |
| `{{Data}}` | Path to data directory (wordlists, etc.) |
| `{{BaseDir}}` | Base directory path |
| `{{DefaultUA}}` | Default user agent string |
| `{{Version}}` | Osmedeus version |

### Platform Detection

| Variable | Values |
|----------|--------|
| `{{PlatformOS}}` | `linux`, `darwin`, `windows` |
| `{{PlatformArch}}` | `amd64`, `arm64` |
| `{{PlatformInDocker}}` | `"true"` if in Docker |
| `{{PlatformInKubernetes}}` | `"true"` if in Kubernetes |
| `{{PlatformCloudProvider}}` | `aws`, `gcp`, `azure`, `local` |

## Custom Parameters

Define in `params:` section, access as `{{param_name}}`:

```yaml
params:
  - name: threads
    default: "10"
  - name: wordlist
    default: "{{Data}}/wordlist.txt"
    required: false

steps:
  - name: scan
    type: bash
    command: "tool -t {{threads}} -w {{wordlist}} {{Target}}"
```

## Parameter Types

```yaml
params:
  - name: param_name
    type: "string"      # string (default), bool, int
    default: "value"    # Default value
    required: true      # Fail if not provided
    generator: uuid()   # Auto-generate value
```

## Parameter Generators

| Generator | Output |
|-----------|--------|
| `uuid()` | UUID v4 |
| `currentDate` | Current date (2006-01-02) |
| `currentDate("2006-01-02T15:04:05")` | Custom date format |
| `currentTimestamp` | Unix timestamp |
| `getEnvVar("KEY")` | Environment variable |
| `getEnvVar("KEY", "default")` | Env var with default |
| `concat("a", "b", "c")` | String concatenation |
| `randomInt` | Random 0-100 |
| `randomInt(1, 10)` | Random in range |
| `randomString` | Random 16-char string |
| `randomString(32)` | Random N-char string |
| `execCmd("ls -la")` | Command output |
| `toLower("TEXT")` | Lowercase |
| `toUpper("text")` | Uppercase |
| `trim("  text  ")` | Trim whitespace |
| `replace("old", "new", "text")` | String replacement |
| `split("a,b,c", ",", 0)` | Split and get index |
| `join(",", "a", "b")` | Join with delimiter |

## Foreach Loop Variables

Use `[[variable]]` (double brackets) inside foreach steps to avoid conflicts with `{{template}}` syntax:

```yaml
- name: scan-each
  type: foreach
  input: "{{Output}}/targets.txt"
  variable: host
  step:
    name: scan
    type: bash
    command: "nmap [[host]] -o {{Output}}/nmap/[[host]].xml"
    #              ^^^^^^^^                    ^^^^^^^^
    #           loop variable              loop variable
    #     {{Output}} is a template variable (resolved once)
```

## Utility Functions (Goja JS Runtime)

Available in `function` steps, `pre_condition`, `condition`, and `script` fields:

### File Operations
- `file_exists(path)` - Check if file exists
- `file_length(path)` - Count non-empty lines
- `read_file(path)` - Read file contents
- `is_empty(value)` - Check if string/file is empty

### Logging
- `log_info(msg)` - Info level log
- `log_warn(msg)` - Warning log
- `log_error(msg)` - Error log

### Data Processing
- `trim(str)` - Trim whitespace
- `exec_python(code)` - Run Python code
- `detect_language(path)` - Detect programming language
- `extract_to(source, dest)` - Extract archive

### Database
- `db_import_sarif(workspace, path)` - Import SARIF vulnerabilities
- `convert_sarif_to_markdown(in, out)` - SARIF to markdown

### Step Exports
- `{{Result}}` - Return value from `script` block in function steps
- `{{response.body}}` - HTTP step response body
- `{{response.content}}` - LLM step response content
- `{{agent_content}}` - Agent step final response
