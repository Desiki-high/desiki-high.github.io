# Rust

## 为什么会出现Rust

```rust
#![deny(warnings)]

#[macro_use]
extern crate log;
#[macro_use]
extern crate lazy_static;
#[macro_use]
extern crate nydus_error;

use std::convert::TryInto;
use std::io::{Error, ErrorKind, Result};
use std::sync::Arc;

use clap::{Arg, ArgAction, ArgMatches, Command};
use nix::sys::signal;
use rlimit::Resource;

use nydus::{get_build_time_info, SubCmdArgs};
use nydus_api::BuildTimeInfo;
use nydus_app::{dump_program_info, setup_logging};
use nydus_service::daemon::DaemonController;
use nydus_service::{
    create_daemon, create_fuse_daemon, validate_threads_configuration, Error as NydusError,
    FsBackendMountCmd, FsBackendType, ServiceArgs,
};

use crate::api_server_glue::ApiServerController;

#[cfg(feature = "virtiofs")]
mod virtiofs;

mod api_server_glue;

/// Minimal number of file descriptors reserved for system.
const RLIMIT_NOFILE_RESERVED: u64 = 16384;
/// Default number of file descriptors.
const RLIMIT_NOFILE_MAX: u64 = 1_000_000;

lazy_static! {
    static ref DAEMON_CONTROLLER: DaemonController = DaemonController::new();
    static ref BTI_STRING: String = get_build_time_info().0;
    static ref BTI: BuildTimeInfo = get_build_time_info().1;
}

fn thread_validator(v: &str) -> std::result::Result<String, String> {
    validate_threads_configuration(v).map(|s| s.to_string())
}
```