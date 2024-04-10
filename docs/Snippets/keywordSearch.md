---
tags: 
    - Snippet
    - X++
    - Prodware ISV
---
# Keyword Search
Dynamics 365 F&O has full-text search capabilitites in the "Sales and marketing" module. With this feature you can performe full-text search in sales order form, product form and sales quotation form. And the search is performed across multiple fields (can be set).
![Sales and marketing search menu](../screenshots/SalesAndMarketingSearchMenu.png)

Limitation of the search are that you can only cover some fields that are present in the inventtable and other tables. And the biggest drawback is that the search terms in the result have to be in the same order as requested/searched.
If you search for **Inverter Battery** you only get results where the order is **Inverter Battery**.

There is a table named **MCRInventTableIndex**, that consists of two fields. One as a reference to the product, and one field that is a concatenation of several fields. As shown under, the Searchtext field contains item number and item description, just concatenated together. Then there have been created a fulltext index on this field to speed up the search on this long string.

With a minor adjustment whe can split the search term with character ```,``` and performe a full-text search where the order of the keywords is not relevant.
As we use the query features, we are limited with the number of joins and every keyword is one join that needs to be performed, I limited the number of keywords relevant in the code to 20 but in theory 26 or so should be possible.

## Code
```xpp
internal final class InventSearchHelperPDE
{
    public static boolean addItemIdsToDS(MCRSearchText _searchText, Query _query)
    {

        Container keywords;
        int conCount;
        int numDataSource;
        _searchText = strReplace(_searchText, '%', '*');

        QueryBuildDataSource parentDataSource  = _query.addDataSource(tableNum(MCRInventTableIndex));

        QueryBuildDataSource qdbsInventDistinctProduct = parentDataSource.addDataSource(tableNum(InventDistinctProduct));
        qdbsInventDistinctProduct.addLink(fieldNum(MCRInventTableIndex,RefRecId),fieldNum(InventDistinctProduct, Product));

        qdbsInventDistinctProduct.joinMode(JoinMode::InnerJoin);

        keywords = str2con(_searchText, ",");

        boolean ret = false;

        while(conCount <= conLen(keywords))
        {
            conCount++;
            str tmpSearchString = conPeek(keywords, conCount);

            if(strLen(tmpSearchString) == 0)
            {
                continue;
            }

            if(numDataSource > 20)
            {
                break;
            }

            numDataSource++;

            QueryBuildDataSource qdbsSearch =  qdbsInventDistinctProduct.addDataSource(tableNum(MCRInventTableIndex));
            QueryBuildRange      qdRange = qdbsSearch.addRange(fieldNum(MCRInventTableIndex,SearchText));
            qdbsSearch.addLink(fieldNum(InventDistinctProduct, Product), fieldNum(MCRInventTableIndex, RefRecId));
            qdbsSearch.joinMode(JoinMode::ExistsJoin);
            
            if(MCRFullTextParameters::find().SearchType == MCRSearchMatchType::Full)
            {
                qdRange.rangeType(QueryRangeType::FullText);
                qdRange.value(SysQuery::value(tmpSearchString));
            }
            else
            {
                //TODO
                qdRange.value(SysQuery::valueLike(tmpSearchString));
            }
             
        }

        if(numDataSource > 1){
            ret = 1;
        }

        return ret;

    }
}
```


```xpp
[ExtensionOf(classStr(MCRInventSearch))]
final class MCRInventSearchPDE_Extension
{
    protected Query buildSearchQuery(MCRSearchText _mcrSearchText)
    {
        Query defaultSearchQuery, customSearchQuery;

        defaultSearchQuery = next buildSearchQuery(_mcrSearchText);
        customSearchQuery = MCRSearchQueryFactory::CreateQuery();

        QueryBuildDataSource qdbs = customSearchQuery.addDataSource(tableNum(InventDistinctProduct));

        boolean result = InventSearchHelperPDE::addItemIdsToDS(_mcrSearchText, customSearchQuery);
        if(result)
        {
            defaultSearchQuery = customSearchQuery;
        }

        return defaultSearchQuery;
    }
}
```
