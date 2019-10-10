---
title: App Service Mobile Apps ile tümleştirme
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
ms.openlocfilehash: 8c3bc90b282092ede0e924d32b50b67e5c4e22b8
ms.sourcegitcommit: 824e3d971490b0272e06f2b8b3fe98bbf7bfcb7f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2019
ms.locfileid: "72244495"
---
# <a name="integration-with-app-service-mobile-apps"></a>App Service Mobile Apps ile tümleştirme

Azure hizmetlerinde sorunsuz ve geri yükleme deneyimini kolaylaştırmak için, [App Service Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) Notification Hubs kullanarak anında iletme bildirimleri için yerleşik desteğe sahiptir. [App Service Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) , kurumsal geliştiriciler ve sistem tümleştiricileri için mobil geliştiricilere zengin bir özellik kümesi sağlayan yüksek düzeyde ölçeklenebilir, genel olarak kullanılabilir bir mobil uygulama geliştirme platformu sunar.

Visual Studio App Center, günümüzde geliştiriciler için daha modern bir mobil çözüm sunar. Mobil uygulama geliştirmeye yönelik uçtan uca ve tümleşik hizmetler merkezi 'ni destekler. Geliştiriciler, sürekli tümleştirme ve teslim işlem hattı ayarlamak için **oluşturma**, **Test** etme ve **dağıtma** hizmetlerini kullanabilir. Uygulama dağıtıldıktan sonra, geliştiriciler **analiz** ve **Tanılama** hizmetlerini kullanarak uygulamasının durumunu ve kullanımını izleyebilir ve **Push** hizmetini kullanarak kullanıcılarla etkileşime geçebilir. Geliştiriciler, uygulama verilerini bulutta kalıcı hale getirmek ve eşitlemek için kullanıcıların ve **veri** hizmetinin kimliklerini doğrulamak üzere **kimlik** doğrulamasından faydalanabilir.

> [!NOTE]
> Mobil uygulamanızda bulut hizmetlerini tümleştirmek istiyorsanız bugün [App Center](https://appcenter.ms/signup?utm_source=NotificationHubs&utm_medium=Azure&utm_campaign=docs) kaydolun.

Mobile Apps geliştiriciler aşağıdaki iş akışıyla Notification Hubs kullanabilir:

1. Cihaz PNS tanıtıcısını al
2. Cihazı uygun Mobile Apps Istemci SDK 'Sı kayıt API 'siyle Notification Hubs kaydetme

    > [!NOTE]
    > Mobile Apps, güvenlik amacıyla kayıtlarda tüm etiketleri uzağa kayacağını unutmayın. Etiketleri cihazlarla ilişkilendirmek için doğrudan arka ucunuzdaki Notification Hubs çalışın.

3. Notification Hubs ile uygulama arka ucunuzdaki bildirimleri gönderin

Aşağıda, bu tümleştirmeyle geliştiricilere getirilen bazı kolaylığı verilmiştir:

- **Mobile Apps Istemci SDK**'ları: Bu çok platformlu SDK 'lar kayıt Için basit API 'ler sağlar ve Mobil uygulamayla bağlantılı Bildirim Hub 'ına otomatik olarak konuşur. Geliştiricilerin Notification Hubs kimlik bilgilerini incelemek ve ek bir hizmetle çalışması gerekmez.
  - *Kullanıcıya gönder*: SDK 'lar, Kullanıcı senaryosuna gönderimi etkinleştirmek için, belirtilen cihazı Mobile Apps kimliği DOĞRULANMıŞ kullanıcı kimliğiyle otomatik olarak etiketleyin.
  - *Cihaza gönder*: sdk 'Lar MOBILE Apps yükleme kimliğini otomatik olarak, Notification Hubs kayıt yapmak için GUID olarak kullanır, geliştiricilerin birden çok hizmet GUID 'sini koruma sorunu.
- **Yükleme modeli**: Mobile Apps, anında iletme bildirim hizmetleri ile hizalanan ve kullanımı kolay bir JSON yüklemesinde bir cihazla ilişkili tüm gönderme özelliklerini temsil etmek için Notification Hubs ' en son gönderim modeliyle birlikte çalışıyor.
- **Esneklik**: geliştiriciler, her zaman tümleştirmeyle birlikte Notification Hubs doğrudan çalışmayı tercih edebilir.
- **[Azure Portal](https://portal.azure.com)'de tümleşik deneyim**: bir özellik olarak gönderim, Mobile Apps görsel olarak temsil edilir ve geliştiriciler Mobile Apps üzerinden ilişkili Bildirim Hub 'ı ile kolayca çalışabilir.
