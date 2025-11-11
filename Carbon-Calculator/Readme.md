# 🌱 Baseline & Application Monitoring Script

This project provides a Bash-based automation script to measure and compare system performance or energy behavior in two modes:

* **Baseline Mode:** When your application is **not running**.
* **Application Mode:** When your application **is running**.

The script automatically sets up a Python environment, initializes a monitoring process, and logs results into structured CSV files for easy comparison.

---

## 🧩 Features

* Automated setup and dependency installation
* Supports both baseline and active monitoring modes
* Flexible runtime duration
* Lightweight and non-intrusive execution
* Generates CSV logs with metadata (`team_name`, `app_run`)

---

## ⚙️ Prerequisites

Ensure the following are installed on your system:

| Tool           | Description                         | Check Command       |
| -------------- | ----------------------------------- | ------------------- |
| **Python 3.x** | Required for environment creation   | `python3 --version` |
| **pip**        | Python package manager              | `pip --version`     |
| **bash**       | Unix shell (default on Linux/macOS) | `bash --version`    |
| **timeout**    | Command-line timer utility          | `timeout --version` |

> 💡 On Ubuntu/Debian, install missing dependencies with:
>
> ```bash
> sudo apt update && sudo apt install python3 python3-venv python3-pip coreutils -y
> ```

---

## 🚀 How It Works

The script runs a background monitoring process for a specified duration and records metrics into a CSV file inside the `emissions_logs/` folder.

It operates in **two modes**:

1. **Baseline Mode (`--app-run false`)**
   Runs automatically for **1 hour (3600 seconds)** to capture baseline system metrics.

2. **Application Mode (`--app-run true`)**
   Requires the user to provide a custom duration in seconds using `--run-seconds`.
   Used when your application is actively running.

---

## 💡 Usage

### 🔹 Syntax

```bash
./emission_calculation.sh --app-run [true|false] --team-name <team_name> [--run-seconds <seconds>] [--debug true|false]
```

### 🔹 Parameters

| Parameter       | Required                          | Default | Description                                         |
| --------------- | --------------------------------- | ------- | --------------------------------------------------- |
| `--app-run`     | ✅ Yes                             | -       | Set `false` for baseline, `true` for app monitoring |
| `--team-name`   | ✅ Yes                             | -       | Identifies your team/project                        |
| `--run-seconds` | ⚠️ Required when `--app-run true` | -       | Duration in seconds for app monitoring              |
| `--debug`       | ❌ No                              | `false` | Shows detailed logs if `true`                       |

---

## 🧭 Step-by-Step Guide

### 1️⃣ Make the Script Executable

```bash
chmod +x emission_calculation.sh
```

### 2️⃣ Run Baseline Monitoring (1 Hour)

Run this **without your app running**:

```bash
./emission_calculation.sh --app-run false --team-name AlphaTeam
```

**What happens:**

* Sets up a Python virtual environment
* Prepares a configuration file
* Runs the monitor for **1 hour (3600 seconds)**
* Saves results to `./emissions_logs/monitoring_app_run_false.csv`

---

### 3️⃣ Run Application Monitoring (Custom Duration)

Run this **while your app is running**:

```bash
./emission_calculation.sh --app-run true --team-name AlphaTeam --run-seconds 3600
```

**What happens:**

* Uses the same setup as baseline mode
* Runs the monitor for **3600 seconds (1 Hour)**
* Saves results to `./emissions_logs/monitoring_app_run_true.csv`

---

## 📊 Output

After successful execution, your results will be stored in:

```
./emissions_logs/monitoring_app_run_false.csv
./emissions_logs/monitoring_app_run_true.csv
```

### CSV Columns Include:

| Column                    | Description                        |
| ------------------------- | ---------------------------------- |
| `timestamp`               | Time of data capture               |
| `duration`                | Total monitoring time (seconds)    |
| `energy_consumed`         | Energy used (kWh)                  |
| `cpu_power` / `ram_power` | Component usage                    |
| `team_name`               | Name of the team specified         |
| `app_run`                 | Indicates baseline or app run mode |

---

## 🔍 Example Output

```bash
✓ Configuration: app_run=false, team_name=AlphaTeam, run_seconds=3600, debug=false
[1/6] Creating Python virtual environment...
✓ Environment activated
[6/6] Starting monitoring process for 3600 seconds...
  Progress: [██████████████████████████████████████████████████] 100% | Completed
✓ Monitoring completed successfully
✓ Metadata added to output file
════════════════════════════════════════════════════════════
✓ Process completed successfully!
  Output file: ./emissions_logs/monitoring_app_run_false.csv
  Team Name: AlphaTeam
  App Running: false
  Duration: 3600 seconds
════════════════════════════════════════════════════════════
```

---

## ⚠️ Troubleshooting

| Issue                              | Possible Cause        | Solution                                    |
| ---------------------------------- | --------------------- | ------------------------------------------- |
| `Error: Python 3 is not installed` | Python missing        | Install via `sudo apt install python3`      |
| `timeout: command not found`       | Missing utility       | Install via `sudo apt install coreutils`    |
| `Permission denied`                | Script not executable | Run `chmod +x emission_calculation.sh`               |
| Virtual env not created            | Missing `venv` module | Install via `sudo apt install python3-venv` |

---

## 🧠 Example

| Mode     | Command Example                                                            | Runtime | Output File                    |
| -------- | -------------------------------------------------------------------------- | ------- | ------------------------------ |
| Baseline | `./emission_calculation.sh --app-run false --team-name AlphaTeam`                   | 1 hour  | `monitoring_app_run_false.csv` |
| App Run  | `./emission_calculation.sh --app-run true --team-name AlphaTeam --run-seconds 1200` | Custom  | `monitoring_app_run_true.csv`  |

---

