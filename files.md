# File Management

## Versions

For each asset we look after there will be one master version and at least one derived version for display on our website. Depending on the media type there may be others to allow for subtitles etc. Each file is identified by its SHA1 hash which can be used to connect the master file back to our records.

### Naming convention

| Version  | Format   | Example                                      |
| -------- | -------- | -------------------------------------------- |
| Master   | {sha1}   | `bd17dabf6fdd24dab5ed0e2e6624d312e4ebeaba`   |
| Web      | w-{sha1} | `w-bd17dabf6fdd24dab5ed0e2e6624d312e4ebeaba` |
| Captions | c-{sha1} | `c-bd17dabf6fdd24dab5ed0e2e6624d312e4ebeaba` |

### File Storage

Wherever we store our files, locally on a hard drive, on a file server, or in the cloud using something like AWS S3, we maintain the same folder structure. This allows us to sync them between locations and retain the same logic.
Files are stored within subdirectories that are based on the file's hash. This avoids having too many files in one directory which can cause issues with certain file systems. The archivebot does this automatically. The structure is as follows - the first 2 characters make up the first folder and the file is then stored within that as above.
`bd17dabf6fdd24dab5ed0e2e6624d312e4ebeaba` would be stored as `bd/bd17dabf6fdd24dab5ed0e2e6624d312e4ebeaba`
