# Implement Pagination in .Net Core 6 LTS

## Set the Page Size in Configuration
```json
"Pagination":{
    "PageSize":5
    //"PageSize":{Number of pages you want per fetched result}
}
```

## Implement in Pagination Logic
* 'Skip' will skip the the number of Items from result set
* 'Take' will pull the remaining data from the resultset
```csharp
var pageresult=float.Parse(config["Pagination:PageSize"]);
var resultset = db.{Model};
var pagecount=(int)Math.Ceiling(resultset.Count()/pageresult);
var result = await resultset.Skip((page - 1)*(int)pageresult)
    .Take((int)pageresult)
    .ToListAsync();
```