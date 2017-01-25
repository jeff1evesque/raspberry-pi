# Stig File(s)

- `stig-general`: performs general hardening, and should be executed as non `root`.
  - this should be run at least by the default `pi` user.
- `stig-root`: rename the default `pi` username, to a supplied perference, and should be executed as `root`.
  - should be run after `stig-general`.