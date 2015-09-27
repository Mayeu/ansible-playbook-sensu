#Change Log
All notable changes to this project will be documented in this file.
This project adheres to [Semantic Versioning](http://semver.org/).
This change log follow the convention proposed by [Kepp a CHANGELOG](http://keepachangelog.com/).

## [0.2.0] 2015-09-27
### Changed
- `sensu_client_hostname` and `sensu_client_address` are now using
  fact data by default. This will prevent the need for override in a
  lot of cases.

### Fixed
- The playbook now force all the sensu part to be started when it is
  runned. Should fix #12

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

[unreleased]: https://github.com/Mayeu/ansible-playbook-sensu/compare/v0.2.0...HEAD
[0.2.0]: https://github.com/Mayeu/ansible-playbook-sensu/compare/v0.1.0...v0.2.0
