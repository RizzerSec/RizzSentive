# RizzSentive

**File Sensitive Exposing**

RizzSentive is a powerful and lightweight Go-based command-line tool designed to detect sensitive information in files and directories. It scans for sensitive keywords, secrets, and patterns (e.g., API keys, passwords, JWT tokens) that may be exposed in source code, configuration files, or other text-based files. With a focus on simplicity, performance, and configurability, RizzSentive is ideal for developers, security researchers, and DevOps professionals to identify and mitigate potential security risks in their projects.

## Features

- **Sensitive Information Detection**: Identifies hardcoded secrets, API keys, passwords, and other sensitive data using keyword and regex-based scanning.
- **File and Directory Scanning**: Supports scanning individual files (`-file`) or directories (`-dir`), including wildcard patterns (e.g., `-dir *`).
- **Customizable Output**: Outputs results in a concise format (`[date] [file_path] [keyword]`) with color-coded severity levels or JSON for programmatic use.
- **High Performance**: Utilizes concurrent workers and buffered channels for fast scanning of large directories.
- **Extensive Configuration**:
  - Filter by severity, keyword, or file extension.
  - Exclude specific directories, files, or patterns.
  - Support for custom keyword lists via JSON files.
  - Options to ignore comments, scan hidden files, or follow symbolic links.
- **Cross-Platform**: Written in Go, ensuring compatibility across Windows, macOS, and Linux.
- **Apache-2.0 Licensed**: Open-source and freely available for use and contribution.

## Installation

### Prerequisites
- [Go](https://golang.org/dl/) (version 1.16 or higher) for building from source.
- A terminal that supports ANSI color codes for colored output (optional).

### Build from Source
1. Clone the repository:
   ```bash
   git clone https://github.com/RizzerSec/RizzSentive.git
   cd RizzSentive
   ```
2. Build the binary:
   ```bash
   go build -o RizzSentive main.go
   ```
3. Move the binary to a directory in your PATH (optional):
   ```bash
   mv RizzSentive /usr/local/bin/
   ```

### Precompiled Binary
Check the [Releases](https://github.com/RizzerSec/RizzSentive/releases) page for precompiled binaries (coming soon).

## Usage

RizzSentive can scan individual files or directories, with flexible options to customize the scanning process.

### Basic Commands
- **Scan a single file**:
  ```bash
  ./RizzSentive -file text.txt
  ```
  Output example:
  ```
  [ 2025-08-08 19:20:05 ] [ text.txt ] [ password ]
  ```

- **Scan all directories (wildcard)**:
  ```bash
  ./RizzSentive -dir *
  ```

- **Scan a specific directory**:
  ```bash
  ./RizzSentive -dir ./my_project
  ```

### Advanced Options
- **Filter by severity**:
  ```bash
  ./RizzSentive -dir ./my_project -severity high
  ```
- **Output to JSON**:
  ```bash
  ./RizzSentive -file config.yaml -json -o results.json
  ```
- **Exclude directories or files**:
  ```bash
  ./RizzSentive -dir * -exclude-dir node_modules,build -exclude-file secrets.txt
  ```
- **Use custom keywords**:
  Create a `keywords.json` file:
  ```json
  {
    "my_key": "high",
    "my_secret": "critical"
  }
  ```
  Then run:
  ```bash
  ./RizzSentive -dir ./my_project -update-keywords keywords.json
  ```
- **Verbose mode with statistics**:
  ```bash
  ./RizzSentive -file text.txt -v -stats
  ```

### Full Flag List
| Flag                | Description                                           | Default        |
|---------------------|-------------------------------------------------------|----------------|
| `-file`             | File(s) to scan (comma-separated)                    | ""             |
| `-dir`              | Directory to scan (supports wildcards)                | "."            |
| `-workers`          | Number of concurrent workers                          | 4              |
| `-o`                | Output file path                                      | ""             |
| `-json`             | Output in JSON format                                 | false          |
| `-severity`         | Filter by severity (low, medium, high, critical)      | ""             |
| `-ext`              | Filter by file extensions (comma-separated)           | ""             |
| `-keyword`          | Filter by keyword (comma-separated)                   | ""             |
| `-regex-only`       | Scan using regex patterns only                        | false          |
| `-update-keywords`  | Path to custom keywords JSON file                     | ""             |
| `-exclude-dir`      | Exclude directories (comma-separated)                 | ""             |
| `-exclude-file`     | Exclude files (comma-separated)                       | ""             |
| `-silent`           | Suppress console output                               | false          |
| `-v`                | Enable verbose logging                                | false          |
| `-timeout`          | Timeout in seconds (0 for no timeout)                 | 0              |
| `-max-size`         | Max file size in bytes                                | 10485760 (10MB)|
| `-follow-symlinks`  | Follow symbolic links                                 | false          |
| `-case-sensitive`   | Case-sensitive keyword matching                       | false          |
| `-hidden`           | Scan hidden files and directories                     | false          |
| `-exclude-pattern`  | Exclude regex patterns (comma-separated)              | ""             |
| `-depth`            | Max directory depth (0 for unlimited)                 | 0              |
| `-count`            | Only count findings without details                   | false          |
| `-rate-limit`       | Rate limit (files/sec, 0 for no limit)                | 0              |
| `-ignore-comments`  | Ignore code comments during scanning                  | false          |
| `-stats`            | Display scan statistics                               | false          |

## Output Format
- **Console Output**: Displays findings in the format `[date] [file_path] [keyword]`, with colors:
  - `date`: Blue
  - `file_path`: Green
  - `keyword`: Severity-based (critical: red, high: magenta, medium: yellow, low: cyan)
  - Brackets: White
  Example:
  ```
  [ 2025-08-08 19:20:05 ] [ test_dir/config.yaml ] [ api_key ]
  ```

- **JSON Output**: Includes full details (date, file path, keyword, line number, line content, severity) when using `-json`.

## Example
Create a test file:
```bash
mkdir test_dir
echo "password=secret123" > test_dir/text.txt
echo "api_key=abc123" > test_dir/config.yaml
```

Scan:
```bash
./RizzSentive -file test_dir/text.txt
```

Output:
```
[ 2025-08-08 19:20:05 ] [ test_dir/text.txt ] [ password ]
```

## Contributing
Contributions are welcome! Please follow these steps:
1. Fork the repository.
2. Create a feature branch (`git checkout -b feature/your-feature`).
3. Commit your changes (`git commit -m "Add your feature"`).
4. Push to the branch (`git push origin feature/your-feature`).
5. Open a pull request.

## License
This project is licensed under the [Apache License 2.0](LICENSE). See the [LICENSE](LICENSE) file for details.

## Contact
For issues, suggestions, or questions, please open an issue on the [GitHub repository](https://github.com/RizzerSec/RizzSentive) or contact the maintainer at [RizzerSec](https://github.com/RizzerSec).

## Acknowledgments
- Built with [Go](https://golang.org/) for performance and portability.
- Inspired by the need to secure sensitive data in codebases.

---
© 2025 RizzerSec. All rights reserved.