# First Find

- **Platform:** picoCTF (author: LT 'syreal' Jones)
- **Difficulty:** Easy
- **Date completed:** 2026-10-15
- **Category:** General Skills

## Summary
A basic intro to the `find` command — a zip archive contains a deeply nested, partially hidden directory tree, and the target file (`uber-secret.txt`) is buried several layers down among decoy folders. Rather than manually navigating folder by folder, `find` locates it in one command.

## Steps
1. Downloaded and unzipped the provided archive.
2. Used `find` to recursively search the extracted directory for the target filename:
   ```
   find . -name "uber-secret.txt"
   ```
3. This returned the full path in one shot:
   ```
   ./adequate_books/more_books/.secret/deeper_secrets/deepest_secrets/uber-secret.txt
   ```
   (Note: the directory path includes a hidden folder, `.secret`, which `find` still traverses by default even though it wouldn't show up in a plain `ls`.)
4. `cd`'d into that path and read the file directly:
   ```
   cat uber-secret.txt
   ```

## Tools used
- `find` — recursively searched the filesystem by filename, skipping the need to manually walk through nested/hidden directories
- `cat` — read the file's contents once located

## Lesson learned
`find -name` searches recursively by default and doesn't skip hidden (dot-prefixed) directories the way a plain `ls` would visually hide them — it's the right tool when you know (or can guess) a filename but not its location, especially inside deliberately obfuscated directory trees.

## Flag
```
academy{f1nd_15_f457_ab443fd1}
```