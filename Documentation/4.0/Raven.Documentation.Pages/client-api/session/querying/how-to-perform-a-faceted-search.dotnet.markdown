# Session : Querying : How to Perform a Faceted (Aggregated) Search

To execute facet (aggregation) query using the session `Query` method, use the `AggregateBy` or `AggregateUsing` methods. This will scope you to the aggregation query builder where you will be allowed to define single or multiple facets for the query using a straightforward and fluent API.

## Syntax

{CODE facet_1@ClientApi\Session\Querying\HowToPerformFacetedSearch.cs /}

| Parameters | | |
| ------------- | ------------- | ----- |
| **facet** | FacetBase | `FacetBase` implementation defining the scope of the facet and its options (either `Facet` or `RangeFacet`) |
| **facets** | `IEnumerable<FacetBase>` | Enumerable containing `FacetBase` implementations |
| **builder** | `Action<IFacetFactory<T>>` | Builder with a fluent API that constructs a `FacetBase` instance |
| **facetSetupDocumentId** | string | ID of a document containing `FacetSetup` | 

### Facet & RangeFacet

{INFO:Facet vs RangeFacet}
`RangeFacet` allows you to split the results of the calculations into several ranges, in contrast to `Facet` where whole spectrum of results will be used to generate a single outcome.
{INFO/}

{CODE-TABS}
{CODE-TAB:csharp:Facet facet_7_3@ClientApi\Session\Querying\HowToPerformFacetedSearch.cs /}
{CODE-TAB:csharp:RangeFacet facet_7_4@ClientApi\Session\Querying\HowToPerformFacetedSearch.cs /}
{CODE-TAB:csharp:FacetAggregation facet_7_5@ClientApi\Session\Querying\HowToPerformFacetedSearch.cs /}
{CODE-TABS/}

### Builder

{CODE facet_7_1@ClientApi\Session\Querying\HowToPerformFacetedSearch.cs /}

| Parameters | | |
| ------------- | ------------- | ----- |
| **path** | string | Points to the index field that should be used for operation (`ByRanges`, `ByField`) or to document field that should be used for aggregation (`SumOn`, `MinOn`, `MaxOn`, `AverageOn`) |
| **fieldName** | string | Points to the index field that should be used for operation (`ByRanges`, `ByField`) or to document field that should be used for aggregation (`SumOn`, `MinOn`, `MaxOn`, `AverageOn`) |
| **displayName** | string | If set, results of a facet will be returned under this name |
| **options** | `FacetOptions` | Non-default options that should be used for operation |

### Options

{CODE facet_7_2@ClientApi\Session\Querying\HowToPerformFacetedSearch.cs /}

| Options | | |
| ------------- | ------------- | ----- |
| **TermSortMode** | `FacetTermSortMode` | Indicates how terms should be sorted (`ValueAsc`, `ValueDesc`, `CountAsc`, `CountDesc`) |
| **IncludeRemainingTerms** | bool | Indicates if remaining terms should be included in results |
| **Start** | int | Used to skip given number of facet results in the outcome |
| **PageSize** | int | Used to limit facet results to the given value |

## Example I

{CODE-TABS}
{CODE-TAB:csharp:Sync facet_2_1@ClientApi\Session\Querying\HowToPerformFacetedSearch.cs /}
{CODE-TAB:csharp:Async facet_2_2@ClientApi\Session\Querying\HowToPerformFacetedSearch.cs /}
{CODE-TAB-BLOCK:sql:RQL}
from index 'Camera/Costs' 
select 
facet(Manufacturer), 
facet(Cost < 200, Cost >= 200 AND Cost < 400, Cost >= 400 AND Cost < 600, Cost >= 600 AND Cost < 800, Cost >= 800),
facet(Megapixels < 3, Megapixels >= 3 AND Megapixels < 7, Megapixels >= 7 AND Megapixels < 10, Megapixels >= 10)
{CODE-TAB-BLOCK/}
{CODE-TABS/}

## Example II

{CODE-TABS}
{CODE-TAB:csharp:Sync facet_3_1@ClientApi\Session\Querying\HowToPerformFacetedSearch.cs /}
{CODE-TAB:csharp:Async facet_3_2@ClientApi\Session\Querying\HowToPerformFacetedSearch.cs /}
{CODE-TAB-BLOCK:sql:RQL}
from index 'Camera/Costs' 
select 
facet(Manufacturer), 
facet(Cost < 200, Cost >= 200 AND Cost < 400, Cost >= 400 AND Cost < 600, Cost >= 600 AND Cost < 800, Cost >= 800),
facet(Megapixels < 3, Megapixels >= 3 AND Megapixels < 7, Megapixels >= 7 AND Megapixels < 10, Megapixels >= 10)
{CODE-TAB-BLOCK/}
{CODE-TABS/}

## Example III

{CODE-TABS}
{CODE-TAB:csharp:Sync facet_4_1@ClientApi\Session\Querying\HowToPerformFacetedSearch.cs /}
{CODE-TAB:csharp:Async facet_4_2@ClientApi\Session\Querying\HowToPerformFacetedSearch.cs /}
{CODE-TAB-BLOCK:sql:RQL}
from index 'Camera/Costs' 
select facet(id('facets/CameraFacets'))
{CODE-TAB-BLOCK/}
{CODE-TABS/}

## Remarks

{WARNING `AggregateBy` only supports aggregation by a single field. If you want to aggregate by multiple fields, you need to emit a single field that contains all values. /}

## Related Articles

### Session

- [How to Query](../../../client-api/session/querying/how-to-query)

### Indexes

- [Faceted Search](../../../indexes/querying/faceted-search) 
