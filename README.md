# VSFSck - Very Simple File System Check

VSFSck is a filesystem integrity checking and repair tool for the Very Simple File System (VSFS). It verifies the filesystem structure, detects errors, and can automatically fix common problems to maintain data integrity.

## Overview

This utility performs comprehensive verification of VSFS filesystem images, including:

- Validation of superblock parameters
- Verification of inode and data block bitmaps
- Detection and repair of invalid block pointers
- Identification and correction of duplicate block usage
- Consistency checking between bitmaps and actual usage

## VSFS Filesystem Structure

VSFS is a simple filesystem with the following structure:

- Block size: 4096 bytes
- Total blocks: 64
- Inode size: 256 bytes
- Inode count: 80
- Layout:
  - Block 0: Superblock
  - Block 1: Inode bitmap
  - Block 2: Data bitmap
  - Blocks 3-7: Inode table
  - Blocks 8-63: Data blocks

## Building

Compile the program with:

```bash
gcc -o vsfsck vsfsck.c
```

## Usage

```bash
./vsfsck <filesystem_image>
```

Where `<filesystem_image>` is the path to a VSFS filesystem image file.

## How It Works

VSFSck performs a two-pass check of the filesystem:

1. **First Pass**: Checks and fixes any errors found in the filesystem
2. **Second Pass**: Verifies that the filesystem is now consistent

### Error Detection and Repair

The tool detects and repairs several types of errors:

- Invalid superblock parameters
- Inconsistencies between inode bitmap and actual inode usage
- Inconsistencies between data bitmap and actual block usage
- Invalid block pointers (out of range)
- Duplicate block usage

## Log File

VSFSck creates a log file (`vsfsck_log.txt`) that records all actions taken, including:
- Errors detected
- Repairs made
- Summary of filesystem state

## Example Output

```
PASS 1: Checking and fixing file system errors
Validating Superblock...
Superblock is valid.
Superblock validation complete.
Checking Inode Bitmap...
Inode bitmap is consistent.
Inode Bitmap check complete.
Checking Data Bitmap...
Error: Data block 10 is marked in bitmap but not used.
Fixing: Marking data block 10 as unused in bitmap.
Bitmap updated on disk.
No bad blocks found.
Data bitmap has 1 inconsistencies.
Data Bitmap check complete.
Checking for duplicate blocks...
No duplicate blocks found.
Duplicate block check complete.
PASS 1: Found and fixed 1 errors

PASS 2: Verifying file system integrity after fixes
Validating Superblock...
Superblock is valid.
Superblock validation complete.
Checking Inode Bitmap...
Inode bitmap is consistent.
Inode Bitmap check complete.
Checking Data Bitmap...
No bad blocks found.
Data bitmap is consistent.
Data Bitmap check complete.
Checking for duplicate blocks...
No duplicate blocks found.
Duplicate block check complete.
PASS 2: File system is now consistent
---- VSFSck completed ----
```

## Technical Details

VSFSck implements the following key features:

- **Block Pointer Validation**: Checks direct, single-indirect, double-indirect, and triple-indirect block pointers
- **Bitmap Consistency**: Ensures that bitmap states match actual usage
- **Duplicate Block Detection**: Identifies blocks referenced by multiple inodes or pointers
- **Error Correction**: Automatically repairs common errors while maintaining filesystem integrity

## License

[Add your preferred license here]

## Author

[Your name/username]
