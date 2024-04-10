---
tags: 
    - D365 F&O
    - X++
---
# Link types - FormDataSource

Form data source link type is a property of the form data source. We can add more than one tables as a data source to the form. Those data sources should have the table level relation.
So, then the developer does not need to work on the coding part to find the relation records. For example, if we create the order form, that order form has orders and order details tables as form datasources. We can add both tables as a data sources to the form.

The parent table and child table should have the table relation. So, once we add those tables in the form as data sources. We can select the child table data source and mention the parent table name in the join source property of the child table form data source property.

## Active
Active link type update the child data sources without any delay when you select the parent table record. When you deal with more records it will affect application performance.

## Delay
Delay form data source link type is also same as active method the difference is that, delay method won't update immediately when you select the parent record. It will update the child data source when you select the parent table.
D365 F&O will use pause statement before updating the child data source. 
For example, if we are dealing with lot of records so, when the user clicks or scrolls the order, order details it will not load immediately the child data source.

So, we can use delay method because for performance improvement.

## Passive
Passive form data source link type won't update the child data source automatically. For example if we select the parent table order then order details child data source won't update. If we need to update the child data source we need to call the child data source execute query method by program (code).

## Inner join
Inner join form data source link type displays the rows that match with parent table and child table. For example if the order doesn't has any order details then the order will not be displayed.

## Outer join
Outer join form data source link type will return all parent records and matched child records. It will return all rows in the parent table.

## Exist join
Exist join form data source link type return matched rows of the parent table. It behaves like inner join but the difference is once parent row are matched with a child record the process stops and return only the parent record.
D365 F&O won't consider how many records are in the child table for the parent row.

## Not exist join
Not exist join form data source link type is totally opposite method to exist join. It will return the not matched parent records with child records.
