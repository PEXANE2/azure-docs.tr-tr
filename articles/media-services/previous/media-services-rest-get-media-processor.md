---
title: REST kullanarak Medya İşlemci örneği nasıl alınır | Microsoft Dokümanlar
description: Azure Medya Hizmetleri için medya içeriğini kodlamak, biçimlendirmek, şifrelemek veya şifresini çözmek için medya işlemcisi bileşenini nasıl oluşturabileceğinizi öğrenin.
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
ms.openlocfilehash: 3d7b3922c9bb7eb79cd6436ba1b265714678fcc8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76774898"
---
# <a name="how-to-get-a-media-processor-instance"></a>Medya İşlemcisi örneği nasıl alınır?
> [!div class="op_single_selector"]
> * [.NET](media-services-get-media-processor.md)
> * [Geri kalanı](media-services-rest-get-media-processor.md)
> 
> 

## <a name="overview"></a>Genel Bakış
Medya İşlemcileri, medya içeriğini kodlama, biçim dönüştürme, şifreleme veya şifre çözme gibi belirli bir video veya ses işleme görevini işleyen bir bileşendir. Medya Hizmetleri'ne gönderilen tüm görevler, video veya ses içeriğini kodlamak, şifrelemek veya dönüştürmek için bir ortam işlemcisi gerektirir. 

## <a name="azure-media-processors"></a>Azure ortam işlemcileri 

Aşağıdaki konu, ortam işlemcilerinin listesini sağlar:

* [Kodlama medya işleyicileri](scenarios-and-availability.md#encoding-media-processors)
* [Analiz medya işlemcileri](scenarios-and-availability.md#analytics-media-processors)

>[!NOTE]
>Medya Hizmetleri'ndeki varlıklara erişirken, HTTP isteklerinizde belirli üstbilgi alanları ve değerleri belirlemeniz gerekir. Daha fazla bilgi için Medya [Hizmetleri REST API Geliştirme kurulumu'na](media-services-rest-how-to-use.md)bakın.

## <a name="connect-to-media-services"></a>Media Services’e bağlanmak

AMS API'sine nasıl bağlanabileceğiniz hakkında bilgi için Azure [AD kimlik doğrulaması yla Azure Medya Hizmetleri API'sine eriş'e](media-services-use-aad-auth-to-access-ams-api.md)bakın. 


## <a name="get-a-media-processor"></a>Ortam işlemcisi alın

Aşağıdaki REST çağrısı, bir medya işlemciörneğinin ada göre nasıl alındığını gösterir (bu durumda, **Media Encoder Standard).** 

İstek:

    GET https://media.windows.net/api/MediaProcessors()?$filter=Name%20eq%20'Media%20Encoder%20Standard' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <token>
    x-ms-version: 2.19
    Host: media.windows.net

Yanıt:

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


## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Sonraki Adımlar
Artık bir ortam işlemci örneğini nasıl elde edebilirsiniz bildiğinize göre, bir varlığı kodlamak için Media Encoder Standardını nasıl kullanacağınızı gösteren [Bir Varlık](media-services-rest-get-started.md) makalesini Nasıl Kodlarsınız'a gidin.

