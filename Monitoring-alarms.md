This is a proposal for monitoring alarms at Coopdevs.
# Disk
## Thresholds

We will define two thresholds: warning and critical. The time it takes between receiving a notification for one and the other will indicate the rate at which the disk is filling up. If the difference is just 5min, you better hurry to fix the issue.

| Severity | Threshold                               |
|----------|-----------------------------------------|
| Warning  | Disk use exceeds 70% of total available |
| Critical | Disk use exceeds 85% of total available |

#### Absolute limit

Apart from that, while the thresholds above are defined in relative terms, we suggest we add an absolute one to ensure no matter what there's always enough free disk to solve the root cause of the issue.

Said absolute limit could be 2 GB.

# Memory
## Thresholds
| Severity | Threshold                                                  |
| -------- | ---------------------------------------------------------- |
| Warning  | Memory use exceeds 70% of total available during 5 minutes |
| Critical | Memory use exceeds 85% of total available during 5 minutes | 

# CPU
## Thresholds
| Severity | Threshold                                                  |
| -------- | ---------------------------------------------------------- |
| Warning  | CPU use exceeds 70% of total available during 5 minutes |
| Critical | CPU use exceeds 85% of total available during 5 minutes | 

#### Absolute limit
100% during 3 minutes

## Per-project settings

We propose to go with this proposal for all projects and adapt per-project only once we have enough monitoring data to do so.
