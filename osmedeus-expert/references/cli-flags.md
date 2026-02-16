# CLI Flags Reference

## Global Flags (All Commands)

```
--settings-file           Path to osm-settings.yaml
--base-folder, -b         Base folder with workflows
--workflow-folder, -F     Custom workflow folder
--verbose, -v             Verbose output
--debug                   Debug mode (verbose + debug logging)
--silent, -q              Suppress all output except errors
--log-file                Path to log file
--log-file-tmp            Create temporary log file
--disable-logging         Disable all logging
--disable-color           Disable colored output
--disable-notification    Disable notifications
--spinner                 Show spinner animations
--ci-output-format        JSON output for CI pipelines
--usage-example, -H       Show usage examples
--full-usage-example      Show full usage in pager
--json                    Output in JSON format
--width                   Max column width for tables (default: 80)
--force                   Skip confirmation prompts
--disable-db              Disable database connection
--skip-auto-setup         Skip automatic setup
```

## Run Command Flags

### Workflow Selection

```
-f, --flow NAME           Flow workflow name
-m, --module NAME         Module workflow(s) (repeatable: -m mod1 -m mod2)
```

### Target Input

```
-t, --target TARGET       Target(s) (repeatable)
-T, --target-file FILE    File with targets (one per line)
--empty-target            Run without target
--convert-to-file         Write targets to temp file, use file path as target
--convert-file-to-line    Expand file target, each line becomes separate target
```

### Parameters

```
-p, --params KEY=VALUE    Parameters (repeatable: -p k1=v1 -p k2=v2)
-P, --params-file FILE    Parameters file (YAML or JSON)
-S, --space NAME          Override {{TargetSpace}}
-W, --workspaces-folder   Override {{Workspaces}}
-w, --workspace PATH      Custom workspace path
```

### Execution Control

```
-c, --concurrency N       Concurrent targets (default: 1)
-B, --tactic TACTIC       Run tactic: aggressive, default, gently
--threads-hold N          Override thread count (0 = use tactic)
--timeout DURATION        Run timeout (e.g., 2h, 30m, 1d)
--repeat                  Repeat after completion
--repeat-wait-time DUR    Wait between repeats (default: 1m)
--dry-run                 Show execution plan without running
--skip-validation         Skip target type validation
```

### Module Selection

```
-x, --exclude MODULE      Exclude module(s) (repeatable)
-X, --fuzzy-exclude STR   Exclude modules matching substring (repeatable)
--std-module              Read module YAML from stdin
--module-url URL          Fetch module YAML from URL
```

### Heuristics

```
--heuristics-check LEVEL  none, basic (default), advanced
```

### Chunking (Large Target Lists)

```
--chunk-size N            Split targets into chunks of N
--chunk-count N           Split into N equal chunks
--chunk-part M            Execute only chunk M (0-indexed)
--chunk-threads N         Override concurrency within chunk
```

### Distributed & Queue

```
-D, --distributed-run     Submit to worker queue (requires Redis)
--redis-url URL           Redis connection URL override
--queue                   Queue for later processing
--queue-run               Process queued tasks
-G, --progress-bar        Show progress bar
--disable-workflow-state  Don't save workflow YAML to output
```

### Server & Webhooks

```
--server-url URL          Server URL for cron registration
--run-priority PRIORITY   low, normal, high, critical
--as-webhook              Register webhook trigger
--webhook-auth-key KEY    Webhook authentication key
```
