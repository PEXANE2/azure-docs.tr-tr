---
title: include dosyası
description: include dosyası
author: anthonychu
ms.service: signalr
ms.topic: include
ms.date: 03/04/2019
ms.author: antchu
ms.custom: include file
ms.openlocfilehash: 73d4d4e1a5f148dce6099b2d747ee1c290bcf7c1
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "67188742"
---
## <a name="run-the-web-application"></a>Web uygulamasını çalıştırma

1. Sizin için GitHub'da tek sayfalı örnek bir web uygulaması barındırılmaktadır. Tarayıcınızı ' [https://azure-samples.github.io/signalr-service-quickstart-serverless-chat/demo/chat-v2/](https://azure-samples.github.io/signalr-service-quickstart-serverless-chat/demo/chat-v2/)için açın.

    > [!NOTE]
    > HTML dosyasının kaynağı [/docs/demo/chat-v2/index.html](https://github.com/Azure-Samples/signalr-service-quickstart-serverless-chat/blob/master/docs/demo/chat-v2/index.html)adresinde bulunmaktadır.

1. İşlev uygulamasının temel URL'si istendiğinde `http://localhost:7071` URL’sini girin.

1. İstenildiğinde bir kullanıcı adı girin.

1. Web uygulaması, Azure SignalR Hizmetine bağlanmak amacıyla bağlantı bilgilerini almak için işlev uygulamasındaki *GetSignalRInfo* işlevini çağırır. Bağlantı tamamlandığında sohbet iletisi giriş kutusu görünür.

1. Bir ileti yazın ve Enter tuşuna basın. Uygulama, iletiyi Azure işlev uygulamasındaki *SendMessage* işlevine gönderir, bu da bağlı tüm istemcilere iletiyi yaymak için SignalR çıkış bağlamasını kullanır. Her şey düzgün çalışıyorsa iletinin uygulamada görünmesi gerekir.

    ![Uygulamayı çalıştırma](../media/signalr-quickstart-azure-functions-csharp/signalr-quickstart-run-application.png)

1. Farklı bir tarayıcı penceresinde web uygulamasının başka bir örneğini açın. Gönderilen tüm iletilerin uygulamanın tüm örneklerinde göründüğünü görürsünüz.

> [!IMPORTANT]
> HTML sayfası HTTPS kullanılarak sunulduğundan, ancak yerel Azure İşlevler çalışma süresi varsayılan olarak HTTP'yi kullandığından, tarayıcınız (Firefox gibi) web sayfasından işlevlerinize istekleri engelleyen karışık içerikli bir ilke uygulayabilir. Bunu çözmek için, bu kısıtlamaya sahip olmayan bir tarayıcı kullanın veya */docs/demo/chat-v2* dizininde [http-server](https://www.npmjs.com/package/http-server) gibi yerel bir HTTP sunucusu başlatın. Kaynağın `CORS` *local.settings.json'daki*ayara eklenmiştir.