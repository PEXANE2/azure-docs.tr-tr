---
title: Media Services REST ile kodlama yaparken bir videoyu alt kırpın
description: Bu konu, REST kullanarak Azure Media Services kodlama sırasında bir videonun nasıl alt kırpılacağını açıklamaktadır
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: how-to
ms.date: 06/10/2019
ms.author: inhenkel
ms.openlocfilehash: 9c568963355068b744820407e6bdd6b1b6721128
ms.sourcegitcommit: 02bc06155692213ef031f049f5dcf4c418e9f509
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2021
ms.locfileid: "106282247"
---
# <a name="subclip-a-video-when-encoding-with-media-services---rest"></a>Media Services REST ile kodlarken bir videoyu alt kırpın

Bir [işi](/rest/api/media/jobs)kullanarak kodlarken videoyu kırpabilir veya alt kırpabilirsiniz. Bu işlev, [Builtınstandardencoderönayar](/rest/api/media/transforms/createorupdate#builtinstandardencoderpreset) ön ayarları veya [Standardencoderönayar](/rest/api/media/transforms/createorupdate#standardencoderpreset) önayarları kullanılarak oluşturulan [dönüşümlerle](/rest/api/media/transforms) birlikte kullanılır. 

Bu konudaki REST örneği, bir kodlama işi gönderdiği için videoyu kırpan bir iş oluşturur. 

## <a name="prerequisites"></a>Önkoşullar

Bu konuda açıklanan adımları tamamlayabilmeniz için şunları yapmanız gerekir:

- [Azure Media Services hesabı oluşturun](./account-create-how-to.md).
- [Azure Media Services REST API'si çağrıları Için Postman 'ı yapılandırın](setup-postman-rest-how-to.md).
    
    [Azure AD belirtecini al](setup-postman-rest-how-to.md#get-azure-ad-token)konusunun son adımını izlediğinizden emin olun. 
- Bir dönüşüm ve çıkış varlıkları oluşturun. [URL 'ye bağlı olarak uzak bir dosya kodlamak ve VIDEO Rest](stream-files-tutorial-with-rest.md) öğreticisini akışa almak Için bir dönüştürme ve çıkış varlıkları oluşturmayı görebilirsiniz.
- [Kodlama kavramı](encode-concept.md) konusunu gözden geçirin.

## <a name="create-a-subclipping-job"></a>Bir alt kırpma işi oluşturun

1. İndirdiğiniz Postman koleksiyonunda **dönüşümler ve işler**  ->  **oluşturma işini alt kırpması ile** yapın.
    
    **PUT** isteği şöyle görünür:
    
    ```
    https://management.azure.com/subscriptions/:subscriptionId/resourceGroups/:resourceGroupName/providers/Microsoft.Media/mediaServices/:accountName/transforms/:transformName/jobs/:jobName?api-version={{api-version}}
    ```
1. "TransformName" ortam değişkeninin değerini, dönüşüm adınızla güncelleştirin. 
1. **Gövde** sekmesini seçin ve "mbir Putasset" değerini çıkış varlık adınızla güncelleştirin.

    ```json
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

    Oluşturulan ve gönderilen iş ve işin durumu hakkında bilgi içeren **yanıtı** görürsünüz. 

## <a name="next-steps"></a>Sonraki adımlar

[Özel dönüşümle kodlama](encode-custom-preset-rest-how-to.md) 
