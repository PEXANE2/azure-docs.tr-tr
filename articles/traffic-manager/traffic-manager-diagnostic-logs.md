---
title: Azure Trafik Yöneticisi'nde tanılama günlüğe kaydetmeyi etkinleştirme
description: Trafik Yöneticisi profiliniz için tanılama günlüğünü nasıl etkinleştirip sonuç olarak oluşturulan günlük dosyalarına nasıl eriştüğünüzi öğrenin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76938667"
---
# <a name="enable-diagnostic-logging-in-azure-traffic-manager"></a>Azure Trafik Yöneticisi'nde tanılama günlüğe kaydetmeyi etkinleştirme

Bu makalede, trafik yöneticisi profili için tanılama günlüğe kaydetme ve günlük verilerine nasıl erişilenler açıklanmaktadır.

Azure Trafik Yöneticisi tanı günlükleri, Trafik Yöneticisi profil kaynağının davranışı hakkında bilgi sağlayabilir. Örneğin, tek tek sondaların neden bir bitiş noktasına karşı zaman alabildiğinizi belirlemek için profilin günlük verilerini kullanabilirsiniz.

## <a name="enable-diagnostic-logging"></a>Tanılama günlüğünü etkinleştirme

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[Azure Bulut Kabuğu'nda](https://shell.azure.com/powershell)veya bilgisayarınızdan PowerShell çalıştırarak takip eden komutları çalıştırabilirsiniz. Azure Bulut Kabuğu ücretsiz bir etkileşimli kabuktır. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. PowerShell'i bilgisayarınızdan çalıştırAcaksanız, 1.0.0 veya sonrası Azure PowerShell modülüne ihtiyacınız vardır. Yüklü sürümü `Get-Module -ListAvailable Az` bulmak için çalıştırabilirsiniz. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız, `Login-AzAccount` Azure'da oturum açabilmek için de çalışmanız gerekir.

1. **Trafik Yöneticisi profilini alın:**

    Tanılama günlüğe kaydetmeyi etkinleştirmek için trafik yöneticisi profilinin kimliğine ihtiyacınız var. [Get-AzTrafficManagerProfile](/powershell/module/az.TrafficManager/Get-azTrafficManagerProfile)ile tanılama günlüğe kaydetmeyi etkinleştirmek istediğiniz Trafik Yöneticisi profilini alın. Çıktı, Trafik Yöneticisi profilinin kimlik bilgilerini içerir.

    ```azurepowershell-interactive
    Get-AzTrafficManagerProfile -Name <TrafficManagerprofilename> -ResourceGroupName <resourcegroupname>
    ```

2. **Trafik Yöneticisi profili için tanılama günlüğe kaydetmeyi etkinleştirin:**

    [Set-AzDiagnosticSetting](https://docs.microsoft.com/powershell/module/az.monitor/set-azdiagnosticsetting?view=latest)ile önceki adımda elde edilen kimliği kullanarak Trafik Yöneticisi profili için tanılama günlüğe kaydetmeyi etkinleştirin. Aşağıdaki komut, Trafik Yöneticisi profilinin ayrıntılı günlüklerini belirli bir Azure Depolama hesabına saklar. 

      ```azurepowershell-interactive
    Set-AzDiagnosticSetting -ResourceId <TrafficManagerprofileResourceId> -StorageAccountId <storageAccountId> -Enabled $true
      ``` 
3. **Tanılama ayarlarını doğrulayın:**

      [Get-AzDiagnosticSetting'i](https://docs.microsoft.com/powershell/module/az.monitor/get-azdiagnosticsetting?view=latest)kullanarak Trafik Yöneticisi profilinin tanı ayarlarını doğrulayın. Aşağıdaki komut, bir kaynak için günlüğe kaydedilen kategorileri görüntüler.

     ```azurepowershell-interactive
     Get-AzDiagnosticSetting -ResourceId <TrafficManagerprofileResourceId>
     ```  
      Trafik Yöneticisi profil kaynak ile ilişkili tüm günlük kategorilerini etkin olarak gösterdiğinden emin olun. Ayrıca, depolama hesabının doğru şekilde ayarlandığını doğrulayın.

## <a name="access-log-files"></a>Günlük dosyalarına eriş
1. [Azure portalında](https://portal.azure.com)oturum açın. 
1. Portaldaki Azure Depolama hesabınıza gidin.
2. Azure depolama hesabınızın **Genel Bakış** sayfasında, **Hizmetler** altında **Blobs'u**seçin.
3. **Kapsayıcılar** **için, insights-logs-probehealthstatusevents'i**seçin ve PT1H.json dosyasına gidin ve bu günlük dosyasının bir kopyasını indirmek ve kaydetmek için **İndir'i** tıklatın.

    ![Trafik Yöneticisi profilinizin günlük dosyalarına blob depolamadan erişin](./media/traffic-manager-logs/traffic-manager-logs.png)


## <a name="traffic-manager-log-schema"></a>Trafik Yöneticisi günlük şeması

Azure Monitor aracılığıyla kullanılabilen tüm tanılama günlükleri, her hizmetin kendi etkinlikleri için benzersiz özellikler yaçması için esneklik le birlikte ortak bir üst düzey şemayı paylaşır. Üst düzey tanılama günlükleri şeması [için, Azure Tanı Günlükleri için Desteklenen hizmetler, şemalar ve kategorilere](../azure-monitor/platform/tutorial-dashboards.md)bakın.

Aşağıdaki tablo, Azure Trafik Yöneticisi profil kaynağına özgü günlük şemasını içerir.

|||||
|----|----|---|---|
|**Alan Adı**|**Alan Türü**|**Tanım**|**Örnek**|
|EndpointName|Dize|Sağlık durumu kaydedilen Trafik Yöneticisi bitiş noktasının adı.|*myPrimaryEndpoint*|
|Durum|Dize|Incelenen Trafik Yöneticisi bitiş noktasının sağlık durumu. Durum **Yukarı** veya **Aşağı**olabilir.|**Yukarı**|
|||||

## <a name="next-steps"></a>Sonraki adımlar

* [Trafik Yöneticisi İzleme](traffic-manager-monitoring.md) hakkında daha fazla bilgi edinin

