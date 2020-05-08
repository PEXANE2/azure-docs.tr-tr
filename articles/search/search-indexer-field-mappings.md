---
title: Dizin oluşturucularda alan eşlemeleri
titleSuffix: Azure Cognitive Search
description: Dizin oluşturucudaki alan eşlemelerini, alan adlarında ve veri gösterimlerinde farklılıklar için bir hesaba göre yapılandırın.
manager: nitinme
author: mattmsft
ms.author: magottei
ms.devlang: rest-api
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: a5820856f7d4c51e41162f01a9687304cb223088
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/05/2020
ms.locfileid: "82791927"
---
# <a name="field-mappings-and-transformations-using-azure-cognitive-search-indexers"></a>Azure Bilişsel Arama Dizinleyicileri kullanarak alan eşlemeleri ve dönüştürmeler

Azure Bilişsel Arama Dizinleyicileri kullanırken, bazen giriş verilerinin hedef dizininizin şemasıyla tam olarak eşleşmediğini fark edersiniz. Bu durumlarda, dizin oluşturma işlemi sırasında verilerinizi yeniden şekillendirmek için **alan eşlemelerini** kullanabilirsiniz.

Alan eşlemelerinin yararlı olduğu bazı durumlar:

* Veri kaynağınız adlı `_id`bir alana sahip, ancak Azure bilişsel arama alt çizgiyle başlayan alan adlarına izin vermiyor. Alan eşleme, bir alanı etkili bir şekilde yeniden adlandırmanızı sağlar.
* Aynı veri kaynağı verilerinden dizindeki çeşitli alanları doldurmak istiyorsunuz. Örneğin, bu alanlara farklı çözümleyiciler uygulamak isteyebilirsiniz.
* Bir dizin alanını birden fazla veri kaynağından alınan verilerle doldurmak istiyorsunuz ve veri kaynakları her biri farklı alan adları kullanır.
* Verilerinizi Base64 olarak kodlamanız veya kodu çözmelisiniz. Alan eşlemeleri, Base64 kodlaması ve kod çözme işlevleri dahil olmak üzere çeşitli **eşleme işlevlerini**destekler.

> [!NOTE]
> Dizin oluşturucularda alan eşlemeleri, veri alanlarını dizin alanlarıyla eşlemenin basit bir yoludur. bu sayede, hafif veri dönüştürme özelliği de vardır. Daha karmaşık veriler, dizini oluşturmak için kullanılan bir biçime yeniden şekillendirmek üzere ön işleme gerektirebilir. Göz önünde bulundurmanız gerekebilecek bir seçenek [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/).

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
> Azure Bilişsel Arama, alan eşlemelerinde alan ve işlev adlarını çözümlemek için büyük/küçük harfe duyarsız karşılaştırma kullanır. Bu kullanışlı bir durumdur (tüm büyük küçük harfe sahip olmanız gerekmez), ancak veri kaynağınız veya dizininizin yalnızca büyük/küçük harfe göre farklı alanlar olamayacağı anlamına gelir.  
>
>

## <a name="map-fields-using-the-net-sdk"></a>.NET SDK kullanarak alanları eşleme

.NET SDK 'da, özelliklere `SourceFieldName` ve `TargetFieldName`isteğe bağlı `MappingFunction` bir başvuruya sahip [FieldMapping](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.fieldmapping) sınıfını kullanarak alan eşlemelerini tanımlarsınız.

`Indexer.FieldMappings` Özelliği doğrudan ayarlayarak Dizin Oluşturucuyu veya daha yenisini oluştururken alan eşlemelerini belirtebilirsiniz.

Aşağıdaki C# örneği, bir Dizin Oluşturucu oluştururken alan eşlemelerini ayarlar.

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

Azure Bilişsel Arama belge anahtarında yalnızca URL güvenli karakterler görünebilir (çünkü müşterilerin [Arama API](https://docs.microsoft.com/rest/api/searchservice/lookup-document) 'sini kullanarak belgeyi ele alabilmesi gerekir). Anahtarınızın kaynak alanı URL güvenli olmayan karakterler içeriyorsa, bu `base64Encode` işlevi dizin oluşturma sırasında dönüştürmek için kullanabilirsiniz. Ancak, bir belge anahtarı (dönüştürme öncesinde ve sonrasında) 1.024 karakterden daha uzun olamaz.

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

Azure Bilişsel Arama iki farklı Base64 kodlaması destekler. Aynı alanı kodlarken ve kodunu çözerken aynı parametreleri kullanmanız gerekir. Daha fazla bilgi için bkz. hangi parametrelerin kullanılacağına karar vermek için [Base64 kodlama seçenekleri](#base64details) .

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

Azure Bilişsel Arama iki farklı Base64 kodlaması destekler. Aynı alanı kodlarken ve kodunu çözerken aynı parametreleri kullanmanız gerekir. Daha ayrıntılı bilgi için bkz. hangi parametrelerin kullanılacağına karar vermek için [Base64 kodlama seçenekleri](#base64details) .

<a name="base64details"></a>

#### <a name="base64-encoding-options"></a>Base64 kodlama seçenekleri

Azure Bilişsel Arama, URL güvenli Base64 kodlamasını ve normal Base64 kodlamasını destekler. Dizin oluşturma sırasında Base64 kodlamalı bir dize, daha sonra aynı kodlama seçenekleriyle yeniden oluşturulmalıdır, aksi takdirde sonuç orijinalle eşleşmez.

`useHttpServerUtilityUrlTokenEncode` Sırasıyla kodlama ve `useHttpServerUtilityUrlTokenDecode` kod çözme parametreleri olarak `true`ayarlanmışsa [HttpServerUtility. Urltokenencoding](https://msdn.microsoft.com/library/system.web.httpserverutility.urltokenencode.aspx) gibi `base64Decode` `base64Encode` davranır ve [HttpServerUtility. urltokenşifre çözme](https://msdn.microsoft.com/library/system.web.httpserverutility.urltokendecode.aspx)gibi davranır.

> [!WARNING]
> `base64Encode` Anahtar değerleri üretmek için kullanılırsa, `useHttpServerUtilityUrlTokenEncode` true olarak ayarlanmalıdır. Anahtar değerleri için yalnızca URL-güvenli Base64 kodlaması kullanılabilir. Anahtar değerlerindeki karakterlerle ilgili kısıtlamaların tamamına yönelik [&#40;Azure Bilişsel Arama&#41;adlandırma kuralları](https://docs.microsoft.com/rest/api/searchservice/naming-rules) ' na bakın.

Azure Bilişsel Arama .NET kitaplıkları, yerleşik kodlama sağlayan tam .NET Framework kabul eder. `useHttpServerUtilityUrlTokenEncode` Ve `useHttpServerUtilityUrlTokenDecode` seçenekleri bu yerleşik işlevlerden yararlanır. .NET Core veya başka bir çerçeve kullanıyorsanız, bu seçenekleri, çerçevesinin kodlama ve kod çözme `false` işlevlerini doğrudan olarak ayarlamayı ve çağırmayı öneririz.

Aşağıdaki tabloda, dizenin `00>00?00`farklı Base64 kodlamaları karşılaştırılmaktadır. Base64 işlevleriniz için gerekli ek işlemeyi (varsa) öğrenmek için, kitaplık kodlama işlevinizi dizeye `00>00?00` uygulayın ve çıktıyı beklenen çıktıyla `MDA-MDA_MDA`karşılaştırın.

| Encoding | Base64 kodlama çıkışı | Kitaplık kodlamasından sonra ek işleme | Kitaplık kod çözmede önce ek işleme |
| --- | --- | --- | --- |
| Doldurma ile Base64 | `MDA+MDA/MDA=` | URL-güvenli karakterler kullanın ve doldurmayı kaldırın | Standart Base64 karakterlerini kullanın ve doldurma ekleyin |
| Doldurma olmadan Base64 | `MDA+MDA/MDA` | URL kullanımı güvenli karakterler | Standart Base64 karakterlerini kullan |
| URL-doldurma ile güvenli Base64 | `MDA-MDA_MDA=` | Doldurmayı kaldır | Doldurma Ekle |
| URL-doldurma olmadan güvenli Base64 | `MDA-MDA_MDA` | Yok | Yok |

<a name="extractTokenAtPositionFunction"></a>

### <a name="extracttokenatposition-function"></a>extractTokenAtPosition işlevi

Belirtilen sınırlayıcıyı kullanarak bir dize alanını böler ve ortaya çıkan bölme içinde belirtilen konumda belirteci seçer.

Bu işlev şu parametreleri kullanır:

* `delimiter`: giriş dizesini bölerken ayırıcı olarak kullanılacak bir dize.
* `position`: giriş dizesi bölünmeden sonra seçmek üzere belirtecin tam sayı sıfır tabanlı konumu.

Örneğin, `Jane Doe`giriş `delimiter` `" "`ise (boşluk) ve 0 `position` ise sonuç, olur; `Jane` 1 `position` ise sonuç olur `Doe`. Konum mevcut olmayan bir belirtece başvuruyorsa bir hata döndürülür.

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

Örneğin, `["red", "white", "blue"]`giriş dizesi ise, türü `Collection(Edm.String)` hedef alanı, ve `red` `white` `blue`olmak üzere üç değer ile doldurulur. JSON dize dizileri olarak ayrıştırılabilecek giriş değerleri için bir hata döndürülür.

#### <a name="example---populate-collection-from-relational-data"></a>Örnek-koleksiyonu ilişkisel verilerden doldur

Azure SQL veritabanı, Azure Bilişsel Arama ile doğal olarak eşleşen `Collection(Edm.String)` yerleşik bir veri türüne sahip değil. Dize koleksiyonu alanlarını doldurmak için, kaynak verilerinizi bir JSON dize dizisi olarak önceden işleyebilir ve sonra `jsonArrayToStringCollection` Mapping işlevini kullanabilirsiniz.

```JSON

"fieldMappings" : [
  {
    "sourceFieldName" : "tags", 
    "mappingFunction" : { "name" : "jsonArrayToStringCollection" }
  }]
```

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
 
 <a name="fixedLengthEncodeFunction"></a>
 
 ### <a name="fixedlengthencode-function"></a>fixedLengthEncode işlevi
 
 Bu işlev, herhangi bir uzunluktaki dizeyi sabit uzunlukta bir dizeye dönüştürür.
 
 ### <a name="example---map-document-keys-that-are-too-long"></a>Örnek-çok uzun olan belge anahtarlarını eşleme
 
Belge anahtarı hakkında 1024 karakterden daha uzun bir süredir hata olduğunda bu işlev, belge anahtarının uzunluğunu azaltmak için uygulanabilir.

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
