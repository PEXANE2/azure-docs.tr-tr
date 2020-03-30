---
title: Video Dizinleyici API ile Dil modelini özelleştirme
titlesuffix: Azure Media Services
description: Video Dizinleyici API ile bir Dil modelini nasıl özelleştireceğimiz öğrenin.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 02/04/2020
ms.author: anzaman
ms.openlocfilehash: 19067bbbaf93c9abc9a9220b09dd482ce9115655
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80127973"
---
# <a name="customize-a-language-model-with-the-video-indexer-api"></a>Video Dizinleyici API ile Dil modelini özelleştirme

Video Indexer, uyarlama metnini, yani sözcük dağarcığının uyarlamasını istediğiniz etki alanından metin yükleyerek konuşma tanımayı özelleştirmek için özel Dil modelleri oluşturmanıza olanak tanır. Modelinizi eğittindikten sonra, uyarlama metninde görünen yeni sözcükler tanınır.

Özel Dil modelleri için ayrıntılı bir genel bakış ve en iyi uygulamalar [için, Video Dizinleyici ile Bir Dil Modelini Özelleştir'e](customize-language-model-overview.md)bakın.

Bu konuda açıklandığı gibi, hesabınızda özel Dil modelleri oluşturmak ve bunları yeniden oluşturmak için Video Dizinleyici API'lerini kullanabilirsiniz. [Ayrıca, Video Indexer web sitesini kullanarak Dili Özelleştir modelinde](customize-language-model-with-api.md)açıklandığı gibi web sitesini de kullanabilirsiniz.

## <a name="create-a-language-model"></a>Dil modeli oluşturma

[Bir dil modeli API oluşturmak](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Create-Language-Model?) belirtilen hesapta yeni bir özel Dil modeli oluşturur. Bu çağrıda Dil modeli için dosya yükleyebilirsiniz. Alternatif olarak, dil modelini buradan oluşturabilir ve daha sonra Dil modelini güncelleyerek model için dosya yükleyebilirsiniz.

> [!NOTE]
> Yine de, dosyaların içeriğini öğrenmek için modelin etkin dosyalarıyla modeli eğitmeniz gerekir. Bir dilin eğitimiile ilgili talimatlar bir sonraki bölümde yer almaktadır.

Dil modeline eklenecek dosyaları yüklemek için, yukarıda gerekli parametreler için değerler sağlamanın yanı sıra FormData'yı kullanarak gövdeye dosya yüklemeniz gerekir. Bu görevi yapmanın iki yolu vardır:

* Anahtar dosya adı olacak ve değer txt dosyası olacaktır.
* Anahtar dosya adı olacak ve değer txt dosyasına bir URL olacaktır.

### <a name="response"></a>Yanıt

Yanıt, bu örnek JSON çıktısının biçimini izleyen modelin her dosyasındaki meta verilerle birlikte yeni oluşturulan Dil modelinde meta veriler sağlar:

```json
{
    "id": "dfae5745-6f1d-4edd-b224-42e1ab57a891",
    "name": "TestModel",
    "language": "En-US",
    "state": "None",
    "languageModelId": "00000000-0000-0000-0000-000000000000",
    "files": [
    {
        "id": "25be7c0e-b6a6-4f48-b981-497e920a0bc9",
        "name": "hellofile",
        "enable": true,
        "creator": "John Doe",
        "creationTime": "2018-04-28T11:55:34.6733333"
    },
    {
        "id": "33025f5b-2354-485e-a50c-4e6b76345ca7",
        "name": "worldfile",
        "enable": true,
        "creator": "John Doe",
        "creationTime": "2018-04-28T11:55:34.86"
    }
    ]
}

```

## <a name="train-a-language-model"></a>Dil modelini eğitin

Bir [dil modeli API'yi,](https://api-portal.videoindexer.ai/docs/services/operations/operations/Train-Language-Model?&pattern=train) dil modeline yüklenen ve dil modelinde etkinleştirilen dosyalardaki içeriklerle belirtilen hesapta özel bir Dil modeli eğitir.

> [!NOTE]
> Önce Dil modelini oluşturmalı ve dosyalarını yüklemeniz gerekir. Dil modelini oluştururken veya Dil modelini güncelleyerek dosya yükleyebilirsiniz.

### <a name="response"></a>Yanıt

Yanıt, bu örnek JSON çıktısının biçimini izleyerek modelin her dosyasındaki meta verilerle birlikte yeni eğitilen Dil modelinde meta veriler sağlar:

```json
{
    "id": "41464adf-e432-42b1-8e09-f52905d7e29d",
    "name": "TestModel",
    "language": "En-US",
    "state": "Waiting",
    "languageModelId": "531e5745-681d-4e1d-b124-12e5ab57a891",
    "files": [
    {
        "id": "84fcf1ac-1952-48f3-b372-18f768eedf83",
        "name": "RenamedFile",
        "enable": false,
        "creator": "John Doe",
        "creationTime": "2018-04-27T20:10:10.5233333"
    },
    {
        "id": "9ac35b4b-1381-49c4-9fe4-8234bfdd0f50",
        "name": "hellofile",
        "enable": true,
        "creator": "John Doe",
        "creationTime": "2018-04-27T20:10:10.68"
    }
    ]
}
```

İade, `id` dil modellerini ayırt etmek için `languageModelId` kullanılan benzersiz bir kimliktir, hem [video dizini yüklemek](https://api-portal.videoindexer.ai/docs/services/operations/operations/Upload-video?) hem de video API'lerini yeniden [dizine almak](https://api-portal.videoindexer.ai/docs/services/operations/operations/Re-index-video?) için kullanılır (Video Indexer yükleme/reindex `linguisticModelId` API'lerinde de bilinir).

## <a name="delete-a-language-model"></a>Dil modelini silme

Bir [dil modeli](https://api-portal.videoindexer.ai/docs/services/operations/operations/Delete-Language-Model?&pattern=delete) API silme belirtilen hesaptan özel bir Dil modeli siler. Silinen Dil modelini kullanan herhangi bir video, siz videoyu yeniden dizine ekleyene kadar aynı dizini tutar. Videoyu yeniden dizine eklerseniz, videoya yeni bir Dil modeli atayabilirsiniz. Aksi takdirde, Video Indexer videoyu yeniden dizine almak için varsayılan modelini kullanır.

### <a name="response"></a>Yanıt

Dil modeli başarıyla silindiğinde iade edilen içerik yoktur.

## <a name="update-a-language-model"></a>Dil modelini güncelleştirme

Dil modeli API'sini [güncelleştirmek,](https://api-portal.videoindexer.ai/docs/services/operations/operations/Update-Language-Model?&pattern=update) belirtilen hesaptaki özel bir Dil kişisi modelini güncelleştirir.

> [!NOTE]
> Dil modelini zaten oluşturmuş olmalısınız. Bu aramayı, model kapsamındaki tüm dosyaları etkinleştirmek veya devre dışı kalmak, Dil modelinin adını güncelleştirmek ve dil modeline eklenecek dosyaları yüklemek için kullanabilirsiniz.

Dil modeline eklenecek dosyaları yüklemek için, yukarıda gerekli parametreler için değerler sağlamanın yanı sıra FormData'yı kullanarak gövdeye dosya yüklemeniz gerekir. Bu görevi yapmanın iki yolu vardır:

* Anahtar dosya adı olacak ve değer txt dosyası olacaktır.
* Anahtar dosya adı olacak ve değer txt dosyasına bir URL olacaktır.

### <a name="response"></a>Yanıt

Yanıt, bu örnek JSON çıktısının biçimini izleyerek modelin her dosyasındaki meta verilerle birlikte yeni eğitilen Dil modelinde meta veriler sağlar:

```json
{
    "id": "41464adf-e432-42b1-8e09-f52905d7e29d",
    "name": "TestModel",
    "language": "En-US",
    "state": "Waiting",
    "languageModelId": "531e5745-681d-4e1d-b124-12e5ab57a891",
    "files": [
    {
        "id": "84fcf1ac-1952-48f3-b372-18f768eedf83",
        "name": "RenamedFile",
        "enable": true,
        "creator": "John Doe",
        "creationTime": "2018-04-27T20:10:10.5233333"
    },
    {
        "id": "9ac35b4b-1381-49c4-9fe4-8234bfdd0f50",
        "name": "hellofile",
        "enable": true,
        "creator": "John Doe",
        "creationTime": "2018-04-27T20:10:10.68"
    }
    ]
}
```

Dosyanın `id` içeriğini indirmek için yanıtdöndürülen dosyaların kullanın.

## <a name="update-a-file-from-a-language-model"></a>Dil modelinden bir dosyayı güncelleştirme

[Dosyagüncelleştirme,](https://api-portal.videoindexer.ai/docs/services/operations/operations/Update-Language-Model-file?&pattern=update) belirtilen hesaptaki özel `enable` bir Dil modelinde bir dosyanın adını ve durumunu güncelleştirmenize olanak tanır.

### <a name="response"></a>Yanıt

Yanıt, aşağıdaki örnek JSON çıktısının biçimini izleyerek güncellediğiniz dosyaüzerinde meta veriler sağlar.

```json
{
  "id": "84fcf1ac-1952-48f3-b372-18f768eedf83",
  "name": "RenamedFile",
  "enable": false,
  "creator": "John Doe",
  "creationTime": "2018-04-27T20:10:10.5233333"
}
```

`id` Yanıtta döndürülen dosyanın içeriğini dosyayı indirmek için dosyayı kullanın.

## <a name="get-a-specific-language-model"></a>Belirli bir Dil modeli alın

[Get](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-Language-Model?&pattern=get) API, dil ve Dil modelinde bulunan dosyalar gibi belirtilen hesapta belirtilen Dil modeliyle ilgili bilgileri döndürür.

### <a name="response"></a>Yanıt

Yanıt, bu örnek JSON çıktısının biçimini izleyen modelin her dosyasındaki meta verilerle birlikte belirtilen Dil modelinde meta veriler sağlar:

```json
{
    "id": "dfae5745-6f1d-4edd-b224-42e1ab57a891",
    "name": "TestModel",
    "language": "En-US",
    "state": "None",
    "languageModelId": "00000000-0000-0000-0000-000000000000",
    "files": [
    {
        "id": "25be7c0e-b6a6-4f48-b981-497e920a0bc9",
        "name": "hellofile",
        "enable": true,
        "creator": "John Doe",
        "creationTime": "2018-04-28T11:55:34.6733333"
    },
    {
        "id": "33025f5b-2354-485e-a50c-4e6b76345ca7",
        "name": "worldfile",
        "enable": true,
        "creator": "John Doe",
        "creationTime": "2018-04-28T11:55:34.86"
    }
    ]
}
```

`id` Yanıtta döndürülen dosyanın içeriğini dosyayı indirmek için dosyayı kullanın.

## <a name="get-all-the-language-models"></a>Tüm Dil modellerini alın

[Get tüm](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-Language-Models?&pattern=get) API, bir listedeki belirtilen hesaptaki tüm özel Dil modellerini döndürür.

### <a name="response"></a>Yanıt

Yanıt, hesabınızdaki tüm Dil modellerinin ve bu örnek JSON çıktısının biçimini izleyen meta verilerinin ve dosyalarının bir listesini sağlar:

```json
[
    {
        "id": "dfae5745-6f1d-4edd-b224-42e1ab57a891",
        "name": "TestModel",
        "language": "En-US",
        "state": "None",
        "languageModelId": "00000000-0000-0000-0000-000000000000",
        "files": [
        {
            "id": "25be7c0e-b6a6-4f48-b981-497e920a0bc9",
            "name": "hellofile",
            "enable": true,
            "creator": "John Doe",
            "creationTime": "2018-04-28T11:55:34.6733333"
        },
        {
            "id": "33025f5b-2354-485e-a50c-4e6b76345ca7",
            "name": "worldfile",
            "enable": true,
            "creator": "John Doe",
            "creationTime": "2018-04-28T11:55:34.86"
        }
        ]
    },
    {
        "id": "dfae5745-6f1d-4edd-b224-42e1ab57a892",
        "name": "AnotherTestModel",
        "language": "En-US",
        "state": "None",
        "languageModelId": "00000000-0000-0000-0000-000000000001",
        "files": []
    }
]
```

## <a name="delete-a-file-from-a-language-model"></a>Dil modelinden dosya silme

[Silme](https://api-portal.videoindexer.ai/docs/services/operations/operations/Delete-Language-Model-File?&pattern=delete) API'si belirtilen hesapta belirtilen Dil modelinden belirtilen dosyayı siler.

### <a name="response"></a>Yanıt

Dosya Dil modelinden başarıyla silindiğinde iade edilen içerik yoktur.

## <a name="get-metadata-on-a-file-from-a-language-model"></a>Dil modelinden dosyadaki meta verileri alma

Bir [dosyaAPI'nin meta verilerini alma,](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-Language-Model-File-Data?&pattern=get%20language%20model) hesabınızdaki seçilen Dil modelinden belirtilen dosyanın içeriğini ve meta verilerini döndürür.

### <a name="response"></a>Yanıt

Yanıt, dosyanın içeriğini ve meta verilerini json biçiminde sağlar, bu örneğe benzer şekilde:

```json
{
    "content": "hello\r\nworld",
    "id": "84fcf1ac-1952-48f3-b372-18f768eedf83",
    "name": "Hello",
    "enable": true,
    "creator": "John Doe",
    "creationTime": "2018-04-27T20:10:10.5233333"
}
```

> [!NOTE]
> Bu örnek dosyanın içeriği iki ayrı satırda "merhaba" ve dünya sözcükleridir.

## <a name="download-a-file-from-a-language-model"></a>Dil modelinden dosya indirme

Dosyayı [karşıdan yükleme](https://api-portal.videoindexer.ai/docs/services/operations/operations/Download-Language-Model-File-Content?) API,belirtilen hesaptaki belirtilen Dil modelinden belirtilen dosyanın içeriğini içeren bir metin dosyası indirir. Bu metin dosyası, başlangıçta yüklenen metin dosyasının içeriğiyle eşleşmelidir.

### <a name="response"></a>Yanıt

Yanıt, JSON formatında dosyanın içeriğini içeren bir metin dosyasının indirilmesi olacaktır.

## <a name="next-steps"></a>Sonraki adımlar

[Web sitesini kullanarak Dil modelini özelleştir](customize-language-model-with-website.md)
