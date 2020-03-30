---
title: Dizin oluşturucularda alan eşlemeleri
titleSuffix: Azure Cognitive Search
description: Alan adları ve veri gösterimleri farklılıklarını hesaba katmak için bir dizin leyicideki alan eşlemelerini yapılandırın.
manager: nitinme
author: mattmsft
ms.author: magottei
ms.devlang: rest-api
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 3e09741e841897032b8146dee67b79e0c26ea5cb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80275161"
---
# <a name="field-mappings-and-transformations-using-azure-cognitive-search-indexers"></a>Azure Bilişsel Arama dizinleyicilerini kullanarak alan eşlemeleri ve dönüşümler

Azure Bilişsel Arama dizinlerini kullanırken, bazen giriş verilerinin hedef dizinizin şemaile tam olarak eşleşmediğini fark elersiniz. Bu gibi durumlarda, dizin oluşturma işlemi sırasında verilerinizi yeniden şekillendirmek için **alan eşlemelerini** kullanabilirsiniz.

Alan eşlemelerinin yararlı olduğu bazı durumlar:

* Veri kaynağınızın adında `_id`bir alanı vardır, ancak Azure Bilişsel Arama alt puanla başlayan alan adlarını izin vermez. Alan eşleme, bir alanı etkili bir şekilde yeniden adlandırmanızı sağlar.
* Dizindeki birkaç alanı aynı veri kaynağı verilerinden doldurmak istiyorsunuz. Örneğin, bu alanlara farklı çözümleyiciler uygulamak isteyebilirsiniz.
* Bir dizin alanını birden fazla veri kaynağından gelen verilerle doldurmak istiyorsunuz ve veri kaynaklarının her biri farklı alan adları kullanır.
* Base64'ün verilerinizi kodlaması veya çözmesi gerekir. Alan eşlemeleri, Base64 kodlama ve çözme işlevleri de dahil olmak üzere çeşitli **eşleme işlevlerini**destekler.

> [!NOTE]
> Azure Bilişsel Arama dizin oluşturilarının alan eşleme özelliği, veri dönüştürme için birkaç seçenekle birlikte veri alanlarını dizin alanlarına eşleştirmenin basit bir yolunu sağlar. Daha karmaşık veriler, dizinlenmesi kolay bir forma yeniden şekillendirmek için ön işleme gerektirebilir.
>
> Microsoft Azure Veri Fabrikası, veri alma ve dönüştürme için bulut tabanlı güçlü bir çözümdür. Ayrıca dizin oluşturmadan önce kaynak verileri dönüştürmek için kod yazabilirsiniz. Kod örnekleri için [bkz.](search-example-adventureworks-modeling.md) [Model multilevel facets](search-example-adventureworks-multilevel-faceting.md)
>

## <a name="set-up-field-mappings"></a>Alan eşlemelerini ayarlama

Alan eşleme üç bölümden oluşur:

1. A `sourceFieldName`, veri kaynağınızdaki bir alanı temsil eder. Bu özellik gereklidir.
2. Arama `targetFieldName`dizininizdeki bir alanı temsil eden isteğe bağlı bir alan. Atlanırsa, veri kaynağındaki yle aynı ad kullanılır.
3. Önceden `mappingFunction`tanımlanmış birkaç işlevden birini kullanarak verilerinizi dönüştürebilen isteğe bağlı bir işlev. Fonksiyonların tam listesi [aşağıdadır.](#mappingFunctions)

Alan eşlemeleri dizinleyici tanımının `fieldMappings` dizine eklenir.

## <a name="map-fields-using-the-rest-api"></a>REST API'sini kullanarak harita alanları

[Dizin Oluştur](https://docs.microsoft.com/rest/api/searchservice/create-Indexer) API isteğini kullanarak yeni bir dizin oluşturarak alan eşlemeleri ekleyebilirsiniz. Varolan bir dizin leyicinin alan eşlemelerini, [Dizin Leyici](https://docs.microsoft.com/rest/api/searchservice/update-indexer) Yi güncelleştir isteğini kullanarak yönetebilirsiniz.

Örneğin, kaynak alanı farklı bir ada sahip bir hedef alanla nasıl eşlendirebilirsiniz:

```JSON

PUT https://[service name].search.windows.net/indexers/myindexer?api-version=[api-version]
Content-Type: application/json
api-key: [admin key]
{
    "dataSourceName" : "mydatasource",
    "targetIndexName" : "myindex",
    "fieldMappings" : [ { "sourceFieldName" : "_id", "targetFieldName" : "id" } ]
}
```

Bir kaynak alanı birden çok alan eşlemeleri başvurulabilir. Aşağıdaki örnek, aynı kaynak alanı iki farklı dizin alanına kopyalayarak bir alanı nasıl "çatalla" gösteriş yapılacağını gösterir:

```JSON

"fieldMappings" : [
    { "sourceFieldName" : "text", "targetFieldName" : "textStandardEnglishAnalyzer" },
    { "sourceFieldName" : "text", "targetFieldName" : "textSoundexAnalyzer" }
]
```

> [!NOTE]
> Azure Bilişsel Arama, alan eşlemelerinde alan ve işlev adlarını çözmek için büyük/küçük harf duyarsız karşılaştırması kullanır. Bu kullanışlıdır (tüm kasayı doğru yapmak zorunda değilsiniz), ancak veri kaynağınızın veya dizininizin yalnızca duruma göre farklılık gösteren alanlara sahip olamayacağı anlamına gelir.  
>
>

## <a name="map-fields-using-the-net-sdk"></a>.NET SDK'yı kullanarak harita alanları

.NET SDK'daki alan eşlemelerini, özellikleri `SourceFieldName` ve `TargetFieldName`isteğe bağlı `MappingFunction` bir başvuruya sahip Olan [FieldMapping](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.fieldmapping) sınıfını kullanarak tanımlarsınız.

Dizin oluşturucuyu yaparken veya daha sonra `Indexer.FieldMappings` özelliği doğrudan ayarlayarak alan eşlemelerini belirtebilirsiniz.

Aşağıdaki C# örneği, bir dizin oluşturarak alan eşlemelerini ayarlar.

```csharp
  List<FieldMapping> map = new List<FieldMapping> {
    // removes a leading underscore from a field name
    new FieldMapping("_custId", "custId"),
    // URL-encodes a field for use as the index key
    new FieldMapping("docPath", "docId", FieldMappingFunction.Base64Encode() )
  };

  Indexer sqlIndexer = new Indexer(
    name: "azure-sql-indexer",
    dataSourceName: sqlDataSource.Name,
    targetIndexName: index.Name,
    fieldMappings: map,
    schedule: new IndexingSchedule(TimeSpan.FromDays(1)));

  await searchService.Indexers.CreateOrUpdateAsync(indexer);
```

<a name="mappingFunctions"></a>

## <a name="field-mapping-functions"></a>Alan eşleme fonksiyonları

Alan eşleme işlevi, bir alanın içeriğini dizinde depolamadan önce dönüştürür. Aşağıdaki eşleme işlevleri şu anda desteklenir:

* [base64Encode](#base64EncodeFunction)
* [base64Decode](#base64DecodeFunction)
* [extractTokenAtPosition](#extractTokenAtPositionFunction)
* [jsonArrayToStringCollection](#jsonArrayToStringCollectionFunction)
* [urlEncode](#urlEncodeFunction)
* [urlDecode](#urlDecodeFunction)

<a name="base64EncodeFunction"></a>

### <a name="base64encode-function"></a>base64Encode fonksiyonu

Giriş dizesinin *URL'ye güvenli* Base64 kodlamasını gerçekleştirir. Girişin UTF-8 kodlanmış olduğunu varsayar.

#### <a name="example---document-key-lookup"></a>Örnek - belge anahtarı arama

Azure Bilişsel Arama belge anahtarında yalnızca URL'ye uygun karakterler görünebilir (çünkü müşterilerin [Arama API'sını](https://docs.microsoft.com/rest/api/searchservice/lookup-document) kullanarak belgeyi ele alabilmeleri gerekir). Anahtarınızın kaynak alanı URL'de güvenli olmayan karakterler içeriyorsa, dizine dönüştürme zamanında dönüştürmek için `base64Encode` işlevi kullanabilirsiniz. Ancak, bir belge anahtarı (dönüşümden önce ve sonra) 1.024 karakterden uzun olamaz.

Kodlanmış anahtarı arama zamanında aldığınızda, özgün anahtar `base64Decode` değerini almak için işlevi kullanabilir ve bunu kaynak belgeyi almak için kullanabilirsiniz.

```JSON

"fieldMappings" : [
  {
    "sourceFieldName" : "SourceKey",
    "targetFieldName" : "IndexKey",
    "mappingFunction" : {
      "name" : "base64Encode",
      "parameters" : { "useHttpServerUtilityUrlTokenEncode" : false }
    }
  }]
 ```

Eşleme işleviniz için bir parametre özelliği eklemezseniz, `{"useHttpServerUtilityUrlTokenEncode" : true}`bu değer varsayılandır.

Azure Bilişsel Arama iki farklı Base64 kodlamayı destekler. Aynı alanı kodlarken ve çözerken aynı parametreleri kullanmalısınız. Daha fazla bilgi için, hangi parametrelerin kullanılacağına karar vermek için [base64 kodlama seçeneklerine](#base64details) bakın.

<a name="base64DecodeFunction"></a>

### <a name="base64decode-function"></a>base64Decode fonksiyonu

Giriş dizesinin Base64 decoding gerçekleştirir. Giriş, *URL'ye güvenli* base64 kodlanmış bir dize olarak kabul edilir.

#### <a name="example---decode-blob-metadata-or-urls"></a>Örnek - blob meta verilerini veya URL'leri çözme

Kaynak verileriniz, düz metin olarak aranabilir hale getirmek istediğiniz blob meta veri dizeleri veya web URL'leri gibi Base64 kodlanmış dizeleri içerebilir. Arama dizininizi doldururken kodlanmış verileri normal dizeleri geri döndürmek için `base64Decode` işlevi kullanabilirsiniz.

```JSON

"fieldMappings" : [
  {
    "sourceFieldName" : "Base64EncodedMetadata",
    "targetFieldName" : "SearchableMetadata",
    "mappingFunction" : { 
      "name" : "base64Decode", 
      "parameters" : { "useHttpServerUtilityUrlTokenDecode" : false }
    }
  }]
```

Bir parametre özelliği eklemezseniz, bu değere `{"useHttpServerUtilityUrlTokenEncode" : true}`varsayılan olarak.

Azure Bilişsel Arama iki farklı Base64 kodlamayı destekler. Aynı alanı kodlarken ve çözerken aynı parametreleri kullanmalısınız. Daha fazla ayrıntı için, hangi parametrelerin kullanılacağına karar vermek için [base64 kodlama seçeneklerine](#base64details) bakın.

<a name="base64details"></a>

#### <a name="base64-encoding-options"></a>base64 kodlama seçenekleri

Azure Bilişsel Arama, URL için güvenli base64 kodlama ve normal base64 kodlamayı destekler. Dizin oluşturma sırasında kodlanmış base64 olan bir dize daha sonra aynı kodlama seçenekleriyle çözülmelidir, aksi takdirde sonuç orijinalle eşleşmez.

`useHttpServerUtilityUrlTokenEncode` `base64Decode` `true` `base64Encode` Kodlama ve kod çözme parametreleri sırasıyla ayarlanırsa, o zaman [HttpServerUtility.UrlTokenEncode](https://msdn.microsoft.com/library/system.web.httpserverutility.urltokenencode.aspx) gibi davranış ve [HttpServerUtility.UrlTokenDecode](https://msdn.microsoft.com/library/system.web.httpserverutility.urltokendecode.aspx)gibi davranış . `useHttpServerUtilityUrlTokenDecode`

> [!WARNING]
> Anahtar `base64Encode` değerleri üretmek için `useHttpServerUtilityUrlTokenEncode` kullanılırsa, doğru olarak ayarlanmalıdır. Anahtar değerler için yalnızca URL'ye uygun base64 kodlaması kullanılabilir. Temel değerlerdeki karakterlere ilişkin kısıtlamaların tamamını&#41;[&#40;Adlandırma kurallarına](https://docs.microsoft.com/rest/api/searchservice/naming-rules) bakın.

Azure Bilişsel Arama'daki .NET kitaplıkları, yerleşik kodlama sağlayan tam .NET Çerçevesi'ni varsayar. Ve `useHttpServerUtilityUrlTokenEncode` `useHttpServerUtilityUrlTokenDecode` seçenekler bu yerleşik işlevden yararlanır. .NET Core veya başka bir çerçeve kullanıyorsanız, `false` bu seçenekleri ayarlamanızı ve çerçevenizin kodlama ve çözme işlevlerini doğrudan aramanızı öneririz.

Aşağıdaki tablo dize `00>00?00`farklı base64 kodlamaları karşılaştırır. Base64 işlevleriniz için gerekli ek işlemeyi (varsa) belirlemek için, kitaplık kodlama işlevinizi dize `00>00?00` üzerine uygulayın ve çıktıyı beklenen çıktıyla `MDA-MDA_MDA`karşılaştırın.

| Encoding | Base64 çıkış kodlamak | Kitaplık kodlaması sonrası ek işleme | Kitaplık çözmeden önce ek işleme |
| --- | --- | --- | --- |
| Dolgu ile Base64 | `MDA+MDA/MDA=` | URL'ye güvenli karakterler kullanın ve dolguları kaldırın | Standart base64 karakter kullanın ve dolgu ekleyin |
| Dolgu olmadan Base64 | `MDA+MDA/MDA` | URL'ye güvenli karakterler kullanma | Standart base64 karakter kullanma |
| Dolgulu URL güvenli base64 | `MDA-MDA_MDA=` | Dolguları kaldırma | Dolgu ekleme |
| Dolgu olmadan URL güvenli base64 | `MDA-MDA_MDA` | None | None |

<a name="extractTokenAtPositionFunction"></a>

### <a name="extracttokenatposition-function"></a>extractTokenAtPosition fonksiyonu

Belirtilen sınırssınırını kullanarak bir dize alanını böler ve sonuçlanan bölmede belirtilen konumda belirteç alır.

Bu işlev aşağıdaki parametreleri kullanır:

* `delimiter`: giriş dizesini bölerken ayırıcı olarak kullanılacak bir dize.
* `position`: giriş dizesi bölündükten sonra seçilen belirteci bir yarım sıfır tabanlı konumu.

Örneğin, `Jane Doe`giriş , `delimiter` is `" "`(boşluk) ve 0 `position` ise, sonuç; `Jane` 1 `position` ise, sonuç `Doe`. Konum, var olmayan bir belirteç ifade ediyorsa, bir hata döndürülür.

#### <a name="example---extract-a-name"></a>Örnek - bir ad ayıklamak

Veri kaynağınız `PersonName` bir alan içerir ve bunu iki `FirstName` `LastName` ayrı ve alan olarak dizine dizine almak istersiniz. Bu işlevi, boşluk karakterini sınırlayıcı olarak kullanarak girişi bölmek için kullanabilirsiniz.

```JSON

"fieldMappings" : [
  {
    "sourceFieldName" : "PersonName",
    "targetFieldName" : "FirstName",
    "mappingFunction" : { "name" : "extractTokenAtPosition", "parameters" : { "delimiter" : " ", "position" : 0 } }
  },
  {
    "sourceFieldName" : "PersonName",
    "targetFieldName" : "LastName",
    "mappingFunction" : { "name" : "extractTokenAtPosition", "parameters" : { "delimiter" : " ", "position" : 1 } }
  }]
```

<a name="jsonArrayToStringCollectionFunction"></a>

### <a name="jsonarraytostringcollection-function"></a>jsonArrayToStringCollection fonksiyonu

JSON dizeleri dizisi olarak biçimlendirilmiş bir dizeyi, dizideki bir `Collection(Edm.String)` alanı doldurmak için kullanılabilecek bir dize dizisine dönüştürür.

Örneğin, giriş dizesi `["red", "white", "blue"]`ise, tür `Collection(Edm.String)` hedef alanı üç değer `red`, ve `white`. `blue` JSON dize dizileri olarak ayrıştırılamayan giriş değerleri için bir hata döndürülür.

#### <a name="example---populate-collection-from-relational-data"></a>Örnek - ilişkisel verilerden toplama yıdoldurma

Azure SQL Veritabanı'nda, Azure Bilişsel Arama'daki `Collection(Edm.String)` alanları doğal olarak eşleyen yerleşik bir veri türü yoktur. Dize toplama alanlarını doldurmak için, kaynak verilerinizi JSON dize dizisi `jsonArrayToStringCollection` olarak önceden işleyebilir ve ardından eşleme işlevini kullanabilirsiniz.

```JSON

"fieldMappings" : [
  {
    "sourceFieldName" : "tags", 
    "mappingFunction" : { "name" : "jsonArrayToStringCollection" }
  }]
```

İlişkili verileri dizin toplama alanlarına dönüştüren ayrıntılı bir örnek [için](search-example-adventureworks-modeling.md)bkz.

<a name="urlEncodeFunction"></a>

### <a name="urlencode-function"></a>urlEncode fonksiyonu

Bu işlev, bir dizeyi "URL güvenli" olacak şekilde kodlamak için kullanılabilir. BIR URL'de izin verilmeyen karakterler içeren bir dize ile kullanıldığında, bu işlev bu "güvensiz" karakterleri karakter-varlık eşdeğerlerine dönüştürür. Bu işlev UTF-8 kodlama biçimini kullanır.

#### <a name="example---document-key-lookup"></a>Örnek - belge anahtarı arama

`urlEncode``base64Encode` işlev, yalnızca URL güvenli olmayan karakterler dönüştürülecekse, diğer karakterleri olduğu gibi tutarken işleve alternatif olarak kullanılabilir.

Diyelim ki, giriş `<hello>` dizesi - sonra `(Edm.String)` tür hedef alan değeri ile doldurulur`%3chello%3e`

Kodlanmış anahtarı arama zamanında aldığınızda, özgün anahtar `urlDecode` değerini almak için işlevi kullanabilir ve bunu kaynak belgeyi almak için kullanabilirsiniz.

```JSON

"fieldMappings" : [
  {
    "sourceFieldName" : "SourceKey",
    "targetFieldName" : "IndexKey",
    "mappingFunction" : {
      "name" : "urlEncode"
    }
  }]
 ```

 <a name="urlDecodeFunction"></a>

 ### <a name="urldecode-function"></a>urlDecode fonksiyonu

 Bu işlev, UTF-8 kodlama biçimini kullanarak URL kodlanmış bir dizeyi decoded dizesine dönüştürür.

 ### <a name="example---decode-blob-metadata"></a>Örnek - blob meta verilerini çöz

 Bazı Azure depolama istemcileri, ASCII olmayan karakterler içeriyorsa blob meta verilerini otomatik olarak url olarak kodlar. Ancak, bu tür meta verileri aranabilir (düz metin olarak) `urlDecode` yapmak istiyorsanız, arama dizininizi doldururken kodlanmış verileri normal dizeleri geri döndürmek için işlevi kullanabilirsiniz.

 ```JSON

"fieldMappings" : [
  {
    "sourceFieldName" : "UrlEncodedMetadata",
    "targetFieldName" : "SearchableMetadata",
    "mappingFunction" : {
      "name" : "urlDecode"
    }
  }]
 ```
 
 <a name="fixedLengthEncodeFunction"></a>
 
 ### <a name="fixedlengthencode-function"></a>sabitLengthEncode fonksiyonu
 
 Bu işlev, herhangi bir uzunluktaki bir dizeyi sabit uzunluktabir dize dönüştürür.
 
 ### <a name="example---map-document-keys-that-are-too-long"></a>Örnek - çok uzun olan harita belge anahtarları
 
Belge anahtarının 1024 karakterden uzun olduğundan şikayet eden hatalarla karşılaştığında, bu işlev belge anahtarının uzunluğunu azaltmak için uygulanabilir.

 ```JSON

"fieldMappings" : [
  {
    "sourceFieldName" : "metadata_storage_path",
    "targetFieldName" : "your key field",
    "mappingFunction" : {
      "name" : "fixedLengthEncode"
    }
  }]
 ```
