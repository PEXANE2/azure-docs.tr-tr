---
title: include dosyası
description: include dosyası
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: luis
ms.topic: include
ms.custom: include file
ms.date: 11/20/2019
ms.author: diberry
ms.openlocfilehash: 0677a361e853f778894b6a62a054636e3276b364
ms.sourcegitcommit: 4c831e768bb43e232de9738b363063590faa0472
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2019
ms.locfileid: "74424415"
---
Bu kıvrımlı tabanlı hızlı başlangıç, bilgi tabanınızdan yanıt alma konusunda size yol gösterir.

## <a name="prerequisites"></a>Önkoşullar

* En son [**kıvır**](https://curl.haxx.se/).
* Bir [soru-cevap oluşturma hizmetine](../How-To/set-up-qnamaker-service-azure.md) sahip olmanız ve [sorular ve yanıtlar ile Bilgi Bankası](../Tutorials/create-publish-query-in-portal.md)etmeniz gerekir.

## <a name="publish-to-get-endpoint"></a>Uç nokta almak için Yayımla

Bilgi tabanınızdan bir soruya yanıt oluşturmaya hazırsanız, bilgi bankaınızı [yayımlayın](../Quickstarts/create-publish-knowledge-base.md#publish-the-knowledge-base) .

## <a name="use-production-endpoint-with-curl"></a>Kıvrımlı ile üretim uç noktasını kullanma

Bilgi Bankası 'niz yayımlandığında **Yayımla** sayfası, yanıt oluşturmak için http isteği ayarlarını görüntüler. **Kıvrımlı** sekmesinde, komut satırı aracından bir yanıt oluşturmak için gereken ayarlar [gösterilmektedir.](https://www.getpostman.com)

[![sonuçları Yayımla](../media/qnamaker-use-to-generate-answer/curl-command-on-publish-page.png)](../media/qnamaker-use-to-generate-answer/curl-command-on-publish-page.png#lightbox)

KıVRıMLı bir yanıt oluşturmak için aşağıdaki adımları izleyin:

1. Metni KıVRıMLı sekmesine kopyalayın. 
1. Bir komut satırı veya Terminal açın ve metni yapıştırın.
1. Bilgi Tabanınızla ilgili olacak soruyu düzenleyin. Soruyu çevreleyen içeren JSON 'ı kaldırmamaya dikkat edin.
1. Komutunu girin. 
1. Yanıt, Yanıtla ilgili bilgileri içerir. 

    ```bash
    > curl -X POST https://qnamaker-f0.azurewebsites.net/qnamaker/knowledgebases/1111f8c-d01b-4698-a2de-85b0dbf3358c/generateAnswer -H "Authorization: EndpointKey 111841fb-c208-4a72-9412-03b6f3e55ca1" -H "Content-type: application/json" -d "{'question':'How do I programmatically update my Knowledge Base?'}"
    {
      "answers": [
        {
          "questions": [
            "How do I programmatically update my Knowledge Base?"
          ],
          "answer": "You can use our REST APIs to manage your Knowledge Base. See here for details: https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/update",
          "score": 100.0,
          "id": 18,
          "source": "Custom Editorial",
          "metadata": [
            {
              "name": "category",
              "value": "api"
            }
          ]
        }
      ]
    }
    ```

## <a name="use-staging-endpoint-with-curl"></a>Kıvrımlı ile hazırlama uç noktası kullanma

Hazırlama uç noktasından bir yanıt almak istiyorsanız `isTest` Body özelliğini kullanın.

```json
isTest:true
```


