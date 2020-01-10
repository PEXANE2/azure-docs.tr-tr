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
ms.sourcegitcommit: 380e3c893dfeed631b4d8f5983c02f978f3188bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2020
ms.locfileid: "75750687"
---
### <a name="enable-logging-with-diagnostics-settings"></a>Tanılama ayarlarıyla günlüğü etkinleştir

[!INCLUDE [updated-for-az](./updated-for-az.md)]

1. Oturum [Azure portalında](https://portal.azure.com) ve IOT hub'ınıza gidin.

2. **Tanılama ayarları**' nı seçin.

3. Seçin **tanılamayı Aç**.

   ![Tanılamayı açma](./media/iot-hub-diagnostics-settings/turnondiagnostics.png)

4. Tanılama ayarlarına bir ad verin.

5. Günlükleri nereye göndermek istediğinizi seçin. Üç seçenekten herhangi bir birleşimini seçebilirsiniz:

   * Bir depolama hesabına arşivle
   * Bir olay hub'ına akış yap
   * Log Analytics’e gönderme

6. Hangi işlemleri izlemek istediğinizi seçin ve bu işlemler için günlükleri etkinleştirin. Tanılama ayarlarının rapor aldığı işlemler şunlardır:

   * Bağlantılar
   * Cihaz telemetrisi
   * Bulut-cihaz iletilerini
   * Cihaz kimliği işlemleri
   * Dosya yüklemeleri
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

Yeni ayarları yaklaşık 10 dakika içinde etkinleşir. Bundan sonra, Günlükler **Tanılama ayarları** dikey penceresinde yapılandırılan arşivleme hedefi ' nde görüntülenir. Tanılamayı yapılandırma hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızdan günlük verilerini toplama ve](../articles/azure-monitor/platform/platform-logs-overview.md)kullanma.
