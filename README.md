# Task Management System

A tmux-based task scheduling and monitoring tool with support for concurrent execution, real-time monitoring, email notifications, and more.

## Features

- 🚀 Concurrent execution of multiple tasks
- 📊 Real-time monitoring and status viewing
- 🛠️ System resource monitoring (CPU, memory, disk)
- 📝 Automatic log management
- 📧 Email notifications (optional)
- 🔧 Simple configuration management

## Quick Start

### Step 1: Install

```bash
# Install from PyPI
pip install lite-slurm

# Or install from source
git clone <repository-url>
cd slurm-like-task-manager
pip install -e .
```

**Requirements:** Python 3.7+ and tmux (install with `sudo apt-get install tmux` or `brew install tmux`)

### Step 2: Initialize

```bash
task config init
```

### Step 3: Run Your First Task

```bash
# Run a task
task run "My Task" "python my_script.py"

# List tasks
task list

# Monitor task output
task monitor <task_id>

# Stop task
task kill <task_id>

# Clean a task
task cleanup <task_id>
```

That's it! You're ready to use the task manager.

## Installation

### From PyPI

```bash
pip install lite-slurm
task config init
```

### From Source

```bash
git clone <repository-url>
cd slurm-like-task-manager
pip install -r requirements.txt
pip install -e .
task config init
```

### System Requirements

- Python 3.7+
- tmux (install: `sudo apt-get install tmux` / `brew install tmux`)

## Usage

### Basic Commands

```bash
# Run task
task run "Task Name" "command to execute"

# List tasks
task list                          # All tasks
task list --status running         # Filter by status
task list --resources              # Show system resources

# Monitor task
task monitor <task_id>            # Real-time monitoring
task monitor <task_id> --lines 100 # Show last 100 lines

# Stop task
task kill <task_id>               # Stop single task
task kill <task_id1> <task_id2>   # Stop multiple tasks
task kill --all                   # Stop all running tasks
task kill <task_id> --force       # Force stop (immediate kill)

# View task info
task status <task_id>             # Task status
task output <task_id>             # Task output
task logs <task_id>               # Task logs

# Cleanup
task cleanup                      # Clean tasks older than 24h
task cleanup 12                   # Clean tasks older than 12h
```

### Examples

```bash
# Run training task
task run "Model Training" "python train.py --epochs 100"

# Run multiple tasks concurrently
task run "Data Processing" "python process_data.py"
task run "Feature Extraction" "python extract_features.py"

# Monitor and manage
task list --resources
task monitor 00001
task kill 00001 00002 00003       # Kill multiple tasks
```

## Configuration

### Email Notifications (Optional)

To enable email notifications:

1. **Create email config** (`~/.task_manager/config/email_config.json`):
   ```json
   {
       "enabled": true,
       "to_email": "your-email@example.com"
   }
   ```

2. **Setup Google API credentials**:
   - Get credentials from [Google Cloud Console](https://console.cloud.google.com/)
   - Enable Gmail API and create OAuth 2.0 credentials
   - Save as `~/.task_manager/config/credentials.json`

3. **Get Gmail token**:
   ```bash
   task config google_api login
   ```

**Quick import** (if you have existing files):
```bash
task config email ~/email_config.json
task config token ~/token.json
task config google_api file ~/credentials.json
```

### Configuration Commands

```bash
task config init                  # Initialize config
task config show                  # Show current config
task config test                  # Test email config
task config email <file>          # Import email config
task config token <file>          # Import token
task config google_api file <file> # Import credentials
task config google_api login      # Login to get token
```

## Command Reference

### `task run <name> <command> [priority] [-r|--realtime]`

Run new task.

- `name`: Task name
- `command`: Command to execute
- `priority`: Task priority (0-10, default 0)
- `-r, --realtime`: Enable real-time unbuffered output

**Examples:**
```bash
task run "Train Model" "python train.py"
task run "Important" "python script.py" 10
task run -r "Stream Logs" "python long.py"
```

### `task kill <task_id> [task_id2] ... [--force] | --all [--force]`

Stop task(s).

- `task_id`: Task ID(s) to stop (can specify multiple)
- `--all`: Stop all running tasks
- `--force`: Force kill immediately (without graceful shutdown)

**Examples:**
```bash
task kill 00001
task kill 00001 00002 00003
task kill 00001 --force
task kill --all
```

### `task monitor <task_id> [--lines N] [--refresh SECONDS]`

Real-time task monitoring.

- `--lines N`: Show last N lines (default: 50)
- `--refresh S`: Refresh interval in seconds (default: 2.0)

### `task list [--status <status>] [--resources]`

List tasks.

- `--status <status>`: Filter by status (pending, running, completed, failed, killed)
- `--resources`: Show system resource usage

### `task status <task_id>`

View task status details.

### `task output <task_id> [--lines N]`

View task output (default: last 50 lines).

### `task logs <task_id> [lines]`

View task log file (default: last 100 lines).

### `task cleanup [-t/--time hours] [task_id1] [task_id2] ...`

Clean up tasks.

- `-t, --time hours`: Clean tasks older than N hours (default: 24)
- `task_id`: Clean specific task(s)

**Examples:**
```bash
task cleanup                    # Clean tasks older than 24h
task cleanup -t 48              # Clean tasks older than 48h
task cleanup 00001 00002        # Clean specific tasks
```

### `task email <action>`

Email notification management.

- `enable`: Enable notifications
- `disable`: Disable notifications
- `show`: Show current config
- `test`: Send test email

## Task Status

- `pending`: Waiting to start
- `running`: Currently executing
- `completed`: Successfully finished
- `failed`: Failed with error
- `killed`: Manually stopped

## File Structure

```
~/.task_manager/
├── config/
│   ├── email_config.json    # Email configuration
│   ├── credentials.json     # Google API credentials
│   └── token.json           # Gmail token
├── logs/                    # Task log files
│   └── <task_id>.log
└── tasks.json               # Task database
```

## Troubleshooting

- **tmux not found**: Install with `sudo apt-get install tmux` or `brew install tmux`
- **Permission denied**: Check script execute permissions
- **Email not working**: Verify Gmail API config and token validity
- **Task not starting**: Check for tmux session name conflicts

Logs location: `~/.task_manager/logs/<task_id>.log`

## Uninstallation

```bash
pip uninstall lite-slurm
rm -rf ~/.task_manager
```

## License

MIT License

## Author

Created by zheng
