---
title: Nesnelere blob ekleme - Azure Digital Twins | Microsoft Dokümanlar
description: Azure Digital Twins'teki kullanıcılara, aygıtlara ve alanlara nasıl blob ekleyeceğinizi öğrenin.
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 01/10/2020
ms.custom: seodec18
ms.openlocfilehash: c85db05e6feeea43023c2391998f837348caed4e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75929708"
---
# <a name="add-blobs-to-objects-in-azure-digital-twins"></a>Azure Digital Twins'teki nesnelere blob ekleme

Blobs, resimler ve günlükler gibi yaygın dosya türlerinin yapılandırılmamış gösterimleridir. Blobs, MIME türü (örneğin: "image/jpeg") ve meta verileri (ad, açıklama, yazı vb.) kullanarak ne tür verileri temsil ettiklerini izler.

Azure Digital Twins, aygıtlara, alanlara ve kullanıcılara lekeler eklemeyi destekler. Blobs bir kullanıcı, bir cihaz fotoğrafı, bir video, bir harita, bir firmware zip, JSON veri, bir günlük, vb için bir profil resmi temsil edebilir

[!INCLUDE [Digital Twins Management API familiarity](../../includes/digital-twins-familiarity.md)]

## <a name="uploading-blobs-overview"></a>Blobs genel bakış yükleme

Lekeleri belirli uç noktalara ve bunların ilgili işlevlerine yüklemek için çok parçalı istekleri kullanabilirsiniz.

[!INCLUDE [Digital Twins multipart requests](../../includes/digital-twins-multipart.md)]

### <a name="blob-metadata"></a>Blob meta verileri

**İçerik Türü** ve **İçerik-Disposition**ek olarak, Azure Digital Twins blob çok parçalı istekleri doğru JSON gövdesini belirtmelidir. Hangi JSON gövdesinin gönderileceği, yapılan HTTP istek operasyonunun türüne bağlıdır.

Dört ana JSON şema şunlardır:

[![JSON şemaları](media/how-to-add-blobs/blob-models-swagger-img.png)](media/how-to-add-blobs/blob-models-swagger-img.png#lightbox)

JSON blob meta verileri aşağıdaki modele uygundur:

```JSON
{
    "parentId": "00000000-0000-0000-0000-000000000000",
    "name": "My First Blob",
    "type": "Map",
    "subtype": "GenericMap",
    "description": "A well chosen description",
    "sharing": "None"
  }
```

| Öznitelik | Tür | Açıklama |
| --- | --- | --- |
| **Parentıd** | Dize | Blob ile ilişkilendirecek ana varlık (boşluklar, aygıtlar veya kullanıcılar) |
| **Adı** |Dize | Damlaiçin insan dostu bir isim |
| **Türü** | Dize | Blob türü - *tip* ve *typeId* kullanamazsınız  |
| **Typeıd** | Tamsayı | Blob türü kimliği - *tip* ve *typeId* kullanamazsınız |
| **Alt** | Dize | Blob alt türü - *alt tip* ve *subtypeId* kullanamazsınız |
| **alt tipId** | Tamsayı | Blob için alt tip kimliği - *alt tip* ve alt *tip Id* kullanamazsınız |
| **Açıklama** | Dize | Lekenin özelleştirilmiş açıklaması |
| **Paylaşım** | Dize | Blob paylaşılıp paylaşılamayacağına`None`- `Tree` `Global`enum [ , ] |

Blob meta verileri her zaman İçerik **Türü** `application/json` ile ilk `.json` öbek olarak veya bir dosya olarak sağlanır. Dosya verileri ikinci yığında sağlanır ve desteklenen herhangi bir MIME türünden olabilir.

Swagger belgeleri bu model şemalarını tüm ayrıntılarıyla açıklar.

[!INCLUDE [Digital Twins Swagger](../../includes/digital-twins-swagger.md)]

[Swagger nasıl kullanılır](./how-to-use-swagger.md)okuyarak referans belgeleri kullanma hakkında bilgi edinin.

### <a name="blobs-response-data"></a>Blobs yanıt verileri

Tek tek döndürülen lekeler aşağıdaki JSON şemasına uygundur:

```JSON
{
  "id": "00000000-0000-0000-0000-000000000000",
  "name": "string",
  "parentId": "00000000-0000-0000-0000-000000000000",
  "type": "string",
  "subtype": "string",
  "typeId": 0,
  "subtypeId": 0,
  "sharing": "None",
  "description": "string",
  "contentInfos": [
    {
      "type": "string",
      "sizeBytes": 0,
      "mD5": "string",
      "version": "string",
      "lastModifiedUtc": "2019-01-12T00:58:08.689Z",
      "metadata": {
        "additionalProp1": "string",
        "additionalProp2": "string",
        "additionalProp3": "string"
      }
    }
  ],
  "fullName": "string",
  "spacePaths": [
    "string"
  ]
}
```

| Öznitelik | Tür | Açıklama |
| --- | --- | --- |
| **Kimliği** | Dize | Blob için benzersiz tanımlayıcı |
| **Adı** |Dize | Damlaiçin insan dostu bir isim |
| **Parentıd** | Dize | Blob ile ilişkilendirecek ana varlık (boşluklar, aygıtlar veya kullanıcılar) |
| **Türü** | Dize | Blob türü - *tip* ve *typeId* kullanamazsınız  |
| **Typeıd** | Tamsayı | Blob türü kimliği - *tip* ve *typeId* kullanamazsınız |
| **Alt** | Dize | Blob alt türü - *alt tip* ve *subtypeId* kullanamazsınız |
| **alt tipId** | Tamsayı | Blob için alt tip kimliği - *alt tip* ve alt *tip Id* kullanamazsınız |
| **Paylaşım** | Dize | Blob paylaşılıp paylaşılamayacağına`None`- `Tree` `Global`enum [ , ] |
| **Açıklama** | Dize | Lekenin özelleştirilmiş açıklaması |
| **contentInfos** | Dizi | Sürüm de dahil olmak üzere yapılandırılmamış meta veri bilgilerini belirtir |
| **fullName** | Dize | Lekenin tam adı |
| **spacePaths** | Dize | Uzay yolu |

Blob meta verileri her zaman İçerik **Türü** `application/json` ile ilk `.json` öbek olarak veya bir dosya olarak sağlanır. Dosya verileri ikinci yığında sağlanır ve desteklenen herhangi bir MIME türünden olabilir.

### <a name="blob-multipart-request-examples"></a>Blob çok parçalı istek örnekleri

[!INCLUDE [Digital Twins Management API](../../includes/digital-twins-management-api.md)]

Bir metin dosyasını blob olarak yüklemek ve bir boşlukla ilişkilendirmek için, doğrulanmış bir HTTP POST isteği nde bulunmak için:

```plaintext
YOUR_MANAGEMENT_API_URL/spaces/blobs
```

Aşağıdaki gövde ile:

```plaintext
--USER_DEFINED_BOUNDARY
Content-Type: application/json; charset=utf-8
Content-Disposition: form-data; name="metadata"

{
  "ParentId": "54213cf5-285f-e611-80c3-000d3a320e1e",
  "Name": "My First Blob",
  "Type": "Map",
  "SubType": "GenericMap",
  "Description": "A well chosen description",
  "Sharing": "None"
}
--USER_DEFINED_BOUNDARY
Content-Disposition: form-data; name="contents"; filename="myblob.txt"
Content-Type: text/plain

This is my blob content. In this case, some text, but I could also be uploading a picture, an JSON file, a firmware zip, etc.

--USER_DEFINED_BOUNDARY--
```

| Değer | Şununla değiştir |
| --- | --- |
| USER_DEFINED_BOUNDARY | Çok parçalı içerik sınır adı |

Aşağıdaki kod, [MultipartFormDataContent](https://docs.microsoft.com/dotnet/api/system.net.http.multipartformdatacontent)sınıfını kullanarak aynı blob yüklemesinin bir .NET uygulamasıdır:

```csharp
//Supply your metadata in a suitable format
var multipartContent = new MultipartFormDataContent("USER_DEFINED_BOUNDARY");

var metadataContent = new StringContent(JsonConvert.SerializeObject(metaData), Encoding.UTF8, "application/json");
metadataContent.Headers.ContentType = MediaTypeHeaderValue.Parse("application/json; charset=utf-8");
multipartContent.Add(metadataContent, "metadata");

//MY_BLOB.txt is the String representation of your text file
var fileContents = new StringContent("MY_BLOB.txt");
fileContents.Headers.ContentType = MediaTypeHeaderValue.Parse("text/plain");
multipartContent.Add(fileContents, "contents");

var response = await httpClient.PostAsync("spaces/blobs", multipartContent);
```

Son olarak, [cURL](https://curl.haxx.se/) kullanıcıları aynı şekilde çok parçalı form isteklerini yapabilir:

```bash
curl -X POST "YOUR_MANAGEMENT_API_URL/spaces/blobs" \
 -H "Authorization: Bearer YOUR_TOKEN" \
 -H "Accept: application/json" \
 -H "Content-Type: multipart/form-data" \
 -F "meta={\"ParentId\":\"YOUR_SPACE_ID\",\"Name\":\"My CURL Blob\",\"Type\":\"Map\",\"SubType\":\"GenericMap\",\"Description\":\"A well chosen description\",\"Sharing\":\"None\"};type=application/json" \
 -F "text=PATH_TO_FILE;type=text/plain"
```

| Değer | Şununla değiştir |
| --- | --- |
| YOUR_TOKEN | Geçerli OAuth 2.0 belirteciniz |
| YOUR_SPACE_ID | Blob ile ilişkilendirmek için alanın kimliği |
| PATH_TO_FILE | Metin dosyanıza giden yol |

[![cURL örneği](media/how-to-add-blobs/http-blob-post-through-curl-img.png)](media/how-to-add-blobs/http-blob-post-through-curl-img.png#lightbox)

Başarılı bir POST yeni blob kimliğini döndürür.

## <a name="api-endpoints"></a>API uç noktaları

Aşağıdaki bölümlerde çekirdek blob ile ilgili API uç noktaları ve işlevleri açıklayınız.

### <a name="devices"></a>Cihazlar

Aygıtlara lekeler ekleyebilirsiniz. Aşağıdaki resimde, Yönetim API'leriniz için Swagger başvuru belgeleri gösterilmektedir. Bu blob tüketimi ve bunları geçmek için gerekli yol parametreleri için cihaz ile ilgili API uç noktaları belirtir.

[![Cihaz lekeleri](media/how-to-add-blobs/blobs-device-api-swagger-img.png)](media/how-to-add-blobs/blobs-device-api-swagger-img.png#lightbox)

Örneğin, bir blob'u güncelleştirmek veya oluşturmak ve blob'u bir aygıta eklemek için, doğrulanmış bir HTTP PATCH isteği nde bulunmak için:

```plaintext
YOUR_MANAGEMENT_API_URL/devices/blobs/YOUR_BLOB_ID
```

| Parametre | Şununla değiştir |
| --- | --- |
| *YOUR_BLOB_ID* | İstenilen blob kimliği |

Başarılı istekler daha önce açıklandığı gibi bir JSON nesnesi [döndürün.](#blobs-response-data)

### <a name="spaces"></a>Boşluk

Boşluklara lekeler de ekleyebilirsiniz. Aşağıdaki resimde, lekeleri işlemekten sorumlu tüm uzay API uç noktaları listelenir. Ayrıca, bu uç noktalara geçmek için herhangi bir yol parametreleri listeler.

[![Uzay lekeleri](media/how-to-add-blobs/blobs-space-api-swagger-img.png)](media/how-to-add-blobs/blobs-space-api-swagger-img.png#lightbox)

Örneğin, bir boşluğa bağlı bir blob döndürmek için, doğrulanmış bir HTTP GET isteği yapmak için:

```plaintext
YOUR_MANAGEMENT_API_URL/spaces/blobs/YOUR_BLOB_ID
```

| Parametre | Şununla değiştir |
| --- | --- |
| *YOUR_BLOB_ID* | İstenilen blob kimliği |

Başarılı istekler daha önce açıklandığı gibi bir JSON nesnesi [döndürün.](#blobs-response-data)

Aynı uç noktaya bir YAMA isteği meta veri açıklamalarını güncelleştirir ve blob sürümlerini oluşturur. HTTP isteği, gerekli meta ve çok parçalı form verileriyle birlikte PATCH yöntemi yle birlikte yapılır.

### <a name="users"></a>Kullanıcılar

Kullanıcı modellerine lekeler ekleyebilirsiniz (örneğin, profil resmini ilişkilendirmek için). Aşağıdaki resimde ilgili kullanıcı API uç noktaları ve `id`gerekli yol parametreleri, aşağıdakigibi gösterilmektedir:

[![Kullanıcı lekeleri](media/how-to-add-blobs/blobs-users-api-swagger-img.png)](media/how-to-add-blobs/blobs-users-api-swagger-img.png#lightbox)

Örneğin, bir kullanıcıya iliştirilmiş bir blob almak için, gerekli form verileriyle doğrulanmış bir HTTP GET isteği yapın:

```plaintext
YOUR_MANAGEMENT_API_URL/users/blobs/YOUR_BLOB_ID
```

| Parametre | Şununla değiştir |
| --- | --- |
| *YOUR_BLOB_ID* | İstenilen blob kimliği |

Başarılı istekler daha önce açıklandığı gibi bir JSON nesnesi [döndürün.](#blobs-response-data)

## <a name="common-errors"></a>Sık karşılaşılan hatalar

* Yaygın bir hata, doğru üstbilgi bilgilerini sağlamamayı içerir:

  ```JSON
  {
      "error": {
          "code": "400.600.000.000",
          "message": "Invalid media type in first section."
      }
  }
  ```

  Bu hatayı gidermek için, genel isteğin uygun bir **İçerik Türü** üstbilgisine sahip olduğunu doğrulayın:

     * `multipart/mixed`
     * `multipart/form-data`

  Ayrıca, her *çok parçalı parçanın* uygun bir karşılık gelen **İçerik Türüne**sahip olduğunu doğrulayın.

* [Mekansal zeka grafiğinizde](concepts-objectmodel-spatialgraph.md)aynı kaynağa birden çok blob atandığında ikinci bir yaygın hata ortaya çıkar:

  ```JSON
  {
      "error": {
          "code": "400.600.000.000",
          "message": "SpaceBlobMetadata already exists."
      }
  }
  ```

  > [!NOTE]
  > **İleti** özniteliği kaynağa göre değişir. 

  Uzamsal grafiğinizdeki her kaynağa yalnızca bir blob (her türden) eklenebilir. 

  Bu hatayı gidermek için, uygun API HTTP PATCH işlemini kullanarak varolan blob'u güncelleştirin. Bunu yapmak, varolan blob verilerini istenilen verilerle değiştirir.

## <a name="next-steps"></a>Sonraki adımlar

- Azure Digital Twins için Swagger başvuru belgeleri hakkında daha fazla bilgi edinmek için [Azure Digital Twins Swagger'ı kullan'ı](how-to-use-swagger.md)okuyun.

- Postman aracılığıyla blobs yüklemek [için, Postman yapılandırmak için nasıl](./how-to-configure-postman.md)okuyun.