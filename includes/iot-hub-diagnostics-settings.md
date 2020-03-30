---
title: include dosyası
description: include dosyası
services: iot-hub
author: dominicbetts
ms.service: iot-hub
ms.topic: include
ms.date: 02/20/2019
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 3778ec2fac13aee29ce361402a535ca70fd56c33
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "75750687"
---
### <a name="enable-logging-with-diagnostics-settings"></a>Tanılama ayarlarıyla günlüğe kaydetmeyi etkinleştirme

[!INCLUDE [updated-for-az](./updated-for-az.md)]

1. [Azure portalında](https://portal.azure.com) oturum açın ve IoT hub'ınıza gidin.

2. **Tanılama ayarlarını**seçin.

3. **Tanılamayı Aç'ı**seçin.

   ![Tanılamayı açma](./media/iot-hub-diagnostics-settings/turnondiagnostics.png)

4. Tanılama ayarlarına bir ad verin.

5. Günlükleri göndermek istediğiniz yeri seçin. Üç seçeneğin herhangi bir birleşimini seçebilirsiniz:

   * Bir depolama hesabına arşivle
   * Bir olay hub'ına akış yap
   * Log Analytics’e gönderme

6. Hangi işlemleri izlemek istediğinizi seçin ve bu işlemler için günlükleri etkinleştirin. Tanılama ayarlarının rapor edebileceği işlemler şunlardır:

   * Bağlantılar
   * Cihaz telemetrisi
   * Buluttan cihaza iletiler
   * Cihaz kimlik işlemleri
   * Dosya karşıya yüklemeleri
   * İleti yönlendirme
   * Bulut-aygıtikiz işlemleri
   * Aygıt-buluta ikiz işlemleri
   * İkiz operasyonlar
   * İş operasyonları
   * Doğrudan yöntemler  
   * Dağıtılmış izleme (önizleme)
   * Yapılandırmalar
   * Cihaz akışları
   * Cihaz ölçümleri

6. Yeni ayarları kaydedin. 

PowerShell ile tanılama ayarlarını açmak istiyorsanız, aşağıdaki kodu kullanın:

```azurepowershell
Connect-AzAccount
Select-AzSubscription -SubscriptionName <subscription that includes your IoT Hub>
Set-AzDiagnosticSetting -ResourceId <your resource Id> -ServiceBusRuleId <your service bus rule Id> -Enabled $true
```

Yeni ayarlar yaklaşık 10 dakika içinde etkili olur. Bundan sonra, günlükler **Tanılama ayarları** bıçak üzerinde yapılandırılmış arşiv hedefi görünür. Tanılamayapılandırma hakkında daha fazla bilgi için [bkz.](../articles/azure-monitor/platform/platform-logs-overview.md)
