# claudeReady2525

A password-protected archive. It is a 7-Zip archive with AES-256 encryption, and the file names are
encrypted too. Nothing inside can be read or listed without the password.

## Download

`git clone https://github.com/RomJ25/claudeReady2525.git` fetches everything (~906 MB). If the clone stops
with `early EOF` or a timeout, fetch the files one at a time instead. In PowerShell, using the `curl.exe`
that ships with Windows 10/11 (each retry resumes where the last one stopped):

```
$b='https://raw.githubusercontent.com/RomJ25/claudeReady2525/main'
$f=@('7zr.exe','SHA256SUMS.txt') + (1..10 | % { 'claudeReady2525.7z.{0:D3}' -f $_ })
foreach($n in $f){ for($i=0;$i -lt 20;$i++){ curl.exe -sSfL -C - -o $n "$b/$n"; if($LASTEXITCODE -eq 0){break} } }
```

## Extract (Windows)

Put every `claudeReady2525.7z.0NN` volume and `7zr.exe` in one folder, then run:

```
7zr.exe x claudeReady2525.7z.001 -oC:\work
```

It asks for the password and reads the other volumes automatically. Any 7-Zip build (`7z x ...`) works the
same way. When it finishes, open the extracted folder and follow its `README.md`.

## Integrity

`SHA256SUMS.txt` lists the hash of every volume and of `7zr.exe`. Check them in PowerShell with
`Get-FileHash <file> -Algorithm SHA256`.

`7zr.exe` is the official standalone 7-Zip console extractor, downloaded unmodified from
https://www.7-zip.org/a/7zr.exe.
