---
title: App Service Mobile Apps ile tümleştirme Notification Hubs
description: Azure Notification Hubs Azure App Service Mobile Apps nasıl çalıştığını öğrenin.
author: sethmanheim
manager: femila
services: notification-hubs
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: multiple
ms.devlang: multiple
ms.topic: overview
ms.custom: mvc
ms.date: 08/06/2020
ms.author: sethm
ms.reviewer: thsomasu
ms.lastreviewed: 01/04/2019
ms.openlocfilehash: e7042be7e407e8e0827e142ba6878dfff812e1f6
ms.sourcegitcommit: 98854e3bd1ab04ce42816cae1892ed0caeedf461
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2020
ms.locfileid: "88004066"
---
# <a name="integration-with-app-service-mobile-apps"></a>App Service Mobile Apps ile Tümleştirme

Azure hizmetleri genelinde sorunsuz ve geri bir deneyimi kolaylaştırmak için [App Service Mobile Apps](/previous-versions/azure/app-service-mobile/app-service-mobile-value-prop) Azure Notification Hubs kullanan bildirimler için yerleşik desteğe sahiptir. [App Service Mobile Apps](/previous-versions/azure/app-service-mobile/app-service-mobile-value-prop) , kurumsal geliştiriciler ve sistem tümleştiricileri için mobil geliştiricilere zengin bir özellik kümesi sağlayan yüksek düzeyde ölçeklenebilir, genel olarak kullanılabilir bir mobil uygulama geliştirme platformu sunar.

Mobile Apps geliştiriciler aşağıdaki iş akışıyla Notification Hubs kullanabilir:

1. Cihaz PNS tanıtıcısını alın.
2. Cihazı Mobile Apps istemci SDK 'Sı kayıt API 'Leri kullanarak Notification Hubs kaydedin.

    > [!NOTE]
    > Mobile Apps'in güvenlik amacıyla kayıtlardaki tüm etiketleri kaldırdığını unutmayın. Etiketleri cihazlarla ilişkilendirmek için doğrudan arka ucunuzdan Notification Hubs ile çalışın.

3. Notification Hubs ile uygulama arka ucunuzdaki bildirimleri gönderin.

Bu tümleştirmenin sağladığı avantajlar şunlardır:

- **Mobile Apps Istemci SDK**'ları: Bu çok platformlu SDK 'lar kayıt Için API 'ler sağlar ve Mobil uygulamayla bağlantılı Bildirim Hub 'ı ile iletişim kurar. Notification Hubs kimlik bilgilerine ihtiyacınız yoktur veya ek bir hizmetle çalışmanız gerekmez.
  - *Kullanıcıya gönder*: SDK 'lar, "kullanıcıya gönderim" senaryosunu etkinleştirmek için belirtilen cihazı Mobile Apps kimliği doğrulanmış BIR kullanıcı kimliğiyle otomatik olarak etiketleyin.
  - *Cihaza gönder*: SDK 'lar, Notification Hubs kaydettirmek Için MOBILE Apps yükleme kimliğini otomatik olarak bir GUID olarak kullanır, bu nedenle birden çok hizmet GUID 'sini sürdürmenize gerek yoktur.
- **Yükleme modeli**: Mobile Apps, anında iletme bildirim hizmetleri ile hizalanan ve kullanımı kolay bir JSON yüklemesinde bir cihazla ilişkili tüm gönderme özelliklerini göstermek için Notification Hubs en son gönderim modeliyle birlikte çalışıyor.
- **Esneklik**: Geliştiriciler, yerinde tümleştirme söz konusu olduğunda bile her zaman için Notification Hubs ile doğrudan çalışmayı tercih edebilir.
- ** [Azure Portal](https://portal.azure.com)tümleşik deneyim**: bir özellik olarak gönderim, Mobile Apps görsel olarak temsil edilir ve geliştiriciler Mobile Apps üzerinden ilişkili Bildirim Hub 'ı ile kolayca çalışabilir.
