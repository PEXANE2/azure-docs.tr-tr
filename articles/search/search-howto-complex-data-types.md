---
title: Karmaşık veri türleri nasıl modellendirilir?
titleSuffix: Azure Cognitive Search
description: İç içe veya hiyerarşik veri yapıları, ComplexType ve Collections veri türleri kullanılarak Azure Bilişsel Arama dizininde modellenebilir.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
tags: complex data types; compound data types; aggregate data types
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 2edd62825de08becf22f2f953a63a7f89f55e0a6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79283062"
---
# <a name="how-to-model-complex-data-types-in-azure-cognitive-search"></a>Azure Bilişsel Arama'da karmaşık veri türleri nasıl modellendirilir?

Azure Bilişsel Arama dizinini doldurmak için kullanılan dış veri kümeleri birçok şekilde gelebilir. Bazen hiyerarşik veya iç içe alt yapıları içerir. Örnekler, tek bir müşteri için birden çok adres, tek bir SKU için birden çok renk ve boyut, tek bir kitabın birden çok yazarı ve benzeri olabilir. Modelleme açısından, bu yapıları *karmaşık*, *bileşik,* *bileşik*veya *toplu* veri türleri olarak anılacaktır görebilirsiniz. Azure Bilişsel Arama'nın bu kavram için kullandığı terim **karmaşık türüdür.** Azure Bilişsel Arama'da karmaşık türler **karmaşık alanlar**kullanılarak modellenir. Karmaşık alan, diğer karmaşık türler de dahil olmak üzere herhangi bir veri türünden olabilecek alt alanları (alt alanlar) içeren bir alandır. Bu, programlama dilinde yapılandırılmış veri türleri olarak benzer şekilde çalışır.

Karmaşık alanlar, veri türüne bağlı olarak belgedeki tek bir nesneyi veya bir nesne dizisini temsil eder. Tür `Edm.ComplexType` alanları tek nesneleri temsil ederken, `Collection(Edm.ComplexType)` tür alanları nesne dizilerini temsil eder.

Azure Bilişsel Arama, karmaşık türleri ve koleksiyonları doğal olarak destekler. Bu türler, Azure Bilişsel Arama dizinindeki hemen hemen tüm JSON yapısını modellemenize olanak tanır. Azure Bilişsel Arama API'lerinin önceki sürümlerinde yalnızca düzleştirilmiş satır kümeleri içe aktarılabilir. En yeni sürümde, dizininiz artık kaynak verilere daha yakından karşılık gelebilir. Başka bir deyişle, kaynak verilerinizin karmaşık türleri varsa, dizininizin karmaşık türleri de olabilir.

Başlamak için, Azure portalındaki **İçe Aktarma veri** sihirbazına yükleyebileceğiniz [Oteller veri kümesini](https://github.com/Azure-Samples/azure-search-sample-data/blob/master/README.md)öneririz. Sihirbaz kaynaktaki karmaşık türleri algılar ve algılanan yapılara dayalı bir dizin şeması önerir.

> [!Note]
> Karmaşık türleri için destek genellikle `api-version=2019-05-06`. 
>
> Arama çözümünüz bir koleksiyondaki düzleştirilmiş veri kümelerinin önceki geçici çözümleri ne olursa kuruluysa, dizininizi en yeni API sürümünde desteklenen karmaşık türleri içerecek şekilde değiştirmeniz gerekir. API sürümlerini yükseltme hakkında daha fazla bilgi için bkz: [En yeni REST API sürümüne yükseltin](search-api-migration.md) veya [en yeni .NET SDK sürümüne yükseltin.](search-dotnet-sdk-migration-version-9.md)

## <a name="example-of-a-complex-structure"></a>Karmaşık bir yapı örneği

Aşağıdaki JSON belgesi basit alanlar ve karmaşık alanlardan oluşur. Karmaşık alanlar, `Address` gibi `Rooms`ve, alt alanlar var. `Address`belgede tek bir nesne olduğundan, bu alt alanlar için tek bir değer kümesi vardır. Buna karşılık, `Rooms` alt alanları için, koleksiyondaki her nesne için bir tane olmak üzere birden çok değer kümesi vardır.

```json
{
  "HotelId": "1",
  "HotelName": "Secret Point Motel",
  "Description": "Ideally located on the main commercial artery of the city in the heart of New York.",
  "Address": {
    "StreetAddress": "677 5th Ave",
    "City": "New York",
    "StateProvince": "NY"
  },
  "Rooms": [
    {
      "Description": "Budget Room, 1 Queen Bed (Cityside)",
      "Type": "Budget Room",
      "BaseRate": 96.99
    },
    {
      "Description": "Deluxe Room, 2 Double Beds (City View)",
      "Type": "Deluxe Room",
      "BaseRate": 150.99
    },
  ]
}
```

## <a name="creating-complex-fields"></a>Karmaşık alanlar oluşturma

Herhangi bir dizin tanımında olduğu gibi, karmaşık türleri içeren bir şema oluşturmak için portalı, [REST API'yi](https://docs.microsoft.com/rest/api/searchservice/create-index)veya [.NET SDK'yı](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index?view=azure-dotnet) kullanabilirsiniz. 

Aşağıdaki örnekte, basit alanlar, koleksiyonlar ve karmaşık türleri olan bir JSON dizini şeması gösterilmektedir. Karmaşık bir tür içinde, her alt alanın bir türü olduğuna ve üst düzey alanların yaptığı gibi özniteliklere sahip olabileceğine dikkat edin. Şema yukarıdaki örnek verilere karşılık gelir. `Address`koleksiyon olmayan karmaşık bir alandır (bir otelin tek bir adresi vardır). `Rooms`karmaşık bir toplama alanıdır (bir otelde birçok oda vardır).

```json
{
  "name": "hotels",
  "fields": [
    { "name": "HotelId", "type": "Edm.String", "key": true, "filterable": true },
    { "name": "HotelName", "type": "Edm.String", "searchable": true, "filterable": false },
    { "name": "Description", "type": "Edm.String", "searchable": true, "analyzer": "en.lucene" },
    { "name": "Address", "type": "Edm.ComplexType",
      "fields": [
        { "name": "StreetAddress", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "searchable": true },
        { "name": "City", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true },
        { "name": "StateProvince", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true }
      ]
    },
    { "name": "Rooms", "type": "Collection(Edm.ComplexType)",
      "fields": [
        { "name": "Description", "type": "Edm.String", "searchable": true, "analyzer": "en.lucene" },
        { "name": "Type", "type": "Edm.String", "searchable": true },
        { "name": "BaseRate", "type": "Edm.Double", "filterable": true, "facetable": true }
      ]
    }
  ]
}
```

## <a name="updating-complex-fields"></a>Karmaşık alanları güncelleştirme

Genel olarak alanlariçin geçerli olan [yeniden dizine alma kurallarının](search-howto-reindex.md) tümü yine karmaşık alanlar için geçerlidir. Burada birkaç ana kural restating, bir alan ekleme bir dizin yeniden gerektirmez, ancak çoğu değişiklik yok.

### <a name="structural-updates-to-the-definition"></a>Tanımda yapısal güncellemeler

Dizin yeniden oluşturma sına gerek kalmadan istediğiniz zaman karmaşık bir alana yeni alt alanlar ekleyebilirsiniz. Örneğin, bir dizine üst `Address` düzey bir alan `Rooms` eklemek gibi,'e "Posta Kodu" veya "Olanaklar" eklemek için izin verilir. Verilerinizi güncelleştirerek bu alanları açıkça doldurana kadar varolan belgelerin yeni alanlar için geçersiz bir değeri vardır.

Karmaşık bir tür içinde, her alt alanın bir türü olduğuna ve üst düzey alanların yaptığı gibi özniteliklere sahip olabileceğine dikkat edin

### <a name="data-updates"></a>Veri güncellemeleri

`upload` Eylem ile bir dizindeki varolan belgeleri güncelleştirmekarmaşık ve basit alanlar için aynı şekilde çalışır - tüm alanlar değiştirilir. Ancak, `merge` (veya `mergeOrUpload` varolan bir belgeye uygulandığında) tüm alanlarda aynı çalışmaz. Özellikle, `merge` bir koleksiyon içinde öğeleri birleştirme desteklemez. Bu sınırlama, ilkel türler ve karmaşık koleksiyonların koleksiyonları için vardır. Koleksiyonu güncelleştirmek için, tam koleksiyon değerini almanız, değişiklik yapmanız ve ardından yeni koleksiyonu Dizin API isteğine eklemeniz gerekir.

## <a name="searching-complex-fields"></a>Karmaşık alanları arama

Serbest biçimli arama ifadeleri karmaşık türlerle beklendiği gibi çalışır. Belgenin herhangi bir yerinde aranabilir alan veya alt alan eşleşirse, belgenin kendisi eşleşir.

Birden çok terim ve işleç olduğunda sorgular daha nüanslı olsun ve bazı terimler [lucene sözdizimi](query-lucene-syntax.md)ile mümkün olduğunca, alan adları belirtilir. Örneğin, bu sorgu, Adres alanının iki alt alanıyla "Portland" ve "OR" olmak üzere iki terimle eşleşmeyi dener:

    search=Address/City:Portland AND Address/State:OR

Bunun gibi sorgular, filtrelerden farklı olarak tam metin arama için *ilişkilendirilmemiştir.* Filtrelerde, karmaşık bir koleksiyonun alt alanları üzerindeki sorgular, aralık değişkenleri veya [ `any` 'deki `all` ](search-query-odata-collection-operators.md)değişkenler kullanılarak ilişkilendirilir. Yukarıdaki Lucene sorgusu, Oregon'daki diğer şehirlerle birlikte "Portland, Maine" ve "Portland, Oregon" belgelerini döndürür. Bu, her yan tümce belgenin tamamında kendi alanının tüm değerleri için geçerli olduğundan, "geçerli alt belge" kavramı olmadığından olur. Bu konuda daha fazla bilgi için Azure [Bilişsel Arama'daki OData toplama filtrelerini anlama](search-query-understand-collection-filters.md)bilginine bakın.

## <a name="selecting-complex-fields"></a>Karmaşık alanları seçme

Parametre, `$select` arama sonuçlarında döndürülen alanların seçiminde kullanılır. Karmaşık bir alanın belirli alt alanlarını seçmek için bu parametreyi kullanmak için, bir`/`eğik çizgi ile ayrılmış üst alan ve alt alanı içerir ( ).

    $select=HotelName, Address/City, Rooms/BaseRate

Arama sonuçlarında olmasını istiyorsanız, alanlar diziniçinde Alınabilir olarak işaretlenmelidir. `$select` Bir deyimde yalnızca Alınabilen olarak işaretlenmiş alanlar kullanılabilir.

## <a name="filter-facet-and-sort-complex-fields"></a>Karmaşık alanları filtreleme, fason ve sıralama

Filtreleme ve alan aramaları için kullanılan aynı [OData yolu sözdizimi,](query-odata-filter-orderby-syntax.md) arama isteğindeki alanları sıralamak, sıralamak ve seçmek için de kullanılabilir. Karmaşık türler için, hangi alt alanların sıralanabilir veya yüz yüze olarak işaretlenebileceğini yöneten kurallar uygulanır. Bu kurallar hakkında daha fazla bilgi için, [Dizin API oluştur başvurusuna](/rest/api/searchservice/create-index)bakın.

### <a name="faceting-sub-fields"></a>Alt alanların yüzyüze

Herhangi bir alt alan, tür `Edm.GeographyPoint` veya `Collection(Edm.GeographyPoint)`.

Faset sonuçlarında döndürülen belge sayımları, karmaşık bir koleksiyondaki (odalar) alt belgeler için değil, ana belge (otel) için hesaplanır. Örneğin, bir otelde "süit" tipi 20 oda olduğunu varsayalım. Bu faset parametre `facet=Rooms/Type`göz önüne alındığında, faset sayısı otel için değil, 20 oda için bir olacaktır.

### <a name="sorting-complex-fields"></a>Karmaşık alanları sıralama

Sıralama işlemleri alt belgelere (Odalar) değil, belgeler (Oteller) için geçerlidir. Odalar gibi karmaşık bir koleksiyona sahipseniz, Odalar'ı hiç sıralayamadığınızı fark etmek önemlidir. Aslında, herhangi bir koleksiyon üzerinde sıralamak olamaz.

Alanların belge başına tek bir değeri olduğunda, alan basit bir alan mı yoksa karmaşık bir türde bir alt alan mı olduğu işlemleri sıralayın. Örneğin, `Address/City` otel başına yalnızca bir adres olduğundan, otellerini şehre `$orderby=Address/City` göre sıralayacaktır.

### <a name="filtering-on-complex-fields"></a>Karmaşık alanlarda filtreleme

Filtre ifadesinde karmaşık bir alanın alt alanlarına başvurabilirsiniz. Alanları sıralamak, sıralamak ve seçmek için kullanılan aynı OData yolu sözdizimini kullanmanız gereken aynı [odata yolu sözdizimini](query-odata-filter-orderby-syntax.md) kullanmanız gereken. Örneğin, aşağıdaki filtre Kanada'daki tüm otelleri döndürür:

    $filter=Address/Country eq 'Canada'

Karmaşık bir toplama alanında filtre lemek için, [ `any` ve `all` işleçleri](search-query-odata-collection-operators.md)ile bir **lambda ifadesi** kullanabilirsiniz. Bu durumda, lambda ifadesinin **aralık değişkeni** alt alanları olan bir nesnedir. Standart OData yolu sözdizimi ile bu alt alanlara başvurabilirsiniz. Örneğin, aşağıdaki filtre, en az bir deluxe odası ve sigara içilmeyen tüm odaları olan tüm otelleri iade edecektir:

    $filter=Rooms/any(room: room/Type eq 'Deluxe Room') and Rooms/all(room: not room/SmokingAllowed)

Üst düzey basit alanlarda olduğu gibi, karmaşık alanların basit alt alanları yalnızca dizin `true` tanımında ayarlanan **filtrelenebilir** özniteliği varsa filtrelere dahil edilebilir. Daha fazla bilgi için, [Dizin API başvurusu oluştur'a](/rest/api/searchservice/create-index)bakın.

## <a name="next-steps"></a>Sonraki adımlar

**İçe Aktarma** [verisi](https://github.com/Azure-Samples/azure-search-sample-data/blob/master/README.md) sihirbazındaki Oteller veri kümesini deneyin. Verilere erişmek için okuma me'sinde sağlanan Cosmos DB bağlantı bilgilerine ihtiyacınız vardır.

Bu bilgiler elinizin altında, sihirbazdaki ilk adımınız yeni bir Azure Cosmos DB veri kaynağı oluşturmaktır. Sihirbazda, hedef dizin sayfasına girdiğinizde, karmaşık türleri olan bir dizin görürsünüz. Bu dizini oluşturun ve yükleyin ve sonra yeni yapıyı anlamak için sorguları çalıştırın.

> [!div class="nextstepaction"]
> [Quickstart: alma, dizin oluşturma ve sorgular için portal sihirbazı](search-get-started-portal.md)
