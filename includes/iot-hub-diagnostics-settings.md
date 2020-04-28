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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75750687"
---
### <a name="enable-logging-with-diagnostics-settings"></a>Tanılama ayarlarıyla günlüğü etkinleştir

[!INCLUDE [updated-for-az](./updated-for-az.md)]

1. [Azure Portal](https://portal.azure.com) oturum açın ve IoT Hub 'ınıza gidin.

2. **Tanılama ayarları**' nı seçin.

3. **Tanılamayı aç '** ı seçin.

   ![Tanılamayı açma](./media/iot-hub-diagnostics-settings/turnondiagnostics.png)

4. Tanılama ayarlarına bir ad verin.

5. Günlükleri nereye göndermek istediğinizi seçin. Üç seçenekten herhangi bir birleşimini seçebilirsiniz:

   * Bir depolama hesabına arşivle
   * Bir olay hub'ına akış yap
   * Log Analytics’e gönderme

6. Hangi işlemleri izlemek istediğinizi seçin ve bu işlemler için günlükleri etkinleştirin. Tanılama ayarlarının rapor aldığı işlemler şunlardır:

   * Bağlantılar
   * Cihaz telemetrisi
   * Buluttan cihaza iletiler
   * Cihaz kimliği işlemleri
   * Dosya karşıya yüklemeleri
   * İleti yönlendirme
   * Buluttan cihaza ikizi işlemleri
   * Cihazdan buluta ikizi işlemleri
   * İkizi işlemleri
   * İş işlemleri
   * Doğrudan yöntemler  
   * Dağıtılmış izleme (Önizleme)
   * Yapılandırmalar
   * Cihaz akışları
   * Cihaz ölçümleri

6. Yeni ayarları kaydedin. 

Tanılama ayarlarını PowerShell ile açmak istiyorsanız aşağıdaki kodu kullanın:

```azurepowershell
Connect-AzAccount
Select-AzSubscription -SubscriptionName <subscription that includes your IoT Hub>
Set-AzDiagnosticSetting -ResourceId <your resource Id> -ServiceBusRuleId <your service bus rule Id> -Enabled $true
```

Yeni ayarlar yaklaşık 10 dakika içinde etkili olur. Bundan sonra, Günlükler **Tanılama ayarları** dikey penceresinde yapılandırılan arşivleme hedefi ' nde görüntülenir. Tanılamayı yapılandırma hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızdan günlük verilerini toplama ve](../articles/azure-monitor/platform/platform-logs-overview.md)kullanma.
