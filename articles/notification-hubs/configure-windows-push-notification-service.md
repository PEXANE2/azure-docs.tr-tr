---
title: Azure Bildirim Hub'larında Windows Anında Bildirim Hizmetini Yapılandırma | Microsoft Dokümanlar
description: Azure bildirim merkezi için Windows Anında Bildirim Hizmeti ayarlarını nasıl yapılandırıyarınız gerektiğini öğrenin.
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
ms.openlocfilehash: 73304e191242725c80204efb132c26aede9ce7e9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80127318"
---
# <a name="configure-windows-push-notification-service-settings-in-the-azure-portal"></a>Azure portalında Windows Anında Bildirim Hizmeti ayarlarını yapılandırma

Bu makalede, Azure portalını kullanarak bir Azure bildirim merkezi için Windows Bildirim Hizmeti (WNS) ayarlarını nasıl yapılandırabileceğiniz gösterilmektedir.  

## <a name="prerequisites"></a>Ön koşullar
Daha önce bir bildirim hub'ı oluşturmadıysanız, şimdi bir bildirim hub'ı oluşturun. Daha fazla bilgi için [bkz.](create-notification-hub-portal.md) 

## <a name="configure-windows-push-notification-service-wns"></a>Windows Anında İtme Bildirim Hizmeti (WNS) yapılandırma

Aşağıdaki yordam, bir bildirim hub'ı için Windows Anında İtme Bildirim Hizmeti (WNS) ayarlarını yapılandırmanız için adımlar verir: 

1. Azure portalında, **Bildirim Merkezi** sayfasında sol menüde **Windows (WNS)** seçeneğini belirleyin.
2. **Paket SID** ve **Güvenlik Anahtarı**için değerleri girin.
3. **Kaydet'i**seçin.

   ![Paket SID ve Güvenlik Anahtarı kutularını gösteren ekran görüntüsü](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-configure-wns.png)

## <a name="next-steps"></a>Sonraki adımlar
Azure Bildirim Hub'ları ve Windows Anında Iletme Bildirim Hizmeti (WNS) kullanarak Evrensel Windows Platformu uygulamalarına bildirimleri iletme için adım adım yönergeleri olan bir öğretici için, [Azure Bildirim Hub'larını kullanarak UWP uygulamalarına bildirim gönder'e](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md)bakın.


