---
name: Bug report
about: Create a report to help us improve
title: "[BUG] - <Component version> - <Title to describe the bug>"
labels: bug
assignees: ''

---

**Describe the bug**
A clear and concise description of what the bug is.

**To Reproduce**
Steps to reproduce the behavior:
1. Go to '...'
2. Click on '....'
3. Scroll down to '....'
4. See error

**Expected behavior**
A clear and concise description of what you expected to happen.

**Screenshots**
If applicable, add screenshots to help explain your problem.

**ESPHome installer version:**
 - 2025.7.1
 - 2025.6.1

**Upload Log from ESPHome Builder at VERY_VERBOSE level:**
```
logger:
  tx_buffer_size: 2048
  # level: NONE #ERROR #WARN #INFO #DEBUG #VERBOSE #VERY_VERBOSE 
  level: VERY_VERBOSE 
  logs:
    jk_rs485_bms: VERY_VERBOSE 
    jk_rs485_sniffer: VERY_VERBOSE 
```

**Additional context**
Add any other context about the problem here, like your yaml file or any other file that will help.
