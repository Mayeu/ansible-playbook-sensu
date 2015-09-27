#Change Log
All notable changes to this project will be documented in this file.
This project adheres to [Semantic Versioning](http://semver.org/).
This change log follow the convention proposed by [Kepp a CHANGELOG](http://keepachangelog.com/).

## Unreleased

### Fixed

- The playbook now force all the sensu part to be started when it is runned. Should fix #12

## 0.1.0

### Added

- This changelog
- Semver
- All the user to define the path and name of the certificate to use.

### Changed

- Patched init script (to prevent sensu to start before redis &
  rabbitmq) on Debian are now optional but still activated by
  default.
- The playbook can now run with empty handlers and checks.
- Split Sensu configuration
- Replace the dashboard install with Uchiwa
- More explanation on how to uses this role

### Fixed

- When activated, the Debian patched init script are now forced even
  if Debian complain about previously existing files.
- Debian's patched init script are now weakly dependent on Redis &
  RabbitMQ server
