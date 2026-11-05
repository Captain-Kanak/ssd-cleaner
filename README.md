## Script 1 — Scan C: drive

```sh
Get-ChildItem C:\ -Force -Directory -ErrorAction SilentlyContinue |
ForEach-Object {
    $size = (Get-ChildItem $_.FullName -Force -File -Recurse -ErrorAction SilentlyContinue |
        Measure-Object Length -Sum).Sum

    [PSCustomObject]@{
        Folder = $_.FullName
        SizeGB = [math]::Round($size / 1GB, 2)
    }
} |
Sort-Object SizeGB -Descending |
Select-Object -First 20
```

## Script B — Scan any specific directory

```sh
$Path = "C:\Users\welcome\AppData\Local"

Get-ChildItem $Path -Force -Directory -ErrorAction SilentlyContinue |
ForEach-Object {
    $size = (Get-ChildItem $_.FullName -Force -File -Recurse -ErrorAction SilentlyContinue |
        Measure-Object Length -Sum).Sum

    [PSCustomObject]@{
        Folder = $_.FullName
        SizeGB = [math]::Round($size / 1GB, 2)
    }
} |
Sort-Object SizeGB -Descending
```

## Script C — Find the largest individual files

```sh
$Path = "C:\Users\welcome"

Get-ChildItem $Path -Force -File -Recurse -ErrorAction SilentlyContinue |
Sort-Object Length -Descending |
Select-Object -First 30 @{
    Name = "File"
    Expression = { $_.FullName }
}, @{
    Name = "SizeGB"
    Expression = { [math]::Round($_.Length / 1GB, 2) }
}
```
