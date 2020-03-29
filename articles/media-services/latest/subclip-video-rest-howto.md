---
title: Azure Media Services REST ile kodlama yaparken bir videonun alt klibi
description: Bu konu, REST kullanarak Azure Medya Hizmetleri ile kodlama yaparken bir videonun nasıl alt klip kesilen nasıl açıklanır
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 06/10/2019
ms.author: juliako
ms.openlocfilehash: c39aded55fe36cb130459a4f6f119f872b1adbc4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76514332"
---
# <a name="subclip-a-video-when-encoding-with-media-services---rest"></a>Medya Hizmetleri ile kodlama yaparken bir videonun alt klibi - REST

Bir [İş'i](https://docs.microsoft.com/rest/api/media/jobs)kullanarak kodlarken videoyu kırpabilir veya alt kesebilirsiniz. Bu [işlevsellik, BuiltInStandardEncoderPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#builtinstandardencoderpreset) hazır ayarları veya [StandardEncoderPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#standardencoderpreset) hazır ayarları kullanılarak oluşturulan herhangi bir [Transform](https://docs.microsoft.com/rest/api/media/transforms) ile çalışır. 

Bu konudaki REST örneği, bir kodlama işi gönderirken videoyu kırpan bir iş oluşturur. 

## <a name="prerequisites"></a>Ön koşullar

Bu konuda açıklanan adımları tamamlamak için şunları yapmak zorundasınız:

- [Bir Azure Medya Hizmetleri hesabı oluşturun.](create-account-cli-how-to.md)
- [Azure Medya Hizmetleri REST API çağrıları için Postacı yapılandırın.](media-rest-apis-with-postman.md)
    
    [Azure AD Belirteci'ni alın'](media-rest-apis-with-postman.md#get-azure-ad-token)ın son adımını takip edin. 
- Dönüşüm ve çıktı Varlıkları oluşturun. URL'ye dayalı uzak bir dosyayı encode'da Dönüşüm ve çıktı Varlıkları'nın nasıl oluşturulabileceğini ve [videoyu nasıl yayınlayabileceğinizi](stream-files-tutorial-with-rest.md) görebilirsiniz - REST öğretici.
- Kodlama kavramı konusunu gözden [geçirin.](encoding-concept.md)

## <a name="create-a-subclipping-job"></a>Alt kupür işi oluşturma

1. İndirdiğiniz Postacı koleksiyonunda, Alt Kırpma ile **Dönüşümler ve işler** -> **Create Job'u**seçin.
    
    **PUT** isteği aşağıdaki gibi görünür:
    
    ```
    https://management.azure.com/subscriptions/:subscriptionId/resourceGroups/:resourceGroupName/providers/Microsoft.Media/mediaServices/:accountName/transforms/:transformName/jobs/:jobName?api-version={{api-version}}
    ```
1. Dönüşüm adınız ile "transformName" ortam değişkeninin değerini güncelleştirin. 
1. **Gövde** sekmesini seçin ve çıktı Varlık adınız ile "myOutputAsset"i güncelleştirin.

    ```
    {
      "properties": {
        "description": "A Job with transform cb9599fb-03b3-40eb-a2ff-7ea909f53735 and single clip.",
       
        "input": {
          "@odata.type": "#Microsoft.Media.JobInputHttp",
          "baseUri": "https://nimbuscdn-nimbuspm.streaming.mediaservices.windows.net/2b533311-b215-4409-80af-529c3e853622/",
          "files": [
                "Ignite-short.mp4"
            ],
          "start": {
            "@odata.type": "#Microsoft.Media.AbsoluteClipTime",
            "time": "PT10S"
          },
          "end": {
            "@odata.type": "#Microsoft.Media.AbsoluteClipTime",
            "time": "PT40S"
          }
        },
      
        "outputs": [
          {
            "@odata.type": "#Microsoft.Media.JobOutputAsset",
            "assetName": "myOutputAsset"
          }
        ],
        "priority": "Normal"
      }
    }
    ```
1. **Gönder**’e basın.

    Oluşturulan ve gönderilen iş ve işin durumu hakkında bilgi içeren **Yanıt'ı** görürsünüz. 

## <a name="next-steps"></a>Sonraki adımlar

[Özel bir dönüştürme ile kodlama](custom-preset-rest-howto.md) 
