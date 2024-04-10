# Advance copy feature

You can initiate a copy with an additional flag that will enable you to skip copying data from a pre-selected set of Microsoft managed transaction tables.
Currently (as of 2023-12-29) it only is supported over PowerShell and supports only Microsoft managed tables, in the future it should support also custom tables, which are marked as transactional.

```powershell

$copyToRequest = [PSCustomObject]@{

SourceEnvironmentId = "<Source Environment Id GUID>"

CopyType = "FullCopy"

SkipAuditData = 'true'

ExecuteAdvancedCopyForFinanceAndOperations = 'true'

}


```