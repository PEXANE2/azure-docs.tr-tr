---
title: App Service Mobile Apps ile Tümleştirme
description: Azure Notification Hubs Azure App Service Mobile Apps nasıl çalıştığını öğrenin.
author: sethmanheim
manager: femila
editor: jwargo
services: notification-hubs
documentationcenter: ''
ms.assetid: 83132dff-a01d-4b31-a426-b57496852b81
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: multiple
ms.devlang: multiple
ms.topic: overview
ms.custom: mvc
ms.date: 01/04/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 01/04/2019
ms.openlocfilehash: d6747193b8c82119e45a24e3e4bffc065db14e51
ms.sourcegitcommit: 7df70220062f1f09738f113f860fad7ab5736e88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2019
ms.locfileid: "71212164"
---
# <a name="integration-with-app-service-mobile-apps"></a>App Service Mobile Apps ile Tümleştirme

> [!NOTE]
> Microsoft, en son işletim sistemi sürümü, hata düzeltmeleri, belge geliştirmeleri ve topluluk PR incelemeleri için destek de dahil olmak üzere Azure App Service Mobile Apps tam olarak desteklemeye yöneliktir. Lütfen ürün ekibinin Azure Mobile Apps yönelik yeni özellik çalışmaunda Şu anda yatırım olmadığını unutmayın. Azure Mobile Apps tüm alanlarında topluluk katkılarını çok iyi biliyoruz.

Azure hizmetleri genelinde kesintisiz ve birleştirici bir deneyimi kolaylaştırmak amacıyla [App Service Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md)'in Notification Hubs'ı kullanan anında iletme bildirimleri için yerleşik desteği mevcuttur. [App Service Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md), Kurumsal Geliştiriciler ve Sistem Tümleştiricileri için mobil geliştiricilere zengin bir özellik kümesi sağlayan, yüksek düzeyde ölçeklenebilir, global olarak kullanılabilir bir mobil uygulama geliştirme platformu sunar.

Mobile Apps geliştiricileri Notification Hubs'ı aşağıdaki iş akışı ile kullanabilir:

1. Cihaz PNS tanıtıcısını alma
2. Uygun Mobile Apps İstemci SDK'sı kayıt API'si yoluyla Notification Hubs'a cihazı kaydetme

    > [!NOTE]
    > Mobile Apps'in güvenlik amacıyla kayıtlardaki tüm etiketleri kaldırdığını unutmayın. Etiketleri cihazlarla ilişkilendirmek için doğrudan arka ucunuzdan Notification Hubs ile çalışın.

3. Uygulama arka ucunuzdan Notification Hubs ile bildirimler gönderme

Bu tümleştirme ile geliştiricilere sağlanan bazı kolaylıklar şunlardır:

- **Mobile Apps Istemci SDK 'ları**: Bu çoklu platform SDK'ları kayıt için basit API'ler sağlar ve mobil uygulama ile bağlantılı bildirim hub'ı ile otomatik olarak konuşur. Geliştiricilerin Notification Hubs kimlik bilgilerini sorgulaması ve ek bir hizmet ile çalışması gerekmez.
  - *Kullanıcıya gönder*: SDK'lar, kullanıcı senaryosuna gönderimi sağlamak için, belirli bir cihazı otomatik olarak Mobile Apps kimliği doğrulanmış Kullanıcı Kimliği ile etiketler.
  - *Cihaza gönder*: SDK'lar, Notification Hubs'a kaydetmek için Mobile Apps Yükleme Kimliği'ni otomatik olarak GUID gibi kullanır. Böylece, geliştiricileri birden çok hizmet GUID'i saklama zahmetinden kurtarır.
- **Yükleme modeli**: Mobile Apps, Anında İletme Bildirimi Hizmetleri ile hizalanan ve kullanımı kolay olan bir JSON Yüklemesi'nde, bir cihaz ile ilişkili tüm gönderim özelliklerini temsil etmek için Notification Hubs'ın en son gönderim modeli ile çalışır.
- **Esneklik**: Geliştiriciler, yerinde tümleştirme söz konusu olduğunda bile her zaman için Notification Hubs ile doğrudan çalışmayı tercih edebilir.
- **[Azure Portal](https://portal.azure.com)'de tümleşik deneyim**: Mobile Apps'te gönderim özelliği görsel olarak temsil edilir ve geliştiriciler Mobile Apps aracılığıyla ilişkili bildirim hub'ı ile kolayca çalışabilir.
