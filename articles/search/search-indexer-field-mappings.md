---
title: Dizin oluşturucular kullanarak otomatik dizin oluşturma için alan eşlemeleri-Azure Search
description: Alan adlarında ve veri gösterimlerinde farklılıklar için Azure Search Dizin Oluşturucu alan eşlemelerini hesaba göre yapılandırın.
ms.date: 05/02/2019
author: mgottein
manager: nitinme
ms.author: magottei
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.custom: seodec2018
ms.openlocfilehash: b64f6dcecb26e35689ad6f569ade6c7862f06f1a
ms.sourcegitcommit: bb8e9f22db4b6f848c7db0ebdfc10e547779cccc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69648132"
---
# <a name="field-mappings-and-transformations-using-azure-search-indexers"></a>Azure Search Dizinleyicileri kullanarak alan eşlemeleri ve dönüştürmeler

Azure Search Dizinleyicileri kullanırken, bazen giriş verilerinin hedef dizininizin şemasıyla çok fazla eşleşmediğini fark edersiniz. Bu durumlarda, dizin oluşturma işlemi sırasında verilerinizi yeniden şekillendirmek için **alan eşlemelerini** kullanabilirsiniz.

Alan eşlemelerinin yararlı olduğu bazı durumlar:

* Veri kaynağınız adlı `_id`bir alana sahip, ancak Azure Search alt çizgiyle başlayan alan adlarına izin vermez. Alan eşleme, bir alanı etkili bir şekilde yeniden adlandırmanızı sağlar.
* Aynı veri kaynağı verilerinden dizindeki çeşitli alanları doldurmak istiyorsunuz. Örneğin, bu alanlara farklı çözümleyiciler uygulamak isteyebilirsiniz.
* Bir dizin alanını birden fazla veri kaynağından alınan verilerle doldurmak istiyorsunuz ve veri kaynakları her biri farklı alan adları kullanır.
* Verilerinizi Base64 olarak kodlamanız veya kodu çözmelisiniz. Alan eşlemeleri, Base64 kodlaması ve kod çözme işlevleri dahil olmak üzere çeşitli **eşleme işlevlerini**destekler.

> [!NOTE]
> Azure Search dizin oluşturucularının alan eşleme özelliği, veri dönüştürme için birkaç seçenekten veri alanlarını dizin alanlarıyla eşlemek için basit bir yol sağlar. Daha karmaşık veriler, dizin oluşturmanın kolay bir biçimde yeniden şekillendirilmesi için önceden işleme gerektirebilir.
>
> Microsoft Azure Data Factory, verileri içeri ve dönüştürme için güçlü bir bulut tabanlı çözümdür. Ayrıca, dizin oluşturmadan önce kaynak verileri dönüştürmek için kod yazabilirsiniz. Kod örnekleri için bkz. [model ilişkisel verileri modelleme](search-example-adventureworks-modeling.md) ve [çok düzeyli modelleri modelleyin](search-example-adventureworks-multilevel-faceting.md).
>

## <a name="set-up-field-mappings"></a>Alan eşlemelerini ayarlama

Bir alan eşlemesi üç bölümden oluşur:

1. Veri `sourceFieldName`kaynağınızdaki bir alanı temsil eden bir. Bu özellik gereklidir.
2. Arama dizininizdeki bir alanı temsil eden isteğe bağlı `targetFieldName`. Atlanırsa, veri kaynağıyla aynı ad kullanılır.
3. Önceden tanımlanmış `mappingFunction`birkaç işlevden birini kullanarak verilerinizi dönüştürebilen isteğe bağlı bir. İşlevlerin tam listesi [aşağıda](#mappingFunctions)verilmiştir.

Alan eşlemeleri, Dizin Oluşturucu tanımının `fieldMappings` dizisine eklenir.

## <a name="map-fields-using-the-rest-api"></a>REST API kullanarak alanları eşleme

[Create Indexer](https://docs.microsoft.com/rest/api/searchservice/create-Indexer) API isteği kullanarak yeni bir Dizin Oluşturucu oluştururken alan eşlemeleri ekleyebilirsiniz. Var olan bir dizin oluşturucunun alan eşlemelerini [güncelleştirme Dizin Oluşturucu](https://docs.microsoft.com/rest/api/searchservice/update-indexer) API isteği kullanarak yönetebilirsiniz.

Örneğin, kaynak alanı farklı bir ada sahip bir hedef alanla eşleme aşağıda verilmiştir:

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

Kaynak alana, birden çok alan eşlemesinde başvurulabilir. Aşağıdaki örnek, bir alanın nasıl "çatallı" olduğunu gösterir ve aynı kaynak alanı iki farklı Dizin alanına kopyalar:

```JSON

"fieldMappings" : [
    { "sourceFieldName" : "text", "targetFieldName" : "textStandardEnglishAnalyzer" },
    { "sourceFieldName" : "text", "targetFieldName" : "textSoundexAnalyzer" }
]
```

> [!NOTE]
> Azure Search, alan eşlemelerinde alan ve işlev adlarını çözümlemek için büyük/küçük harfe duyarsız karşılaştırma kullanır. Bu kullanışlı bir durumdur (tüm büyük küçük harfe sahip olmanız gerekmez), ancak veri kaynağınız veya dizininizin yalnızca büyük/küçük harfe göre farklı alanlar olamayacağı anlamına gelir.  
>
>

## <a name="map-fields-using-the-net-sdk"></a>.NET SDK kullanarak alanları eşleme

.NET SDK 'da, özelliklere `SourceFieldName` ve `TargetFieldName`isteğe bağlı `MappingFunction` bir başvuruya sahip [FieldMapping](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.fieldmapping) sınıfını kullanarak alan eşlemelerini tanımlarsınız.

`Indexer.FieldMappings` Özelliği doğrudan ayarlayarak Dizin Oluşturucuyu veya daha yenisini oluştururken alan eşlemelerini belirtebilirsiniz.

Aşağıdaki C# örnek, bir Dizin Oluşturucu oluştururken alan eşlemelerini ayarlar.

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

## <a name="field-mapping-functions"></a>Alan eşleme işlevleri

Alan eşleme işlevi, bir alanın içeriğini dizinde depolanmadan önce dönüştürür. Şu eşleme işlevleri şu anda destekleniyor:

* [base64Encode](#base64EncodeFunction)
* [base64Decode](#base64DecodeFunction)
* [extractTokenAtPosition](#extractTokenAtPositionFunction)
* [jsonArrayToStringCollection](#jsonArrayToStringCollectionFunction)
* [urlEncode](#urlEncodeFunction)
* [URL kod çözme](#urlDecodeFunction)

<a name="base64EncodeFunction"></a>

### <a name="base64encode-function"></a>base64Encode işlevi

Giriş dizesinin *URL güvenli* Base64 kodlamasını gerçekleştirir. Girişin UTF-8 kodlamalı olduğunu varsayar.

#### <a name="example---document-key-lookup"></a>Örnek-belge anahtarı arama

Azure Search belge anahtarında yalnızca URL-güvenli karakterler görünebilir (çünkü müşterilerin [Arama API](https://docs.microsoft.com/rest/api/searchservice/lookup-document) 'sini kullanarak belgeyi ele alabilmesi gerekir). Anahtarınızın kaynak alanı URL güvenli olmayan karakterler içeriyorsa, bu `base64Encode` işlevi dizin oluşturma sırasında dönüştürmek için kullanabilirsiniz.

Arama zamanında kodlanmış anahtarı aldığınızda, bu `base64Decode` işlevi kullanarak özgün anahtar değerini alabilir ve kaynak belgeyi almak için kullanabilirsiniz.

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

Eşleme işleviniz için bir Parameters özelliği eklemezseniz, varsayılan değer `{"useHttpServerUtilityUrlTokenEncode" : true}`olarak belirlenmiştir.

Azure Search iki farklı Base64 kodlaması destekler. Aynı alanı kodlarken ve kodunu çözerken aynı parametreleri kullanmanız gerekir. Daha fazla bilgi için bkz. hangi parametrelerin kullanılacağına karar vermek için [Base64 kodlama seçenekleri](#base64details) .

<a name="base64DecodeFunction"></a>

### <a name="base64decode-function"></a>base64Decode işlevi

Giriş dizesinin Base64 kodunu çözme işlemini gerçekleştirir. Girişin, *URL-güvenli* Base64 kodlamalı bir dize olduğu varsayılır.

#### <a name="example---decode-blob-metadata-or-urls"></a>Örnek-blob meta verileri veya URL kodunu çöz

Kaynak verileriniz, düz metin olarak aranabilir olmasını istediğiniz blob meta veri dizeleri veya Web URL 'Leri gibi Base64 kodlamalı dizeler içerebilir. Arama dizininizi doldururken, `base64Decode` kodlanmış verileri normal dizelere geri döndürmek için işlevini kullanabilirsiniz.

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

Bir Parameters özelliği eklemezseniz, varsayılan değer `{"useHttpServerUtilityUrlTokenEncode" : true}`olarak belirlenmiştir.

Azure Search iki farklı Base64 kodlaması destekler. Aynı alanı kodlarken ve kodunu çözerken aynı parametreleri kullanmanız gerekir. Daha ayrıntılı bilgi için bkz. hangi parametrelerin kullanılacağına karar vermek için [Base64 kodlama seçenekleri](#base64details) .

<a name="base64details"></a>

#### <a name="base64-encoding-options"></a>Base64 kodlama seçenekleri

Azure Search iki farklı Base64 kodlaması destekler: **HttpServerUtility URL belirteci**ve **Padding olmadan URL güvenli Base64 kodlaması**. Dizin oluşturma sırasında Base64 kodlamalı bir dizenin daha sonra aynı kodlama seçenekleriyle kodu oluşturması gerekir, aksi takdirde sonuç orijinalle eşleşmez.

`true` `base64Encode` `base64Decode` Sırasıyla kodlama ve kod çözme [](https://msdn.microsoft.com/library/system.web.httpserverutility.urltokenencode.aspx) parametreleri olarak ayarlanmışsa HttpServerUtility. urltokenencoding gibi davranır ve şunun gibi `useHttpServerUtilityUrlTokenDecode` `useHttpServerUtilityUrlTokenEncode`davranır [ HttpServerUtility. Urltokenkodunu çöz](https://msdn.microsoft.com/library/system.web.httpserverutility.urltokendecode.aspx).

Tam .NET Framework kullanmıyorsanız (yani, .NET Core veya başka bir çatı kullanıyorsanız) Azure Search davranışına benzemek üzere anahtar değerleri oluşturmak için, `useHttpServerUtilityUrlTokenEncode` `false`ve `useHttpServerUtilityUrlTokenDecode` ' i ayarlamanız gerekir. Kullandığınız kitaplığa bağlı olarak, Base64 kodlaması ve kod çözme işlevleri Azure Search tarafından kullanılanlardan farklı bir şekilde farklılık gösterebilir.

Aşağıdaki tabloda, dizenin `00>00?00`farklı Base64 kodlamaları karşılaştırılmaktadır. Base64 işlevleriniz için gerekli ek işlemeyi (varsa) öğrenmek için, kitaplık kodlama işlevinizi dizeye `00>00?00` uygulayın ve çıktıyı beklenen çıktıyla `MDA-MDA_MDA`karşılaştırın.

| Encoding | Base64 kodlama çıkışı | Kitaplık kodlamasından sonra ek işleme | Kitaplık kod çözmede önce ek işleme |
| --- | --- | --- | --- |
| Doldurma ile Base64 | `MDA+MDA/MDA=` | URL-güvenli karakterler kullanın ve doldurmayı kaldırın | Standart Base64 karakterlerini kullanın ve doldurma ekleyin |
| Doldurma olmadan Base64 | `MDA+MDA/MDA` | URL kullanımı güvenli karakterler | Standart Base64 karakterlerini kullan |
| URL-doldurma ile güvenli Base64 | `MDA-MDA_MDA=` | Doldurmayı kaldır | Doldurma Ekle |
| URL-doldurma olmadan güvenli Base64 | `MDA-MDA_MDA` | Yok. | Yok. |

<a name="extractTokenAtPositionFunction"></a>

### <a name="extracttokenatposition-function"></a>extractTokenAtPosition işlevi

Belirtilen sınırlayıcıyı kullanarak bir dize alanını böler ve ortaya çıkan bölme içinde belirtilen konumda belirteci seçer.

Bu işlev şu parametreleri kullanır:

* `delimiter`: giriş dizesini bölerken ayırıcı olarak kullanılacak bir dize.
* `position`: giriş dizesi bölünmeden sonra seçmek üzere belirtecin tam sayı sıfır tabanlı konumu.

`Jane Doe`Örneğin, giriş `Jane` `Doe` `position` ise (boşluk) ve `position` 0 ise sonuç olur; Eğer 1 ise sonuç olur. `" "` `delimiter` Konum mevcut olmayan bir belirtece başvuruyorsa bir hata döndürülür.

#### <a name="example---extract-a-name"></a>Örnek-bir adı Ayıkla

Veri kaynağınız bir `PersonName` alan içerir ve bunu iki ayrı `FirstName` ve `LastName` alan olarak dizinlemek istiyorsunuz. Bu işlevi, boşluk karakteri ayırıcı olarak kullanarak girişi ayırmak için kullanabilirsiniz.

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

### <a name="jsonarraytostringcollection-function"></a>jsonArrayToStringCollection işlevi

Dize dizisi olarak biçimlendirilen bir dizeyi, dizindeki bir `Collection(Edm.String)` alanı doldurmak için kullanılabilen bir dize dizisine dönüştürür.

Örneğin `["red", "white", "blue"]`, giriş dizesi ise, türü `Collection(Edm.String)` hedef alanı, ve `white` `blue`olmak üzere üç değer `red`ile doldurulur. JSON dize dizileri olarak ayrıştırılabilecek giriş değerleri için bir hata döndürülür.

#### <a name="example---populate-collection-from-relational-data"></a>Örnek-koleksiyonu ilişkisel verilerden doldur

Azure SQL veritabanı, Azure Search ' deki `Collection(Edm.String)` alanlarla doğal olarak eşleyen yerleşik bir veri türüne sahip değildir. Dize koleksiyonu alanlarını doldurmak için, kaynak verilerinizi bir JSON dize dizisi olarak önceden işleyebilir ve sonra `jsonArrayToStringCollection` Mapping işlevini kullanabilirsiniz.

```JSON

"fieldMappings" : [
  {
    "sourceFieldName" : "tags", 
    "mappingFunction" : { "name" : "jsonArrayToStringCollection" }
  }]
```

İlişkisel verileri dizin toplama alanlarına dönüştüren ayrıntılı bir örnek için bkz. [model ilişkisel verileri](search-example-adventureworks-modeling.md).

<a name="urlEncodeFunction"></a>

### <a name="urlencode-function"></a>urlEncode işlevi

Bu işlev, "URL Safe" olması için bir dizeyi kodlamak üzere kullanılabilir. URL 'de izin verilmeyen karakterler içeren bir dizeyle birlikte kullanıldığında, bu işlev "güvenli olmayan" karakterleri karakter varlığı eşdeğerlerine dönüştürür. Bu işlev UTF-8 kodlama biçimini kullanır.

#### <a name="example---document-key-lookup"></a>Örnek-belge anahtarı arama

`urlEncode`işlev, yalnızca URL güvensiz karakter dönüştürülürse, `base64Encode` diğer karakterlerin olduğu gibi tutulması işlevine alternatif olarak kullanılabilir.

Örneğin, giriş dizesi `<hello>` -sonra tür `(Edm.String)` hedef alanı, değer ile doldurulur`%3chello%3e`

Arama zamanında kodlanmış anahtarı aldığınızda, bu `urlDecode` işlevi kullanarak özgün anahtar değerini alabilir ve kaynak belgeyi almak için kullanabilirsiniz.

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

 ### <a name="urldecode-function"></a>Urlşifre kodu işlevi

 Bu işlev, UTF-8 kodlama biçimini kullanarak bir URL kodlamalı dizeyi kodu çözülen dizeye dönüştürür.

 ### <a name="example---decode-blob-metadata"></a>Örnek-blob meta verilerinin kodunu çöz

 Bazı Azure Storage istemcileri ASCII olmayan karakterler içeriyorsa blob meta verilerini otomatik olarak URL 'yi kodlayın. Ancak, bu tür meta verileri aranabilir (düz metin olarak) yapmak istiyorsanız, arama dizininizi doldururken kodlanmış verileri `urlDecode` normal dizelere geri döndürmek için işlevini kullanabilirsiniz.

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