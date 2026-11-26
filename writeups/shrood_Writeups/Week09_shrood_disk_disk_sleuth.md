# Disk, disk, sleuth! II

**Platform:** CyLab Security Academy (picoCTF 2021)
**Category:** Forensics
**Difficulty:** Medium
**Author:** syreal
**Date completed:** 2026-11-26

## Challenge
Given a disk image (`dds2-alpine.flag.img.gz`), find the flag inside a file named `down-at-the-bottom.txt`.

## Steps
1. Downloaded and extracted the image: `wget` → `gunzip dds2-alpine.flag.img.gz`.
2. Ran `mmls dds2-alpine.flag.img` to read the partition table (media layer) and find where the real filesystem starts:
   ```
   002:  000:000   0000002048   0000262143   0000260096   Linux (0x83)
   ```
   Only one real partition — offset `2048` sectors.
3. Tried `fls -o 2048 dds2-alpine.flag.img` (root listing only) — came back empty, meaning the file wasn't sitting directly in the root directory.
4. Switched to a recursive listing to search the whole directory tree at once instead of manually stepping into each subdirectory one inode at a time:
   ```
   fls -r -o 2048 dds2-alpine.flag.img | grep txt
   ```
   → `+ r/r 18291:    down-at-the-bottom.txt`
5. Tried `fls -o 2048 dds2-alpine.flag.img 18291` — errored (`Error extracting file from image: Error reading directory contents`), because `18291` is a **file** inode, not a directory — `fls` expects a directory inode to list contents of; pointing it at a file inode doesn't work.
6. Switched to the correct tool for pulling a file by inode number — `icat` (inode layer) instead of `fls` (filename layer):
   ```
   icat -o 2048 dds2-alpine.flag.img 18291
   ```
   This dumped the file content directly.
7. The file's contents were the flag written out as ASCII art, one boxed letter at a time (`(a) (c) (a) (d) (e) (m) (y) ({) ...`). Read the letters inside the boxes in order to reconstruct the flag.

## Mistakes along the way
- Tried `fls -o 2048 dds2-alpine.flag.img 18291` to "look inside" the file the same way you'd step into a subdirectory — `fls` only lists directory contents, it can't read/display a file's actual data, and a plain file inode has no directory listing to produce.

## Tools used
- `wget`, `gunzip`, `mmls`, `fls` (`-r` recursive mode), `icat`

## Lesson learned
- `fls` without `-r` only lists one directory level at a time (root, by default) — if the target file isn't directly visible, recurse (`-r`) and `grep` instead of manually walking into every subdirectory inode by inode.
- `fls` and `icat` have different jobs: `fls` lists filenames/directories (filename layer), `icat` extracts a file's actual content by inode number (inode layer). Pointing `fls` at a file inode (instead of a directory inode) fails — that's `icat`'s job.
- A "flag file" doesn't have to contain the flag as plain text — here it was ASCII art spelling it out letter by letter, so always actually read file output carefully rather than grepping for `academy{` and assuming nothing's there.

## Flag
`academy{f0r3ns1c4t0r_n0v1c3_95c645ad}`