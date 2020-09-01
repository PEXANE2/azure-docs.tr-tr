---
title: REST kullanarak medya Işlemcisi örneği alma | Microsoft Docs
description: Azure Media Services için medya içeriğini kodlamak, dönüştürmek, şifrelemek veya şifresini çözmek için bir medya işlemcisi bileşeni oluşturmayı öğrenin.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: f9ff1997-0da6-4528-aaed-792837e5be41
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2019
ms.author: juliako
ms.openlocfilehash: ba625aa048360a4c201b91b4a5a4a7ca4dd6277b
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89269514"
---
# <a name="how-to-get-a-media-processor-instance"></a>Medya Işlemcisi örneği alma

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)]

> [!div class="op_single_selector"]
> * [.NET](media-services-get-media-processor.md)
> * [REST](media-services-rest-get-media-processor.md)
> 
> 

## <a name="overview"></a>Genel Bakış
Medya Işlemcileri kodlama, biçim dönüştürme, şifreleme veya medya içeriğini çözme gibi belirli bir video veya ses işleme görevini işleyen bir bileşendir. Media Services gönderilen tüm görevler, video veya ses içeriğini kodlamak, şifrelemek veya dönüştürmek için bir medya işlemcisi gerektirir. 

## <a name="azure-media-processors"></a>Azure Medya işlemcileri 

Aşağıdaki konuda medya işlemcilerin listesi verilmiştir:

* [Kodlama medya işleyicileri](scenarios-and-availability.md#encoding-media-processors)
* [Analiz medya işlemcileri](scenarios-and-availability.md#analytics-media-processors)

>[!NOTE]
>Media Services varlıklara erişirken, HTTP isteklerinizin belirli üstbilgi alanlarını ve değerlerini ayarlamanız gerekir. Daha fazla bilgi için bkz. [Media Services REST API Geliştirme Için kurulum](media-services-rest-how-to-use.md).

## <a name="connect-to-media-services"></a>Media Services’e bağlanmak

AMS API 'sine bağlanma hakkında daha fazla bilgi için bkz. [Azure AD kimlik doğrulamasıyla Azure MEDIA SERVICES API 'Sine erişme](media-services-use-aad-auth-to-access-ams-api.md). 


## <a name="get-a-media-processor"></a>Medya işlemcisi al

Aşağıdaki REST çağrısı, bir medya işlemcisi örneğinin ada göre nasıl alınacağını gösterir (Bu durumda, **Media Encoder Standard**). 

İstek:

```console
GET https://media.windows.net/api/MediaProcessors()?$filter=Name%20eq%20'Media%20Encoder%20Standard' HTTP/1.1
DataServiceVersion: 1.0;NetFx
MaxDataServiceVersion: 3.0;NetFx
Accept: application/json
Accept-Charset: UTF-8
User-Agent: Microsoft ADO.NET Data Services
Authorization: Bearer <token>
x-ms-version: 2.19
Host: media.windows.net
```

Yanıt:

```console
. . .

{  
   "odata.metadata":"https://media.windows.net/api/$metadata#MediaProcessors",
   "value":[  
      {  
         "Id":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
         "Description":"Media Encoder Standard",
         "Name":"Media Encoder Standard",
         "Sku":"",
         "Vendor":"Microsoft",
         "Version":"1.1"
      }
   ]
}
```


## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geribildirim gönderme
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Sonraki Adımlar
Bir medya işlemcisi örneğinin nasıl alınacağını öğrenmiş olduğunuza göre, bir varlığı kodlamak için Media Encoder Standard nasıl kullanacağınızı gösteren [bir varlık kodlama](media-services-rest-get-started.md) makalesine gidin.

