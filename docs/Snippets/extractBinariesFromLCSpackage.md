---
tags: 
    - Unified Experience
    - Snippet
---
# Extract binary folders from ZIPs

With the new [Unified Experience](../Documentation/Unified%20Experience/index.md) developers are going to need the binaries from the ISV solutions on their dev machines. 
That can be for example accomplished with unzipping the software deployable package and using the following PowerShell script to extract every binary zip to the reference package folder and renaming the folder to the respective model name.

```ps1
## this refers to your custom metadata directory

$PLD="C:\Git\CustomPackage\bin"

## this refers to the binary package you want to extract

$PKG = "C:\Git\CustomPackage\AOSPackages"

$FILES=(Get-ChildItem $PKG)

foreach ($file in $FILES) {

$folder=$file.Name.Replace('dynamicsax-', '')

$folder=$folder.Split('.',2)[0]

write-output "mkdir $PLD/$folder"

New-Item -Type directory -Path "$PLD/$folder"

write-output "unzip $PKG\$file"

Expand-Archive -Path $PKG\$file -DestinationPath $PLD/$folder

}

```