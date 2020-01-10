---
title: 'Hızlı başlangıç: konuşmayı, amaçları ve varlıkları tanıma, Python-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 12/27/2019
ms.author: erhopf
ms.openlocfilehash: aee5d6e1f6ed0519df7d1059b39f215d9f0d9091
ms.sourcegitcommit: 51ed913864f11e78a4a98599b55bbb036550d8a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2020
ms.locfileid: "75660575"
---
Amaç tanıma hızlı başlangıcı ' nı tamamlayabilmeniz için, LUSıS önizleme portalını kullanarak bir LUO hesabı ve proje oluşturmanız gerekir. Bu hızlı başlangıçta yalnızca bir LUSıS aboneliği gerekir. Konuşma hizmeti aboneliği gerekli değildir.

Yapmanız gereken ilk şey, LUSıS önizleme portalını kullanarak bir LUO hesabı ve uygulaması oluşturmaktır. Oluşturduğunuz LUU uygulaması, giriş otomasyonu için, amaçları, varlıkları ve örnek utbotları sağlayan önceden oluşturulmuş bir etki alanı kullanacaktır. İşiniz bittiğinde, bulutta çalışan ve konuşma SDK 'sını kullanarak çağırabileceğinizi bir LUO uç noktası olur. 

LUSıS uygulamanızı oluşturmak için bu yönergeleri izleyin: 

* <a href="https://docs.microsoft.com/en-us/azure/cognitive-services/luis/luis-get-started-create-app" target="_blank">Hızlı başlangıç: önceden oluşturulmuş etki alanı uygulaması oluşturma</a>

İşiniz bittiğinde üç şey olması gerekir: 

* LUSıS anahtarınız
* LUSıS bölgeniz
* LUSıS uygulama KIMLIĞINIZ

Burada, bu bilgileri [lusıs Önizleme portalında](https://preview.luis.ai/)bulabilirsiniz:

1. LUSıS önizleme portalından **Yönet**' i ve ardından **Azure kaynakları**' nı seçin. Bu sayfada, LUSıS anahtarınızı ve konumunuzu (bazen _bölge_olarak adlandırılır) bulacaksınız.  

   > [!div class="mx-imgBorder"]
   > ![LUSıS anahtar ve konumu](../../../media/luis/luis-key-region.png)

2. Anahtarınızı ve konumunuzu aldıktan sonra uygulama KIMLIĞI gerekir. **Uygulama ayarlarını** seçin--uygulama kimliğiniz bu sayfada bulunur.

   > [!div class="mx-imgBorder"]
   > ![LUSıS uygulama KIMLIĞI](../../../media/luis/luis-app-id.png)