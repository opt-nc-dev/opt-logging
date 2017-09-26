# Changelog
All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](http://keepachangelog.com/en/1.0.0/)
and this project adheres to [Semantic Versioning](http://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [1.0.0] - 2017-09-07
### Added
- logs-defaults.xml configuration
- logs-metiers.xml configuration
- define environment variable ENV_VAR_LOG_FILE
- define environment variable ENV_VAR_METIER_LOG_FILE
- Add LogMetierService component
### Changed
- Nothing
### Removed
- Nothing

## [1.1.0] - 2017-09-08
### Added
- define environment variable MAX_HISTORY
- define environment variable MAX_FILE_SIZE
- define environment variable TOTAL_SIZE_CAP
- Add log rotation configuration in logs-defaults.xml with SizeAndTimeBasedRollingPolicy
- Add log rotation configuration in logs-metiers.xml with SizeAndTimeBasedRollingPolicy
### Changed
- Rename environment variable ENV_VAR_LOG_FILE to LOG_FILE
- Rename environment variable ENV_VAR_METIER_LOG_FILE to LOG_FILE_JSON
### Removed
- Nothing

## [1.1.1] - 2017-09-08
### Added
- Nothing
### Changed
- Fix bug and replace FileAppender by RollingFileAppender
### Removed
- Nothing

## [1.2.0] - 2017-09-14
### Added
- Nothing
### Changed
- Only log on file when profile is not 'dev'
### Removed
- Nothing