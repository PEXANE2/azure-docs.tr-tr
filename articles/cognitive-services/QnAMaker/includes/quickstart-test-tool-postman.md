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
ms.openlocfilehash: 0b442c8cccf8ee9ed5194a7d3dfbfb4d7aa055a3
ms.sourcegitcommit: 4c831e768bb43e232de9738b363063590faa0472
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2019
ms.locfileid: "74424422"
---
Bu Postman tabanlı hızlı başlangıç, bilgi tabanınızdan yanıt alma konusunda size yol gösterir.

## <a name="prerequisites"></a>Önkoşullar

* En son [**Postman**](https://www.getpostman.com/).
* Bir [soru-cevap oluşturma hizmetine](../How-To/set-up-qnamaker-service-azure.md) sahip olmanız ve [sorular ve yanıtlar ile Bilgi Bankası](../Tutorials/create-publish-query-in-portal.md)etmeniz gerekir. 

## <a name="publish-to-get-endpoint"></a>Uç nokta almak için Yayımla

Bilgi tabanınızdan bir soruya yanıt oluşturmaya hazırsanız, bilgi bankaınızı [yayımlayın](../Quickstarts/create-publish-knowledge-base.md#publish-the-knowledge-base) .

## <a name="use-production-endpoint-with-postman"></a>Üretim uç noktasını Postman ile kullanma

Bilgi Bankası 'niz yayımlandığında **Yayımla** sayfası, yanıt oluşturmak için http isteği ayarlarını görüntüler. Varsayılan Görünüm [Postman](https://www.getpostman.com)'dan bir yanıt oluşturmak için gereken ayarları gösterir.

Aşağıdaki resimdeki sarı sayılar, aşağıdaki adımlarda kullanılacak ad/değer çiftlerini gösterir.

[![sonuçları Yayımla](../media/qnamaker-quickstart-get-answer-with-postman/publish-settings.png)](../media/qnamaker-quickstart-get-answer-with-postman/publish-settings.png#lightbox)

Postman ile yanıt oluşturmak için aşağıdaki adımları izleyin:

1. Postman 'yi açın. Bir yapı taşı seçmeniz istenirse, **temel istek** yapı taşını seçin. **İstek adını** `Generate QnA Maker answer`ve **koleksiyonu** `Generate QnA Maker answers`olarak ayarlayın. Bir koleksiyona kaydetmek istemiyorsanız **iptal** düğmesini seçin.
1. Çalışma alanında, **Post**'un http yöntemini seçin.

    [Postman 'Da ![, POST yöntemini ayarla](../media/qnamaker-quickstart-get-answer-with-postman/postman-select-post-method.png)](../media/qnamaker-quickstart-get-answer-with-postman/postman-select-post-method.png#lightbox)

1. URL için, tüm URL 'YI oluşturmak için ana bilgisayar değerini (görüntüden #2) ve gönderi değerini (görüntüden #1) birleştirin. Tüm örnek URL şöyle görünür: 

    `https://qnamaker-f0.azurewebsites.net/qnamaker/knowledgebases/e1115f8c-d01b-4698-a2ed-85b0dbf3348c/generateAnswer`

    [Postman 'Da ![, URL 'YI tamamen ayarlayın](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-method-and-url.png)](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-method-and-url.png#lightbox)

1. URL 'nin altındaki **üstbilgiler** sekmesini seçin ve **Toplu Düzenle**' yi seçin. 

1. Üstbilgileri (#3 ve #4 görüntüden) metin alanına kopyalayın.

    [Postman 'Da ![üst bilgileri ayarlayın](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-headers.png)](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-headers.png#lightbox)

1. **Gövde** sekmesini seçin.
1. **Ham** biçimi seçin ve soruyu temsıl eden JSON (#5 görüntüden) girin.

    `{"question":"How do I programmatically update my Knowledge Base?"}`

    [Postman 'Da ![Body JSON değerini ayarlayın](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-body-json-value.png)](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-body-json-value.png#lightbox)

1. **Gönder** düğmesini seçin.
1. Yanıt, istemci uygulaması için önemli olabilecek diğer bilgilerle birlikte yanıtı içerir. 

    [Postman 'Da ![Body JSON değerini ayarlayın](../media/qnamaker-quickstart-get-answer-with-postman/receive-postman-response.png)](../media/qnamaker-quickstart-get-answer-with-postman/receive-postman-response.png#lightbox)

## <a name="use-staging-endpoint"></a>Hazırlama uç noktası kullan

Hazırlama uç noktasından bir yanıt almak istiyorsanız URL 'YI `isTest` Body özelliğine ekleyin.
