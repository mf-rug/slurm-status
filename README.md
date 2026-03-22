# slurm-status

A terminal dashboard for Slurm HPC clusters. Shows your priority standing, job status, disk quota, and usage history — all in one glance.

![No dependencies](https://img.shields.io/badge/dependencies-none-green) ![Python 3.6+](https://img.shields.io/badge/python-3.6%2B-blue)

## Features

- **Fairshare & priority** — see how your usage compares to your allotment and what that means for scheduling priority
- **Running jobs** — elapsed time, time remaining, and progress bars
- **Pending jobs** — estimated start times, wait durations, and per-job status reasons (Resources, Priority, etc.)
- **Disk quota** — space and inode usage across all filesystems with color-coded warnings
- **Job history** — 14-day summary with success/fail/cancel breakdown
- **Usage tracking** — logs your fairshare data on every run; view trends with `--history`

## Installation

It's a single Python file with no dependencies beyond the standard library.

```bash
# Copy to somewhere in your PATH
cp slurm-status ~/bin/slurm-status
chmod +x ~/bin/slurm-status

# Or just run it directly
python3 slurm-status
```

## Usage

```bash
# Full dashboard
slurm-status

# Usage history charts
slurm-status --history
slurm-status -H
```

## Dashboard sections

### Fairshare & Priority Standing

Shows your effective usage vs. your fair share allotment. The priority verdict is based on the usage ratio (not the raw fairshare factor, which can be misleading).

### Running / Pending Jobs

Running jobs show a progress bar based on elapsed time vs. time limit. Pending jobs show the scheduler's estimated start time and the reason they're waiting:
- **next up, waiting for GPU** (Resources) — you're next in line
- **queued behind other jobs** (Priority) — other jobs have higher priority

### Disk Quota

Parses `hbquota` output. Color thresholds: green (<75%), yellow (75-90%), red (>90%).

### Usage History

Each dashboard run logs a snapshot to `~/.slurm_usage_history.jsonl`. Use `--history` to see tall Unicode charts of:
- Usage of your share over time
- Fairshare factor over time
- Raw CPU-seconds over time

## Notes

- The disk quota section uses `hbquota`. If your cluster uses a different quota command, update the `parse_hbquota()` function.
- Estimated start times come from Slurm's scheduler and are often conservative — jobs frequently start earlier than predicted.
- Requesting more **time** than needed has no fairshare penalty (you're billed for actual usage). Requesting more **CPUs/GPUs** than needed does cost you (billed for the full allocation).

## License

MIT
