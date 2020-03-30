---
title: OData ifade sözdizimi başvurusu
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama sorgularında OData ifadeleri için resmi dilbilgisi ve sözdizimi belirtimi.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
translation.priority.mt:
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pt-br
- ru-ru
- zh-cn
- zh-tw
ms.openlocfilehash: f3422fd10e062ae87bc165491e0d01ac2b4943d2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72793228"
---
# <a name="odata-expression-syntax-reference-for-azure-cognitive-search"></a>Azure Bilişsel Arama için OData ifade sözdizimi başvurusu

Azure Bilişsel Arama, API boyunca parametre olarak [OData ifadelerini](https://docs.oasis-open.org/odata/odata/v4.01/odata-v4.01-part2-url-conventions.html) kullanır. En sık, OData ifadeler `$orderby` ve `$filter` parametreler için kullanılır. Bu ifadeler, birden çok yan tümce, işlev ve işleç içeren karmaşık olabilir. Ancak, Azure Bilişsel Arama REST API'sinin birçok yerinde özellik yolları gibi basit OData ifadeleri bile kullanılır. Örneğin, yol ifadeleri API'de karmaşık alanların alt alanlarına başvurmak için kullanılır ( örneğin, örneğin bir [düşündürücüdeki](index-add-suggesters.md) `$select` alt alanları listelerken, bir [puanlama işlevi,](index-add-scoring-profiles.md)parametre ve hatta [Lucene sorgularında alanlı arama.](query-lucene-syntax.md)

Bu makalede, resmi bir dilbilgisi kullanarak OData ifadelerinin tüm bu formları açıklanmaktadır. Dilbilgisini görsel olarak keşfetmeye yardımcı olacak [etkileşimli](#syntax-diagram) bir diyagram da vardır.

## <a name="formal-grammar"></a>Resmi dilbilgisi

Azure Bilişsel Arama tarafından desteklenen OData dilinin alt kümesini Bir EBNF[(Genişletilmiş Backus-Naur Formu)](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)dilbilgisi kullanarak tanımlayabiliriz. Kurallar, en karmaşık ifadelerle başlayarak ve bunları daha ilkel ifadelere ayırarak "yukarıdan aşağıya" olarak listelenir. En üstte, Azure Bilişsel Arama REST API'sinin belirli parametrelerine karşılık gelen dilbilgisi kuralları yer alıyor:

- [`$filter`](search-query-odata-filter.md), `filter_expression` kural tarafından tanımlanır.
- [`$orderby`](search-query-odata-orderby.md), `order_by_expression` kural tarafından tanımlanır.
- [`$select`](search-query-odata-select.md), `select_expression` kural tarafından tanımlanır.
- `field_path` Kural tarafından tanımlanan alan yolları. Alan yolları API boyunca kullanılır. Bir dizinin üst düzey alanlarına veya bir veya daha fazla [karmaşık alan](search-howto-complex-data-types.md) atasına sahip alt alanlara başvurabilirler.

Sonra EBNF etkileşimle dilbilgisi ve kuralları arasındaki ilişkileri keşfetmek için izin veren bir göz atılabilir [sözdizimi diyagramıdır.](https://en.wikipedia.org/wiki/Syntax_diagram)

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
/* Top-level rules */

filter_expression ::= boolean_expression

order_by_expression ::= order_by_clause(',' order_by_clause)*

select_expression ::= '*' | field_path(',' field_path)*

field_path ::= identifier('/'identifier)*


/* Shared base rules */

identifier ::= [a-zA-Z_][a-zA-Z_0-9]*


/* Rules for $orderby */

order_by_clause ::= (field_path | sortable_function) ('asc' | 'desc')?

sortable_function ::= geo_distance_call | 'search.score()'


/* Rules for $filter */

boolean_expression ::=
    collection_filter_expression
    | logical_expression
    | comparison_expression
    | boolean_literal
    | boolean_function_call
    | '(' boolean_expression ')'
    | variable

/* This can be a range variable in the case of a lambda, or a field path. */
variable ::= identifier | field_path

collection_filter_expression ::=
    field_path'/all(' lambda_expression ')'
    | field_path'/any(' lambda_expression ')'
    | field_path'/any()'

lambda_expression ::= identifier ':' boolean_expression

logical_expression ::=
    boolean_expression ('and' | 'or') boolean_expression
    | 'not' boolean_expression

comparison_expression ::= 
    variable_or_function comparison_operator constant | 
    constant comparison_operator variable_or_function

variable_or_function ::= variable | function_call

comparison_operator ::= 'gt' | 'lt' | 'ge' | 'le' | 'eq' | 'ne'


/* Rules for constants and literals */

constant ::=
    string_literal
    | date_time_offset_literal
    | integer_literal
    | float_literal
    | boolean_literal
    | 'null'

string_literal ::= "'"([^'] | "''")*"'"

date_time_offset_literal ::= date_part'T'time_part time_zone

date_part ::= year'-'month'-'day

time_part ::= hour':'minute(':'second('.'fractional_seconds)?)?

zero_to_fifty_nine ::= [0-5]digit

digit ::= [0-9]

year ::= digit digit digit digit

month ::= '0'[1-9] | '1'[0-2]

day ::= '0'[1-9] | [1-2]digit | '3'[0-1]

hour ::= [0-1]digit | '2'[0-3]

minute ::= zero_to_fifty_nine

second ::= zero_to_fifty_nine

fractional_seconds ::= integer_literal

time_zone ::= 'Z' | sign hour':'minute

sign ::= '+' | '-'

/* In practice integer literals are limited in length to the precision of
the corresponding EDM data type. */
integer_literal ::= digit+

float_literal ::=
    sign? whole_part fractional_part? exponent?
    | 'NaN'
    | '-INF'
    | 'INF'

whole_part ::= integer_literal

fractional_part ::= '.'integer_literal

exponent ::= 'e' sign? integer_literal

boolean_literal ::= 'true' | 'false'


/* Rules for functions */

function_call ::=
    geo_distance_call |
    boolean_function_call

geo_distance_call ::=
    'geo.distance(' variable ',' geo_point ')'
    | 'geo.distance(' geo_point ',' variable ')'

geo_point ::= "geography'POINT(" lon_lat ")'"

lon_lat ::= float_literal ' ' float_literal

boolean_function_call ::=
    geo_intersects_call |
    search_in_call |
    search_is_match_call

geo_intersects_call ::=
    'geo.intersects(' variable ',' geo_polygon ')'

/* You need at least four points to form a polygon, where the first and
last points are the same. */
geo_polygon ::=
    "geography'POLYGON((" lon_lat ',' lon_lat ',' lon_lat ',' lon_lat_list "))'"

lon_lat_list ::= lon_lat(',' lon_lat)*

search_in_call ::=
    'search.in(' variable ',' string_literal(',' string_literal)? ')'

/* Note that it is illegal to call search.ismatch or search.ismatchscoring
from inside a lambda expression. */
search_is_match_call ::=
    'search.ismatch'('scoring')?'(' search_is_match_parameters ')'

search_is_match_parameters ::=
    string_literal(',' string_literal(',' query_type ',' search_mode)?)?

query_type ::= "'full'" | "'simple'"

search_mode ::= "'any'" | "'all'"
```

## <a name="syntax-diagram"></a>Sözdizimi diyagramı

Azure Bilişsel Arama tarafından desteklenen OData dil dilbilgisini görsel olarak keşfetmek için etkileşimli sözdizimi diyagramını deneyin:

> [!div class="nextstepaction"]
> [Azure Bilişsel Arama için OData sözdizimi diyagramı](https://azuresearch.github.io/odata-syntax-diagram/)

## <a name="see-also"></a>Ayrıca bkz.  

- [Azure Bilişsel Arama'daki Filtreler](search-filters.md)
- [Arama Belgeleri &#40;Azure Bilişsel Arama REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
- [Lucene sorgu söz dizimi](query-lucene-syntax.md)
- [Azure Bilişsel Arama'da basit sorgu sözdizimi](query-simple-syntax.md)
