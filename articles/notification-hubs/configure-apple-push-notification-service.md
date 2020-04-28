---
title: Azure Notification Hubs Apple Anında İletilen Bildirim Servisi yapılandırma | Microsoft Docs
description: Apple Anında İletilen Bildirim Servisi (APNS) ayarlarını kullanarak bir Azure Notification Hub 'ını yapılandırmayı öğrenin.
services: notification-hubs
author: sethmanheim
manager: femila
editor: jwargo
ms.service: notification-hubs
ms.workload: mobile
ms.topic: article
ms.date: 03/25/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 03/25/2019
ms.openlocfilehash: eb1122ba3de0002507589d3e607d1e39d905c308
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80127528"
---
# <a name="configure-apple-push-notification-service-settings-for-a-notification-hub-in-the-azure-portal"></a>Azure portal bir Bildirim Hub 'ı için Apple Anında İletilen Bildirim Servisi ayarlarını yapılandırma

Bu makalede, Azure portal kullanarak bir Azure Notification Hub 'ı için Apple Anında İletilen Bildirim Servisi (APNS) ayarlarının nasıl yapılandırılacağı gösterilmektedir. 

## <a name="prerequisites"></a>Ön koşullar
Henüz bir Bildirim Hub 'ı oluşturmadıysanız, şimdi bir tane oluşturun. Daha fazla bilgi için [Azure Portal Azure Notification Hub 'ı oluşturma](create-notification-hub-portal.md)konusuna bakın. 

## <a name="configure-apple-push-notification-service"></a>Apple Anında İletilen Bildirim Servisi Yapılandır

Aşağıdaki yordam bir Bildirim Hub 'ı için Apple Anında İletilen Bildirim Servisi (APNS) ayarlarını yapılandırma adımları sunar:

1. Azure portal, **Bildirim Hub** 'ı sayfasında, sol taraftaki menüden **Apple (APNs)** öğesini seçin.

1. **Kimlik doğrulama modu**Için, **sertifika** ya da **belirteç**seçin.

   a. **Sertifika**seçerseniz:
   * Dosya simgesini seçin ve ardından karşıya yüklemek istediğiniz *. p12* dosyasını seçin.
   * Bir parola girin.
   * **Korumalı alan** modunu seçin. Ya da uygulamanızı mağazadan satın alan kullanıcılara anında iletme bildirimleri göndermek için **Üretim** modu ' nu seçin.

     ![Azure portal bir APNS sertifika yapılandırmasının ekran görüntüsü](./media/configure-apple-push-notification-service/notification-hubs-apple-config-cert.png)

   b. **Belirteç**' yi seçerseniz:

   * **Anahtar kimliği**, **paket KIMLIĞI**, **Takım Kimliği**ve **belirteç**değerlerini girin.
   * **Korumalı alan** modunu seçin. Ya da uygulamanızı mağazadan satın alan kullanıcılara anında iletme bildirimleri göndermek için **Üretim** modu ' nu seçin.

     ![Azure portal bir APNS belirteç yapılandırması ekran görüntüsü](./media/configure-apple-push-notification-service/notification-hubs-apple-config-token.png)

## <a name="next-steps"></a>Sonraki adımlar
İOS cihazlarına bildirim göndermeye yönelik adım adım yönergeler için aşağıdaki makaleye bakın: [Notification Hubs ve APNs kullanarak iOS cihazlarına anında iletme bildirimleri gönderme](notification-hubs-ios-apple-push-notification-apns-get-started.md)
