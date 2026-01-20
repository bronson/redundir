# redundir

A command-line utility that finds directories containing duplicate files and ranks them by redundancy.

## Installation

```sh
curl -o ~/.local/bin/redundir https://raw.githubusercontent.com/USER/redundirs/main/redundir
chmod +x ~/.local/bin/redundir
```

Or just copy `redundir` anywhere in your `$PATH`. Requires Python 3.10+ with no external dependencies.

## Usage

```
redundir [directory] [-a ALGORITHM] [-j N] [-v] [-q]
```

| Option | Description |
|--------|-------------|
| `directory` | Directory to scan (default: `.`) |
| `-a, --algorithm` | Hash algorithm: `md5`, `sha1`, `sha256`, `blake2b`, `blake2s` (default: `blake2b`) |
| `-j, --jobs` | Number of parallel hashing jobs (default: `4`, use `1` to disable) |
| `-v, --verbose` | Show scan progress |
| `-q, --quiet` | Suppress status messages |

## Example

```
$ redundir ~/Documents
Scanning /home/user/Documents...
100.00% (4/4) /home/user/Documents/backups/old
 75.00% (3/4) /home/user/Documents/photos/2023
 66.67% (2/3) /home/user/Documents/projects/archive

Found 3 directories with duplicate files.
```

## How It Works

1. Recursively scans all files and groups them by size (fast, no hashing needed)
2. Only hashes files that have potential duplicates (same size as another file)
3. Uses parallel processing (4 workers by default) to hash files quickly
4. Identifies duplicates (files with identical content anywhere in the tree)
5. Calculates each directory's **redundancy score**: `duplicate_files / total_files`
6. Outputs directories sorted by score (most redundant first)

**Performance optimizations:**
- Size-based pre-filtering skips hashing files with unique sizes
- BLAKE2b hashing is faster than SHA256
- Parallel processing utilizes multiple CPU cores
- 8MB chunk size for efficient I/O

## License

MIT

## Maybe todo?

* Probably need to be smarter about handling symlinks.
* It's not hardened against malicious content.
* Do I need to worry about filesystem boundaries? Probably not?
