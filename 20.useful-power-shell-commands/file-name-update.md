#### replace the first occurrence of . with - in all filenames inside the folder
```powershell
$folder = "C:\Users\venka\github-repos\sql-server-training\MS-SQL\00.basics"
Get-ChildItem -Path $folder -File | ForEach-Object {
    $newName = $_.Name -replace '^(.*?)\.', '$1-'
    if ($newName -ne $_.Name) {
        Rename-Item -Path $_.FullName -NewName $newName -Force
    }
}
```
