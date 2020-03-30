---
title: Azure Bildirim Hub'larında Microsoft Anında İşleme Bildirim Hizmetini Yapılandırma | Microsoft Dokümanlar
description: Azure bildirim merkezi için Microsoft Anında Bildirim Hizmeti ayarlarını nasıl yapılandırıyarıştırmayı öğrenin.
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
ms.openlocfilehash: 99f29e7910fe6070c6202f6a936173455f979732
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80127334"
---
# <a name="configure-microsoft-push-notification-service-mpns-settings-in-the-azure-portal"></a>Azure portalında Microsoft Anında Bildirim Hizmeti (MPNS) ayarlarını yapılandırma

Bu makalede, Azure portalını kullanarak bir Azure bildirim merkezi için Microsoft Anında İşleme Bildirim Hizmeti (MPNS) ayarlarını nasıl yapılandırabileceğiniz gösterilmektedir. 

## <a name="prerequisites"></a>Ön koşullar
Daha önce bir bildirim hub'ı oluşturmadıysanız, şimdi bir bildirim hub'ı oluşturun. Daha fazla bilgi için [bkz.](create-notification-hub-portal.md) 

## <a name="configure-microsoft-push-notification-service-mpns"></a>Microsoft Anında İtme Bildirim Hizmeti (MPNS) yapılandırma

Aşağıdaki yordam, bir bildirim hub'ı için Microsoft Anında İtme Bildirim Hizmeti (MPNS) ayarlarını yapılandırmanız için adımlar verir: 

1. Azure portalında, **Bildirim Merkezi** sayfasında sol menüde Windows **Phone (MPNS)** seçeneğini belirleyin.
1. Kimlik doğrulamamış veya kimlik doğrulaması yapılan anında iletme bildirimlerini etkinleştirin:

   a. Kimlik doğrulamamış anında iletme bildirimlerini etkinleştirmek **için, kimliği doğrulanmamış itme Kaydet'i etkinleştir'i** > **Save**seçin.

      ![Kimlik doğrulamamış anında iletme bildirimlerinin nasıl etkinleştirilen gösteren ekran görüntüsü](./media/notification-hubs-windows-phone-get-started/azure-portal-unauth.png)

   b. Kimlik doğrulaması anında iletme bildirimlerini etkinleştirmek için:
      * Araç çubuğunda **Sertifika Yükle'yi**seçin.
      * Dosya simgesini seçin ve ardından sertifika dosyasını seçin.
      * Sertifika için parola belirtin.
      * **Tamam'ı**seçin.
      * Windows **Phone (MPNS)** sayfasında **Kaydet'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar
Azure Bildirim Hub'ları ve Microsoft Anında Iletme Bildirim Hizmeti (MPNS) kullanarak Windows Phone aygıtlarına bildirimleri itme yle ilgili adım adım yönergeleri içeren bir öğretici için, [Bildirim Hub'larını kullanarak Windows Phone uygulamalarına anında iletme](notification-hubs-windows-mobile-push-notifications-mpns.md)bildirimleri'ne bakın.

