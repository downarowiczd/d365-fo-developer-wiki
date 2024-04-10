---
tags: 
    - X++
    - Snippet
---
# Factbox visibility

PartList should not be used anymore as is marked for ***InternalUseOnly*** by Microsoft.

```xpp
internal static void toggleFormPartVisibility(FormRun _formRun, str _formStr, boolean _isVisible = false)
{
    //PartList                partList;
    //partList = new PartList(_formRun);

    if(_formRun.getPartControlByName(_formStr) != null)
    {
        _formRun.getPartControlByName(_formStr).visible(_isVisible);
    }

    //if(partList.getPartControlByFormName(_formStr) != null)
    //{
    //    partList.getPartControlByFormName(_formStr).visible(_isVisible);
    //}
}
```