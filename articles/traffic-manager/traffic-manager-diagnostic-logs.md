---
title: Azure Traffic Manager tanılama günlüğünü etkinleştirme
description: Traffic Manager profiliniz için tanılama günlüğünü etkinleştirme ve sonuç olarak oluşturulan günlük dosyalarına erişme hakkında bilgi edinin.
services: traffic-manager
author: rohinkoul
manager: twooley
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/25/2019
ms.author: rohink
ms.openlocfilehash: 0ed2ecef86795f62aa3fe5798dcd0d07adbaf9cc
ms.sourcegitcommit: fa6fe765e08aa2e015f2f8dbc2445664d63cc591
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2020
ms.locfileid: "76938667"
---
# <a name="enable-diagnostic-logging-in-azure-traffic-manager"></a>Azure Traffic Manager tanılama günlüğünü etkinleştirme

Bu makalede, bir Traffic Manager profili için tanılama günlüğü ve erişim günlüğü verilerine nasıl etkinleştirileceği açıklanır.

Azure Traffic Manager tanılama günlükleri, Traffic Manager profili kaynağının davranışına ilişkin öngörüler sağlayabilir. Örneğin, tek tek yoklamaların bir uç noktada zaman aşımına uğradığını anlamak için profilin günlük verilerini kullanabilirsiniz.

## <a name="enable-diagnostic-logging"></a>Tanılama günlüğünü etkinleştirme

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[Azure Cloud Shell](https://shell.azure.com/powershell)izleyen komutları veya bilgisayarınızdan PowerShell 'i çalıştırarak çalıştırabilirsiniz. Azure Cloud Shell, ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. PowerShell 'i bilgisayarınızdan çalıştırırsanız, Azure PowerShell Module, 1.0.0 veya sonraki bir sürümü gerekir. Yüklü sürümü bulmak için `Get-Module -ListAvailable Az` çalıştırabilirsiniz. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/install-az-ps). PowerShell 'i yerel olarak çalıştırıyorsanız Azure 'da oturum açmak için de `Login-AzAccount` çalıştırmanız gerekir.

1. **Traffic Manager profilini alın:**

    Tanılama günlük kaydını etkinleştirmek için bir Traffic Manager profili KIMLIĞI gerekir. [Get-AzTrafficManagerProfile](/powershell/module/az.TrafficManager/Get-azTrafficManagerProfile)ile için tanılama günlüğünü etkinleştirmek istediğiniz Traffic Manager profilini alın. Çıktı, Traffic Manager profilinin KIMLIK bilgilerini içerir.

    ```azurepowershell-interactive
    Get-AzTrafficManagerProfile -Name <TrafficManagerprofilename> -ResourceGroupName <resourcegroupname>
    ```

2. **Traffic Manager profili için tanılama günlüğünü etkinleştirin:**

    Önceki adımda [set-AzDiagnosticSetting](https://docs.microsoft.com/powershell/module/az.monitor/set-azdiagnosticsetting?view=latest)ile elde edilen kimliği kullanarak Traffic Manager profili için tanılama günlüğünü etkinleştirin. Aşağıdaki komut Traffic Manager profili için ayrıntılı günlükleri belirtilen Azure depolama hesabına depolar. 

      ```azurepowershell-interactive
    Set-AzDiagnosticSetting -ResourceId <TrafficManagerprofileResourceId> -StorageAccountId <storageAccountId> -Enabled $true
      ``` 
3. **Tanılama ayarlarını doğrulayın:**

      [Get-AzDiagnosticSetting](https://docs.microsoft.com/powershell/module/az.monitor/get-azdiagnosticsetting?view=latest)kullanarak Traffic Manager profili için tanılama ayarlarını doğrulayın. Aşağıdaki komut, bir kaynak için günlüğe kaydedilen kategorileri görüntüler.

     ```azurepowershell-interactive
     Get-AzDiagnosticSetting -ResourceId <TrafficManagerprofileResourceId>
     ```  
      Traffic Manager profili kaynağıyla ilişkili tüm günlük kategorilerinin etkinleştirildiğinden emin olun. Ayrıca, depolama hesabının doğru şekilde ayarlandığını doğrulayın.

## <a name="access-log-files"></a>Günlük dosyalarına erişin
1. [Azure Portal](https://portal.azure.com)’ında oturum açın. 
1. Portalda Azure depolama hesabınıza gidin.
2. Azure depolama hesabınızın **genel bakış** sayfasında, **Hizmetler** ' in altında **Bloblar**' ı seçin.
3. **Kapsayıcılar**için, **Öngörüler-logs-probehealthkara sevents**' i seçin ve PT1H. JSON dosyasına gidin ve bu günlük dosyasının bir kopyasını indirip kaydetmek için **İndir** ' e tıklayın.

    ![Traffic Manager profilinizin günlük dosyalarına blob depolamadan erişin](./media/traffic-manager-logs/traffic-manager-logs.png)


## <a name="traffic-manager-log-schema"></a>Traffic Manager log şeması

Azure Izleyici aracılığıyla kullanılabilen tüm tanılama günlükleri, her bir hizmetin kendi olayları için benzersiz özellikler yaymasını sağlayan ortak bir üst düzey şemayı paylaşır. Üst düzey tanılama günlükleri şeması için bkz. [Azure tanılama günlükleri Için desteklenen hizmetler, şemalar ve Kategoriler](../azure-monitor/platform/tutorial-dashboards.md).

Aşağıdaki tabloda, Azure Traffic Manager profili kaynağına özgü Günlükler şeması yer almaktadır.

|||||
|----|----|---|---|
|**Alan Adı**|**Alan türü**|**Tanım**|**Örnek**|
|EndpointName|Dize|Sistem durumu kaydedilirken Traffic Manager uç noktasının adı.|*myPrimaryEndpoint*|
|Durum|Dize|Araştırılan Traffic Manager uç noktasının sistem durumu. Durum **yukarı** veya **aşağı**olabilir.|**Ayarlama**|
|||||

## <a name="next-steps"></a>Sonraki adımlar

* [Traffic Manager izleme](traffic-manager-monitoring.md) hakkında daha fazla bilgi edinin

