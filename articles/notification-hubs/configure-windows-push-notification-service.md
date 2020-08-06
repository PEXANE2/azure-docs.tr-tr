---
title: Azure Notification Hubs Windows anında bildirim hizmeti 'ni yapılandırma | Microsoft Docs
description: Bir Azure Notification Hub 'ı için Windows anında bildirim hizmeti ayarlarını yapılandırmayı öğrenin.
services: notification-hubs
author: sethmanheim
manager: femila
ms.service: notification-hubs
ms.workload: mobile
ms.topic: article
ms.date: 08/04/2020
ms.author: sethm
ms.reviewer: thsomasu
ms.lastreviewed: 03/25/2019
ms.openlocfilehash: 62d73a23fff3bd15d873a579d451b2f91509b7fe
ms.sourcegitcommit: 5a37753456bc2e152c3cb765b90dc7815c27a0a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87758739"
---
# <a name="configure-windows-push-notification-service-settings-in-the-azure-portal"></a>Azure portal Windows anında Iletme bildirimi hizmeti ayarlarını yapılandırma

Bu makalede, Azure portal kullanarak bir Azure Notification Hub 'ı için Windows Notification hizmeti (WNS) ayarlarının nasıl yapılandırılacağı gösterilmektedir.  

## <a name="prerequisites"></a>Önkoşullar

Henüz bir Bildirim Hub 'ı oluşturmadıysanız, şimdi bir tane oluşturun. Daha fazla bilgi için [Azure Portal Azure Notification Hub 'ı oluşturma](create-notification-hub-portal.md)konusuna bakın.

## <a name="configure-windows-push-notification-service-wns"></a>Windows anında Iletme Bildirimi hizmetini yapılandırma (WNS)

Aşağıdaki yordamda, bir Bildirim Hub 'ı için Windows anında bildirim hizmeti (WNS) ayarlarını yapılandırma adımları açıklanmaktadır:

1. Azure portal, **Bildirim Hub** 'ı sayfasında, sol taraftaki menüden **Windows (WNS)** seçeneğini belirleyin.
2. **Paket SID 'si** ve **güvenlik anahtarı**için değerler girin.
3. **Kaydet**’i seçin.

   ![Paket SID ve güvenlik anahtarı kutularını gösteren ekran görüntüsü](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-configure-wns.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure Notification Hubs ve Windows anında Iletme bildirimi hizmeti 'ni (WNS) kullanarak Evrensel Windows Platformu uygulamalarına anında iletme bildirimleri göndermeye yönelik adım adım yönergeler için, bkz. [azure Notification Hubs kullanarak UWP uygulamalarına bildirim gönderme](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md).
