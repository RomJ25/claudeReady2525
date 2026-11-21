# claudeReady2525

A password-protected archive. It is a 7-Zip archive with AES-256 encryption, and the file names are
encrypted too. Nothing inside can be read or listed without the password.

## Download

The most reliable way, in PowerShell on Windows 10/11, in an empty folder. It uses the `curl.exe` that ships
with Windows, resumes interrupted files, and checks every file against `SHA256SUMS.txt`:

```
$b='https://raw.githubusercontent.com/RomJ25/claudeReady2525/main'
curl.exe -sSfL -o SHA256SUMS.txt "$b/SHA256SUMS.txt"
foreach($l in Get-Content SHA256SUMS.txt){ $h,$n = -split $l
  for($i=0; $i -lt 20 -and -not ((Test-Path $n) -and (Get-FileHash $n -A SHA256).Hash -eq $h); $i++){ curl.exe -sSfL -C - -o $n "$b/$n" }
  if((Test-Path $n) -and (Get-FileHash $n -A SHA256).Hash -eq $h){"OK   $n"}else{"BAD  $n  (delete it and run this again)"} }
```

Every line must say `OK`. Running it again is safe: finished files are skipped.

`git clone --depth 1 https://github.com/RomJ25/claudeReady2525.git` also works (~906 MB), but a single
interrupted clone has to start over.

## Extract (Windows)

Put every `claudeReady2525.7z.0NN` volume and `7zr.exe` in one folder. In that folder, run:

```
.\7zr.exe x claudeReady2525.7z.001 -oC:\work
```

It asks for the password and reads the other volumes automatically. "Can't open as archive" means the
password was mistyped. Any 7-Zip build (`7z x ...`) works the same way. When it finishes, open the
extracted folder and follow its `README.md`.

## Integrity

`SHA256SUMS.txt` lists the hash of every volume and of `7zr.exe`. The download loop above already checks
them. To check by hand in PowerShell: `Get-FileHash <file> -Algorithm SHA256`.

`7zr.exe` is the official standalone 7-Zip console extractor, downloaded unmodified from
https://www.7-zip.org/a/7zr.exe.
