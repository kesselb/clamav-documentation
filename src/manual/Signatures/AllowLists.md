# Allow list databases

## File allow lists

To allow a specific file use the MD5 signature format and place it inside a database file with the extension of `.fp` (for "false positive"). To allow a specific file with the SHA1 or SHA256 file hash signature format, place the signature inside a database file with the extension of `.sfp` (for "SHA false positive").

To generate FP or SFP signatures, try something like this...

MD5:

```bash
sigtool --md5 /path/to/false/positive/file >> /path/to/databases/false-positives.fp
```

SHA256:

```bash
sigtool --sha256 /path/to/false/positive/file >> /path/to/databases/false-positives.sfp
```

Here's an example adding the EICAR test file to an allow list by generating a sha256 false positive signature:

```bash
❯ clamscan ~/Downloads/eicar.com
/mnt/c/Users/micah/Downloads/eicar.com: Win.Test.EICAR_HDB-1 FOUND

...

❯ sigtool --sha256 ~/Downloads/eicar.com >> /var/lib/clamav/false-positives.sfp

❯ clamscan ~/Downloads/eicar.com
/mnt/c/Users/micah/Downloads/eicar.com: OK
...
```

## Signature ignore lists

To ignore a specific signature from the database you just add the signature name into a local file with the `.ign2` extension and store it inside the database directory.

E.g:

```
Eicar-Test-Signature
```

Additionally, you can follow the signature name with the MD5 of the entire database entry for this signature. In such a case, the signature will no longer be ignored when its entry in the database gets modified (eg. the signature gets updated to avoid false alerts). E.g:

```
Eicar-Test-Signature:bc356bae4c42f19a3de16e333ba3569c
```

Historically, signature ignores were added to `.ign` files. This format is still functional, though it has been replaced by the `.ign2` database.
