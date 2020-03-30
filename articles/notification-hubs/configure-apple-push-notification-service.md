---
title: Azure Bildirim Hub'larında Apple Anında Bildirim Hizmetini Yapılandırma | Microsoft Dokümanlar
description: Apple Anında Bildirim Hizmeti (APNS) ayarlarıyla bir Azure bildirim hub'ı nasıl yapılandırıştırmayı öğrenin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80127528"
---
# <a name="configure-apple-push-notification-service-settings-for-a-notification-hub-in-the-azure-portal"></a>Azure portalındaki bir bildirim merkezi için Apple Anında Bildirim Hizmeti ayarlarını yapılandırın

Bu makalede, Azure portalını kullanarak bir Azure bildirim merkezi için Apple Push Bildirim Hizmeti (APNS) ayarlarını nasıl yapılandırabileceğiniz gösterilmektedir. 

## <a name="prerequisites"></a>Ön koşullar
Daha önce bir bildirim hub'ı oluşturmadıysanız, şimdi bir bildirim hub'ı oluşturun. Daha fazla bilgi için [bkz.](create-notification-hub-portal.md) 

## <a name="configure-apple-push-notification-service"></a>Apple Anında İtme Bildirim Hizmetini Yapılandırma

Aşağıdaki yordam, bir bildirim hub'ı için Apple Anında Bildirim Hizmeti (APNS) ayarlarını yapılandırmanız için adımlar verir:

1. Azure portalında, **Bildirim Merkezi** sayfasında sol menüde **Apple (APNS)** seçeneğini belirleyin.

1. **Kimlik Doğrulama Modu**için **Sertifika** veya **Belirteç'i**seçin.

   a. **Sertifika'yı**seçerseniz:
   * Dosya simgesini seçin ve sonra yüklemek istediğiniz *.p12* dosyasını seçin.
   * Parola girin.
   * **Korumalı alan** modunu seçin. Veya uygulamanızı mağazadan satın alan kullanıcılara anında iletme bildirimleri göndermek için **Üretim** modunu seçin.

     ![Azure portalında apns sertifikası yapılandırmasının ekran görüntüsü](./media/configure-apple-push-notification-service/notification-hubs-apple-config-cert.png)

   b. **Token'ı**seçerseniz:

   * **Anahtar Kimliği,** Paket **Kimliği,** **Takım Kimliği**ve **Belirteç**değerlerini girin.
   * **Korumalı alan** modunu seçin. Veya uygulamanızı mağazadan satın alan kullanıcılara anında iletme bildirimleri göndermek için **Üretim** modunu seçin.

     ![Azure portalında apns belirteç yapılandırmasının ekran görüntüsü](./media/configure-apple-push-notification-service/notification-hubs-apple-config-token.png)

## <a name="next-steps"></a>Sonraki adımlar
bildirimleri iOS aygıtlarına iletme için adım adım yönergeleri içeren bir öğretici için aşağıdaki makaleye bakın: [Bildirim Hub'ları ve APNS'leri kullanarak iOS aygıtlarına bildirimleri iletme](notification-hubs-ios-apple-push-notification-apns-get-started.md)
