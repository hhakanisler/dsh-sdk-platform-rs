# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.4.11] -2024-09-30
### Fixed
- Retry mechanism for when PKI endpoint is not yet avaialble during rolling restart DSH ([#101](https://github.com/kpn-dsh/dsh-sdk-platform-rs/issues/101))

## [0.4.10] -2024-09-30
### Added
- Add new with client methods to REST and MQTTT token fetcher

### Changed
- used async aware Mutex in MQTTT token fetcher
- Cargo clippy suggestions


## [0.4.9] - 2024-09-27
### Changed
- Bugfix deadlock when running on mac together with Kafka Proxy
- Adjust logging to INFO for fetching environment variables

## [0.4.8] - 2024-09-13
### Fixed
- Correct Prod-lz endpoint 

## [0.4.7] - yanked - 2024-09-13
### Changed
- updated rest token endpoint due to CP migration

## [0.4.6] - 2024-08-24
### Added
- Add token fetcher for DSH MQTT
  - fetch token for MQTT
  - cache tokens till expiration time
  - support fetching tokens for multiple clients
### Changed
- Updated dependencies
  - Lazy_static to 1.5


## [0.4.5] - 2024-07-24
### Added
- Add additional/optional config for producers and consumer
  - The config can be set via environment variables

## [0.4.4] - 2024-07-15 
### Added
- Provide new methods on `Properties`
  - fetch_datastream: Highlevel async method to retun a `Result<Datastream>`
  - fetch_datastream_blocking: Highlevel blocking method to return a `Result<Datastream>`
  - reqwest_blocking_client_config: Returns a `Result<reqwest::blocking::ClientConfig>`
- Provide new methods on `Datastream`
  - fetch: Lowlevel async method to retun a `Result<Datastream>`
  - fetch_datastream: Lowlevel  blocking method to return a `Result<Datastream>`
- Add derive Debug to graceful_shutdown::Shutdown
- Provide optional way to give path to load local_datastreams.json (`LOCAL_DATASTREAMS_JSON`)

### Changed
- Verify if local_datastreams.json parses correctly, else panic instead of using datastreams default
- Optimized bootstrap sequence and moved fetching datastreams to a separate method in `Datastream`


## [0.4.3] - 2024-07-08
### Added
- Add missing get functions to datastreams struct

### Fixed
- Issue with building docsrs

## [0.4.2] - 2024-07-01
### Added
- Add token fetcher for DSH REST API
- Add platform enum with metadata
- Add example on how to use the token fetcher and rest api client


## [0.4.1] - 2024-06-13
### Added
- Add loading Certificates and Keys from $PKI_CONFIG_DIR
  - Compatbile with DSH VPN
  - Compatbile with Kafka Proxy
- Overwrrite tenant name via $DSH_TENANT_NAME variable

### Changed
- Restructure of the private functions to make it more modular
- Improved logging
- Improved API Documentation

## [0.4.0] - 2024-04-25

### Fixed
- Fixed vulnerability RUSTSEC-2023-0071 by replacing Picky with RCGen
- Fixed compile issues when defaullt feature = false and graceful_shutdown, metrics or DLQ is enabled

### Added
- Add Default implementation for Dsh::Properties
  - Points to localhost:9092 for kafka, localhost:8081 for schemastore
  - local_datastreams.json is now optional as it falssback to default values
- Overwrite Kafka config via environment variables for producer and consumer
- Add extra check in github actions to check for compile issues for all features independently

### Changed
- **Breaking change:** consumer_rdkafka_config and producer_rdkafka_config returns `ClientConfig` instead of `Result<ClientConfig>`
- **Breaking change:** certificates and keys are now returned as `T` instead of `Result<T>` 
- **Breaking change:** Private key is based on ECDSA instead of RSA 
- **Breaking change:** Error enum is now non_exhaustive

### Removed
- Removed return of Picky key struct is removed

## [0.3.1] - 2024-03-25
  
### Fixed
- Add missing prometheus export back to the SDK

## [0.3.0] - yanked - 2024-03-25
  
### Changed
- Metrics http server based on Hyper instead of Warp
  - **Breaking change:**  start_metrics_server() now returns a tokio::JoinHandle<Result<()>>
  - Runs per default on a separate thread
- Fixed documentation for dsh::Properties::get()

### Removed
- Deprecated Dsh::Properties::new() removed
- Removed unused dependencies in Cargo.toml

## [0.2.0] - 2024-03-06
  
### Added

- Make dsh::Properties lazily available dsh::Properties::get() 
- New low level certificate functions to the SDK
  - Return keys and certificates as DER format
  - Create client.key, client.crt and ca.crt in a folder
- Write datastreams.json as file to a folder
- Read DSH_SECRET_TOKEN_PATH from environment and read secret token from file as fallback for DSH_SECRET_TOKEN
  - Required for running in system space
- Logging when reading required env variables
- Task_id to Properties
- Some missing unit tests

### Changed
Most breaking changes are related to the new low level API and do not impact normal use of the SDK.
- Add deprecation warning to dsh::Properties::new() to use dsh::Properties::get() instead
- **Breaking change:**  Return producer config in a `Result<ClientConfig>` instead of a `ClientConfig`
- **Breaking change:**  Pem formatted certificates and keys returns a `Result<String>` instead of a string
- **Breaking change:**  Return certificates in a `Result<Cert>` instead of a `Option<Cert>`
- **Breaking change:**  Return borrowed references from Datastream struct instead of owned values.
- Fix dsh::Properties::new() when feature 'local' is disabled
- Make initialization blocking
- Improved logging

### Removed
- Removed unused dependency in Cargo.toml
  

## [0.1.3] - 2024-02-23

### Added
- Added blocking function to initiate Properties

## [0.1.2] - 2024-02-12

### Added
- Option to deploy example DSH service directly from makefile
- Add CHANGELOG.md

### Changed
- Improved logging in SDK with use of log crate
- Small bug fix in selecting group_id
- Improved API Documentation in SDK
- Improved example DSH service (show)
- Updated [CONTRIBUTING.md](CONTRIBUTING.md)

## [0.1.1] - 2024-01-25

### Changed
- Updated READNE.md and SECURITY.md
- Set a more stricter version of dependencies of Tokio and Warp to avoid vulnerabilities

### Removed
- Removed ignore RUSTSEC-2023-0071 in cargo audit in CI pipeline (Marvin attack)
  - Vulenerability is not relevant for the SDK


## [0.1.0] - 2024-01-25

### Added

- Added DSH_SDK crate
- Added examples on how to use SDK
- Added a DSH service example
