With the cli tool burn we can search for information on dpg brands.
It's built on the hackathon (15/03/2023) organized by the data engineering guild.

### What do we want?
```
burn --help
burn --version
burn --output csv|json
burn --select app_id,source_system_id,brand_short
burn --select app_id,brand_short --where platform=selectives
burn --limit 5
burn usage
```
### How to organize ourselves?
In order to learn the Rust basics, we want to work individually on a feature.
If one team member is ready, the person gives a demo and we do a code review.
This should take 5-10 minutes.
Afterwards, that person could join another individual or group who picked up another feature.
In the end we will end up in one big group working on the last feature and glue everything together.

### Links to read or do (optional)
- [CLI Guidelines](https://clig.dev/)
- [Rust CLI tutorial](https://rust-cli.github.io/book/tutorial/index.html)
- [Rust tutorial](https://learn.microsoft.com/en-us/training/paths/rust-first-steps/)

### How do I get set up? ###

* Install [Rust](https://www.rust-lang.org/tools/install)
* Clone this repo

To run the app:
```
cargo build
target/debug/burn
```
Optionally link target/debug/burn to your path.

### Features

#### Output formats
User should be able to request different output formats. For example:
```
burn --output csv
```
Should return brand information in csv format.
Let's provide json and csv.

#### Query processing
The user should be able to select a limited set of fields and apply filters. For example:
```
burn --select app_id,source_system_id,long_name --where platform=selectives&&brandShort=AD
```
Should only return three fields: app_id, source_system_id and the brand long name.
And only for the platform selectives and brand short X.
Another feature is the ability to limit the result set.

#### Parsing and documentation of arguments
Obviously the cli tool needs to be able to parse the arguments.

#### Telemetry
We would like to know how users use burn.
The tool should store usage stats locally on the user's machine.
The user should be able to retrieve this information.
Make sure each command emits an event (can we add it #[derive(TelemetryEmit)] ???).
Finally make sure the event gets pushed to the telemetry server.

Things to be tracked:

- platform (Mac, Linux...)
- command run (command and success/fail)
- application version (burn version)

#### Telemetry Server
[Rust WASM](https://www.rust-lang.org/what/wasm)

Create a web server with an endpoint that accepts events emitted from the cli. 
Provide a display page that summarizes all events, grouped by platform, command and state (SUCCESS or FAIL).
<!---
THIS IS WHERE THE REAL DOCS START 
--->
# Overview 
`burn` is a command line tool which allows you to quickly get relevant mappings for a given DPG brand, without having to
 leave your terminal.

# Installation

# Usage
```
Usage: burn [OPTIONS]

Options:
  -s, --select <SELECT>      Select list of fields separated by comma (e.g. burn -s field1,field2,field3)
  -o, --output <OUTPUT>      Output filepath, stdout if not present
  -f, --filetype <FILETYPE>  Output filetype [possible values: csv, json]
  -w, --where <FILTERS>      Where clause to filter the select by. Takes key=value pairs like so: key1=value1&key2=value2
  -l, --limit <LIMIT>        Limit for the number of results to return
  -u, --usage                Show usage data
  -h, --help                 Print help
```

Example query:
```
burn -s brand1,someotherbrand -o output_file.csv -f csv -w "some_col=hallo&smthgelse=1" -l 100
```
