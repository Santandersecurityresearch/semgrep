<p align="center">
    <img src="semgrep.svg" height="100" alt="Semgrep logo"/>
</p>
<h3 align="center">
  Lightweight static analysis for many languages.
  </br>
  Find and block bug variants with rules that look like source code.
</h3>

<p align="center">
  <a href="#getting-started">Getting Started</a>
  <span> · </span>
  <a href="#Examples">Examples</a>
  <span> · </span>
  <a href="#resources">Resources</a>
  <br/>
  <a href="#usage">Usage</a>
  <span> · </span>
  <a href="#contributing">Contributing</a>
  <span> · </span>
  <a href="#commercial-support">Commercial Support</a>
</p>

<p align="center">
  <a href="https://formulae.brew.sh/formula/semgrep">
    <img src="https://img.shields.io/homebrew/v/semgrep?style=flat-square" alt="Homebrew" />
  </a>
  <a href="https://pypi.org/project/semgrep/">
    <img alt="PyPI" src="https://img.shields.io/pypi/v/semgrep?style=flat-square&color=blue">
  </a>
  <a href="https://r2c.dev/slack">
    <img src="https://img.shields.io/badge/slack-join-green?style=flat-square" alt="Issues welcome!" />
  </a>
  <a href="https://github.com/returntocorp/semgrep/issues/new/choose">
    <img src="https://img.shields.io/badge/issues-welcome-green?style=flat-square" alt="Issues welcome!" />
  </a>
  <a href="https://github.com/returntocorp/semgrep#readme">
    <img src="https://img.shields.io/github/stars/returntocorp/semgrep?label=GitHub%20Stars&style=flat-square" alt="1000+ GitHub stars" />
  </a>
</p>

Semgrep tl;dr:

- A simple, customizable, and fast static analysis tool for finding bugs
- Combines the speed and customization of `grep` with the precision of traditional static analysis tools
- No painful domain-specific language; patterns look like the source code you’re targeting
- Batteries included with hundreds of existing community rules for OWASP 10 issues and common mistakes
- Run it in CI, at pre-commit, or in the editor
- Runs offline on uncompiled code

Semgrep supports:

- C (beta)
- Go
- Java
- JavaScript
- JSON
- OCaml
- Python
- Ruby (beta)

Semgrep is proudly supported by r2c. Learn more about a hosted version of Semgrep with an enterprise feature set at [r2c.dev](https://r2c.dev/).

## Getting Started

The best place to start with Semgrep is its [Quick Start](https://semgrep.dev/editor). For a more in-depth introduction, see the [Semgrep Tutorial](https://semgrep.dev/learn).

Semgrep can be installed using `brew`, `pip`, or `docker`:

```sh
# For macOS
$ brew install semgrep

# On Ubuntu/WSL/linux, we recommend installing via `pip`
$ pip3 install semgrep

# To try Semgrep without installation run via Docker
$ docker run --rm -v "${PWD}:/src" returntocorp/semgrep --help
```

Once installed, Semgrep can be run with single patterns or entire rule packs:

```sh
# Check for Python == where the left and right hand sides are the same (often a bug)
$ semgrep -e `$X==$X` --lang=py path/to/src

# Run the default rule pack with rules for many languages
$ semgrep --config=default path/to/src
```

Explore the Semgrep Registry of rules and CI integrations at [semgrep.dev](https://semgrep.dev/packs).

## Examples

### Core Uses

- Ban dangerous APIs [[1](https://semgrep.live/clintgibler:no-exec)]
- Search routes and authentiation [[1](https://semgrep.live/clintgibler:spring-routes)]
- Enforce the use secure defaults [[1](https://semgrep.dev/dlukeomalley:flask-set-cookie)]
- Enforce project best-practices [[1](https://semgrep.dev/dlukeomalley:use-assertEqual-for-equality), [2](https://semgrep.dev/dlukeomalley:unchecked-subprocess-call)]
- Codify project-specific knowledge [[1](https://semgrep.dev/dlukeomalley:verify-before-make)]
- Audit security hotspots [[1](https://semgrep.live/ievans:airflow-xss), [2](https://semgrep.dev/dlukeomalley:hardcoded-credentials)]
- Audit configuration files [[1](https://semgrep.dev/dlukeomalley:s3-arn-use)]

### Upgrading Code

- Migrate from deprecated APIs [[1](https://semgrep.dev/editor?registry=java.lang.security.audit.crypto.des-is-deprecated), [2](https://semgrep.dev/editor?registry=python.bokeh.maintainability.deprecated.deprecated_apis), [3](https://semgrep.dev/editor?registry=python.flask.maintainability.deprecated.deprecated-apis)]
- Apply automatic fixes [[1](https://semgrep.live/clintgibler:use-listenAndServeTLS), [2]()]

## Resources

Learn more:

- [Live editor](https://semgrep.dev/editor)
- [Community rule registry](https://semgrep.dev/r)
- [Documentation](docs/README.md)
- [r2c YouTube channel](https://www.youtube.com/channel/UC5ahcFBorwzUTqPipFhjkWg) for more videos.

Get in touch:

- Submit a [bug report](https://github.com/returntocorp/semgrep/issues)
- Join the [Semgrep Slack](https://join.slack.com/t/r2c-community/shared_invite/enQtNjU0NDYzMjAwODY4LWE3NTg1MGNhYTAwMzk5ZGRhMjQ2MzVhNGJiZjI1ZWQ0NjQ2YWI4ZGY3OGViMGJjNzA4ODQ3MjEzOWExNjZlNTA) to say "hi" or ask questions

## Usage

### Command Line Options

```shell
$ semgrep --help

positional arguments:
  target                Search these files or directories.
                        Defaults to  entire current working directory.
                        Implied argument if piping to semgrep.

optional arguments:
  -h, --help            show this help message and exit
  --exclude EXCLUDE     Skip any file with this name; --exclude='*.py'
                        will ignore foo.py as well as src/foo.py.
                        Can add multiple times. Overrides includes.
  --include INCLUDE     Scan only files with this name,
                        such as --include='*.jsx'. Can add multiple times.
  --version             Show the version and exit.

config:
  -f CONFIG, --config CONFIG
                        YAML configuration file, directory of YAML files
                        ending in .yml|.yaml, URL of a configuration file,
                        or semgrep registry entry name. See README for
                        information on configuration file format.
  -e PATTERN, --pattern PATTERN
                        Code search pattern. See README for information
                        on pattern features.
  -l LANG, --lang LANG  Parse pattern and all files in specified language.
                        Must be used with -e/--pattern.
  --validate            Validate configuration file(s). No search is performed.
  --strict              Only invoke semgrep if configuration files(s) are valid.
  --dangerously-allow-arbitrary-code-execution-from-rules
                        WARNING: allow rules to run arbitrary code.
                        ONLY ENABLE IF YOU TRUST THE SOURCE OF ALL RULES IN YOUR CONFIGURATION.
  -j JOBS, --jobs JOBS  Number of subprocesses to use to run checks in parallel.
                        Defaults to the number of CPUs on the system.

output:
  -q, --quiet           Do not print anything to stdout. Results can be
                        saved to an output file via -o/--output. Exit
                        code provides success status.
  --no-rewrite-rule-ids
                        Do not rewrite rule ids when they appear in nested
                        sub-directories (by default, rule 'foo' in
                        test/rules.yaml will be renamed 'test.foo').
  -o OUTPUT, --output OUTPUT
                        Save search results to a file or post to URL.
                        Default is to print to stdout.
  --json                Output results in JSON format.
  --debugging-json      Output JSON with extra debugging information.
  --sarif               Output results in SARIF format.
  --test                Run test suite.
  --dump-ast            Show AST of the input file or passed expression and
                        then exit (can use --json).
  --error               Exit 1 if there are findings. Useful for CI and scripts.
  -a, --autofix         Apply the autofix patches. WARNING: data loss can
                        occur with this flag. Make sure your files are stored
                        in a version control system.

logging:
  -v, --verbose         Set the logging level to verbose.
                        E.g. statements about which files are being processed
                        will be printed.
```

### Exit Codes

`semgrep` may exit with the following exit codes:

- `0`: Semgrep ran successfully and found no errors
- `1`: Semgrep ran successfully and found issues in your code
- \>=`2`: Semgrep failed to run

## Contributing

Semgrep is LGPL-licensed, feel free to help out: [CONTRIBUTING](https://github.com/returntocorp/semgrep/blob/develop/CONTRIBUTING.md).

Semgrep is a frontend to a larger program analysis library named [`pfff`](https://github.com/returntocorp/pfff/). `pfff` began and was open-sourced at [Facebook](https://github.com/facebookarchive/pfff) but is now archived. The primary maintainer now works at [r2c](https://r2c.dev). Semgrep was originally named `sgrep` and was renamed to avoid collisons with existing projects.

## Commercial Support

Semgrep is supported by [r2c](https://r2c.dev). We're hiring!

Interested in a fully-supported, hosted version of semgrep? [Drop your email](https://forms.gle/dpUUvSo1WtELL8DW6) and we'll be in touch!
