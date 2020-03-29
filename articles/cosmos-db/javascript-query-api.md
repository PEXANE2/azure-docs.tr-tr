---
title: Azure Cosmos DB Depolanan Yordamlar ve Tetikleyiciler'de JavaScript tümleşik sorgu API'si ile çalışın
description: Bu makalede, Azure Cosmos DB'de depolanmış yordamlar ve tetikleyiciler oluşturmak için JavaScript dili tümleşik sorgu API'si kavramları tanıtıştır.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/01/2019
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: 7b7ad470b3330224e80a7160fc1a37bb5ee1cde8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76901827"
---
# <a name="javascript-query-api-in-azure-cosmos-db"></a>Azure Cosmos DB'de JavaScript sorgu API'si

Cosmos DB'de SQL API'yi kullanarak sorgu lar vermenin yanı sıra, [Cosmos DB sunucu tarafı SDK, Cosmos DB](https://azure.github.io/azure-cosmosdb-js-server/) Stored Yordamları ve Tetikleyicileri'nde optimize edilmiş sorguları gerçekleştirmek için bir JavaScript arabirimi sağlar. Bu JavaScript arabirimini kullanmak için SQL dilinden haberdar olmak zorunda değilsiniz. JavaScript sorgu API'si, ECMAScript5 dizi yerleşiklerine ve Lodash gibi popüler JavaScript kitaplıklarına aşina bir sözdizimi yle, yüklem işlevlerini işlev çağrıları dizisine geçirerek sorguları programlı bir şekilde oluşturmanıza olanak tanır. Sorgular JavaScript çalışma zamanı tarafından ayrıştırılır ve Azure Cosmos DB endeksleri kullanılarak verimli bir şekilde yürütülür.

## <a name="supported-javascript-functions"></a>Desteklenen JavaScript işlevleri

| **İşlev** | **Açıklama** |
|---------|---------|
|`chain() ... .value([callback] [, options])`|Değerle sonlandırılması gereken zincirli bir çağrı başlatır().|
|`filter(predicateFunction [, options] [, callback])`|Giriş belgelerini çıkan kümeye filtrelemek/çıkarmak için girişi doğru/yanlış döndüren bir yüklem işlevi kullanarak girişi filtreler. Bu işlev, SQL'deki WHERE yan tümcesine benzer şekilde çalışır.|
|`flatten([isShallow] [, options] [, callback])`|Her giriş öğesindeki dizileri tek bir dizide birleştirir ve düzleştirir. Bu işlev LINQ'daki SelectMany'a benzer şekilde çalışır.|
|`map(transformationFunction [, options] [, callback])`|Her giriş öğesini bir JavaScript nesnesi veya değeriyle eşleyen bir dönüştürme işlevi verilen bir projeksiyon uygular. Bu işlev, SQL'deki SELECT yan tümcesine benzer şekilde çalışır.|
|`pluck([propertyName] [, options] [, callback])`|Bu işlev, her giriş öğesinden tek bir özelliğin değerini ayıklayan bir harita için bir kısayoldur.|
|`sortBy([predicate] [, options] [, callback])`|Giriş belgesi akışındaki belgeleri, verilen yüklemi kullanarak artan sırada sıralayarak sıralayarak yeni bir belge kümesi üretir. Bu işlev, SQL'deki ORDER BY yan tümcesine benzer şekilde çalışır.|
|`sortByDescending([predicate] [, options] [, callback])`|Giriş belge akışındaki belgeleri verilen yüklemi kullanarak azalan sırada sıralayarak yeni bir belge kümesi üretir. Bu işlev, SQL'deki ORDER BY x DESC yan tümcesine benzer.|
|`unwind(collectionSelector, [resultSelector], [options], [callback])`|İç diziile kendi kendine birleştirme gerçekleştirir ve sonuç projeksiyonuna tuples olarak her iki taraftan da sonuç ekler. Örneğin, bir kişi belgeye kişi ile katılmak.evcil hayvanlar [kişi, evcil hayvan] tuples üretecek. Bu, .NET LINK'teki SelectMany'a benzer.|

Yüklem ve/veya seçici işlevlerine dahil edildiğinde, aşağıdaki JavaScript yapıları doğrudan Azure Cosmos DB endekslerinde çalışacak şekilde otomatik olarak optimize edilir:

- Basit işleçler: `=` `+` `-` `*` `/` `%` `|` `^` `&` `==` `!=` `===` `!===` `<` `>` `<=` `>=` `||` `&&` `<<` `>>` `>>>!``~`
- Gerçek edebi, nesne edebi dahil:{}
- var, dönüş

Aşağıdaki JavaScript yapıları Azure Cosmos DB endeksleri için optimize edilmez:

- Kontrol akışı (örneğin, eğer, for, while)
- İşlev çağrıları

Daha fazla bilgi için [Cosmos DB Server Side JavaScript Belgeleri'ne](https://azure.github.io/azure-cosmosdb-js-server/)bakın.

## <a name="sql-to-javascript-cheat-sheet"></a>SQL JavaScript hile sayfası

Aşağıdaki tabloda çeşitli SQL sorguları ve ilgili JavaScript sorguları sunar. SQL sorgularında olduğu gibi, özellikler (örneğin, item.id) büyük/küçük harf duyarlıdır.

> [!NOTE]
> `__`(çift alt çizgi) JavaScript `getContext().getCollection()` sorgu API'sını kullanırken bir diğer addır.

|**SQL**|**JavaScript Sorgu API**|**Açıklama**|
|---|---|---|
|SEÇ *<br>FROM dokümanlar| __.map(fonksiyon(doc) { <br>&nbsp;&nbsp;&nbsp;&nbsp;dönüş doc;<br>});|Tüm belgelerde (devamı belirteci ile paginated) sonuçları olduğu gibi.|
|SELECT <br>&nbsp;&nbsp;&nbsp;docs.id,<br>&nbsp;&nbsp;&nbsp;docs.message AS msg,<br>&nbsp;&nbsp;&nbsp;docs.actions <br>FROM dokümanlar|__.map(fonksiyon(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;dönüş {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;id: doc.id,<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;msg: doc.message,<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;eylemler:doc.actions<br>&nbsp;&nbsp;&nbsp;&nbsp;};<br>});|Kimlik, ileti (msg takma adı) ve tüm belgelerden eylem projeleri.|
|SEÇ *<br>FROM dokümanlar<br>WHERE<br>&nbsp;&nbsp;&nbsp;docs.id="X998_Y998"|__.filter(function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;return doc.id ==="X998_Y998";<br>});|Yüklemi olan belgeler için sorgular: id = "X998_Y998".|
|SEÇ *<br>FROM dokümanlar<br>WHERE<br>&nbsp;&nbsp;&nbsp;ARRAY_CONTAINS(docs. Etiketler, 123)|__.filter(fonksiyon(x) {<br>&nbsp;&nbsp;&nbsp;&nbsp;return x.Etiketler && x.Tags.indexOf(123) > -1;<br>});|Etiketler özelliği olan belgeler için sorgular ve Etiketler değeri 123 içeren bir dizidir.|
|SELECT<br>&nbsp;&nbsp;&nbsp;docs.id,<br>&nbsp;&nbsp;&nbsp;docs.message AS msg<br>FROM dokümanlar<br>WHERE<br>&nbsp;&nbsp;&nbsp;docs.id="X998_Y998"|__.zincir()<br>&nbsp;&nbsp;&nbsp;&nbsp;.filter(fonksiyon(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return doc.id ==="X998_Y998";<br>&nbsp;&nbsp;&nbsp;&nbsp;})<br>&nbsp;&nbsp;&nbsp;&nbsp;.map(fonksiyon(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;dönüş {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;id: doc.id,<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;msg: doc.message<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;};<br>&nbsp;&nbsp;&nbsp;&nbsp;})<br>.value();|Yüklemi, id = "X998_Y998" olan belgeler için sorgular ve ardından kimlik ve iletiyi (msg'ye takma ad) projeleri.|
|DEĞER SEÇ etiketi<br>FROM dokümanlar<br>DOC'larda JOIN etiketi. Etiketler<br>docs._ts GÖRE SİPARİş|__.zincir()<br>&nbsp;&nbsp;&nbsp;&nbsp;.filter(fonksiyon(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;doc'u iade edin. Etiketler && Array.isArray(doc. Etiketler);<br>&nbsp;&nbsp;&nbsp;&nbsp;})<br>&nbsp;&nbsp;&nbsp;&nbsp;.sortBy(fonksiyon(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;doc._ts;<br>&nbsp;&nbsp;&nbsp;&nbsp;})<br>&nbsp;&nbsp;&nbsp;&nbsp;.pluck("Etiketler")<br>&nbsp;&nbsp;&nbsp;&nbsp;.flatten()<br>&nbsp;&nbsp;&nbsp;&nbsp;.value()|Dizi özelliği olan belgeler için filtreler, Etiketler ve ortaya çıkan belgeleri _ts zaman damgası sistem özelliğine göre sıralar ve ardından projeler + Etiketler dizisini düzleştirir.|

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB'de daha fazla kavram ve depolanmış yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevler yazma ve kullanma hakkında daha fazla bilgi edinin:

- [Javascript Query API kullanarak depolanan yordamlar ve tetikleyiciler nasıl yazılır?](how-to-write-javascript-query-api.md)
- [Azure Cosmos DB depolanan yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevlerle çalışma](stored-procedures-triggers-udfs.md)
- [Azure Cosmos DB'de depolanan yordamlar, tetikleyiciler, kullanıcı tanımlı işlevler nasıl kullanılır?](how-to-use-stored-procedures-triggers-udfs.md)
- [Azure Cosmos DB JavaScript sunucu tarafı API başvurusu](https://azure.github.io/azure-cosmosdb-js-server)
- [JavaScript ES6 (ECMA 2015)](https://www.ecma-international.org/ecma-262/6.0/)
