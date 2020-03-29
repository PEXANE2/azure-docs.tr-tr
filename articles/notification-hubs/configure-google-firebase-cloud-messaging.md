---
title: Azure Bildirim Hub'larında Google Firebase Bulut Mesajlaşmalarını Yapılandırma | Microsoft Dokümanlar
description: Google Firebase Bulut Mesajlaşma ayarlarıyla bir Azure bildirim merkezini nasıl yapılandırabilirsiniz öğrenin.
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
ms.openlocfilehash: 1adbce654bc5c057270df9a874911731a0135034
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80127461"
---
# <a name="configure-google-firebase-settings-for-a-notification-hub-in-the-azure-portal"></a>Azure portalındaki bir bildirim merkezi için Google Firebase ayarlarını yapılandırın

Bu makalede, Azure portalını kullanarak bir Azure bildirim merkezi için Google Firebase Bulut İletisi (FCM) ayarlarını nasıl yapılandırabileceğiniz gösterilmektedir.  

## <a name="prerequisites"></a>Ön koşullar
Daha önce bir bildirim hub'ı oluşturmadıysanız, şimdi bir bildirim hub'ı oluşturun. Daha fazla bilgi için [bkz.](create-notification-hub-portal.md) 

## <a name="configure-google-firebase-cloud-messaging-fcm"></a>Google Firebase Bulut Mesajlaşma (FCM) yapılandırma

Aşağıdaki yordam, bir bildirim hub'ı için Google Firebase Bulut Mesajlaşma (FCM) ayarlarını yapılandırmanız için adımlar verir: 

1. Azure portalında, **Bildirim Merkezi** sayfasında sol menüde **Google 'ı (GCM/FCM)** seçin. 
2. Daha önce kaydettiğiniz FCM projesi için **API Anahtarını** yapıştırın. 
3. **Kaydet'i**seçin. 

   ![Google FCM için Bildirim Hub'larının nasıl yapılandırılabildiğini gösteren ekran görüntüsü](./media/notification-hubs-android-push-notification-google-fcm-get-started/fcm-server-key.png)

## <a name="next-steps"></a>Sonraki adımlar
Azure Bildirim Hub'larını ve Google Firebase Bulut Mesajlaşmasını kullanarak Android cihazlara bildirimleri itme yle ilgili adım adım talimatlar içeren bir öğretici için Bildirim [Hub'larını ve Google FCM'yi kullanarak Android cihazlara anında iletme](notification-hubs-android-push-notification-google-fcm-get-started.md)bildirimleri'ne bakın.

