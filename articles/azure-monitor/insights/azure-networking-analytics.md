---
title: Azure Monitör'de Azure Ağ Analizi çözümü | Microsoft Dokümanlar
description: Azure ağ güvenliği grup günlüklerini ve Azure Uygulama Ağ Geçidi günlüklerini incelemek için Azure Monitor'daki Azure Ağ Analizi çözümünü kullanabilirsiniz.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 06/21/2018
ms.openlocfilehash: 1045f86db5e1a9ed1979a266937974045e401e27
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79275574"
---
# <a name="azure-networking-monitoring-solutions-in-azure-monitor"></a>Azure Monitor'da Azure ağ izleme çözümleri

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Azure Monitor, ağlarınızı izlemek için aşağıdaki çözümleri sunar:
* Ağ Performans Monitörü (NPM) için
    * Ağınızın durumunu izleme
* İncelenmek üzere Azure Uygulama Ağ Geçidi analitiği
    * Azure Uygulama Ağ Geçidi günlükleri
    * Azure Uygulama Ağ Geçidi ölçümleri
* Bulut ağınızdaki ağ etkinliğini izlemek ve denetlemek için çözümler
    * [Trafik Analitiği](https://docs.microsoft.com/azure/networking/network-monitoring-overview#traffic-analytics) 
    * Azure Ağ Güvenliği Grubu Analizi

## <a name="network-performance-monitor-npm"></a>Ağ Performans Monitörü (NPM)

[Ağ Performans Monitörü](https://docs.microsoft.com/azure/networking/network-monitoring-overview) yönetimi çözümü, ağların durumunu, kullanılabilirliğini ve ulaşılabilirliğini izleyen bir ağ izleme çözümüdür.  Arasındaki bağlantıyı izlemek için kullanılır:

* Genel bulut ve şirket içi
* Veri merkezleri ve kullanıcı konumları (şubeler)
* Çok katmanlı bir uygulamanın çeşitli katmanlarına ev sahipliği yapan alt ağlar.

Daha fazla bilgi için [Bkz. Ağ Performans İzleyicisi.](https://docs.microsoft.com/azure/networking/network-monitoring-overview)

## <a name="azure-application-gateway-and-network-security-group-analytics"></a>Azure Uygulama Ağ Geçidi ve Ağ Güvenliği Grubu analitiği
Çözümleri kullanmak için:
1. Azure Monitor'a yönetim çözümünü ekleyin ve
2. Tanılamanın Azure Monitor'daki Bir Günlük Analizi çalışma alanına yönlendirilmesini etkinleştirin. Günlükleri Azure Blob depolama alanına yazmak gerekli değildir.

Uygulama Ağ Geçidi ve Ağ Güvenlik Gruplarından biri veya her ikisi için tanılama ve ilgili çözümü etkinleştirebilirsiniz.

Belirli bir kaynak türü için tanılama kaynağı günlüğe kaydetmeyi etkinleştirmez, ancak çözümü yüklerseniz, bu kaynağın pano bıçakları boştur ve bir hata iletisi görüntüler.

> [!NOTE]
> Ocak 2017'de, Uygulama Ağ Geçitleri ve Ağ Güvenliği Gruplarından Log Analytics çalışma alanına günlük göndermenin desteklenen yolu değişti. Azure Ağ **Analizi (amortismana ermiş)** çözümgörürseniz, izlemeniz gereken adımlar için [eski Ağ Analizi çözümünden geçiş](#migrating-from-the-old-networking-analytics-solution) ebakın.
>
>

## <a name="review-azure-networking-data-collection-details"></a>Azure ağ veri toplama ayrıntılarını gözden geçirin
Azure Uygulama Ağ Geçidi analitiği ve Ağ Güvenliği Grubu analiz yönetimi çözümleri, tanılama günlüklerini doğrudan Azure Uygulama Ağ Geçitleri ve Ağ Güvenlik Gruplarından toplar. Günlükleri Azure Blob depolama alanına yazmak gerekli değildir ve veri toplama için aracı gerekmez.

Aşağıdaki tabloda veri toplama yöntemleri ve Azure Application Gateway analitiği ve Ağ Güvenliği Grubu analitiği için verilerin nasıl toplandığıyla ilgili diğer ayrıntılar gösterilmektedir.

| Platform | Doğrudan aracı | Sistem Merkezi Operasyon Yöneticisi | Azure | Operasyon Yöneticisi gerekli mi? | Yönetim grubu aracılığıyla gönderilen Operasyon Yöneticisi aracı verileri | Toplama sıklığı |
| --- | --- | --- | --- | --- | --- | --- |
| Azure |  |  |&#8226; |  |  |günlüğe kaydedildiğinde |


## <a name="azure-application-gateway-analytics-solution-in-azure-monitor"></a>Azure Monitor'da Azure Uygulama Ağ Geçidi analizi çözümü

![Azure Uygulama Ağ Geçidi Analizi simgesi](media/azure-networking-analytics/azure-analytics-symbol.png)

Uygulama Ağ Geçitleri için aşağıdaki günlükler desteklenir:

* UygulamaGatewayAccessLog
* UygulamaGatewayPerformanceLog
* UygulamaGatewayFirewallLog

Uygulama Ağ Geçitleri için aşağıdaki ölçümler desteklenir:tekrar


* 5 dakikalık iş için

### <a name="install-and-configure-the-solution"></a>Çözümü yükleme ve yapılandırma
Azure Uygulama Ağ Geçidi analizi çözümlerini yüklemek ve yapılandırmak için aşağıdaki yönergeleri kullanın:

1. Azure Uygulama Ağ Geçidi analizi çözümünü [Azure pazaralanından](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.AzureAppGatewayAnalyticsOMS?tab=Overview) veya [Çözüm Galerisi'nden Azure Monitörü Ekle çözümlerinde](../../azure-monitor/insights/solutions.md)açıklanan işlemi kullanarak etkinleştirin.
2. İzlemek istediğiniz [Uygulama Ağ Geçitleri](../../application-gateway/application-gateway-diagnostics.md) için tanılama günlüğe kaydetmeyi etkinleştirin.

#### <a name="enable-azure-application-gateway-diagnostics-in-the-portal"></a>Portalda Azure Uygulama Ağ Geçidi tanılamasını etkinleştirme

1. Azure portalında, izlemek için Uygulama Ağ Geçidi kaynağına gidin.
2. Aşağıdaki sayfayı açmak için *Tanılama günlüklerini* seçin.

   ![Azure Uygulama Ağ Geçidi kaynağının görüntüsü](media/azure-networking-analytics/log-analytics-appgateway-enable-diagnostics01.png)
3. Aşağıdaki sayfayı açmak için *tanılamayı Aç'ı* tıklatın.

   ![Azure Uygulama Ağ Geçidi kaynağının görüntüsü](media/azure-networking-analytics/log-analytics-appgateway-enable-diagnostics02.png)
4. Tanılamayı açmak için Durum altında *Aç'ı* tıklatın. *Status*
5. *Günlük Analitiğine Gönder*için onay kutusunu tıklatın.
6. Varolan bir Günlük Analizi çalışma alanını seçin veya bir çalışma alanı oluşturun.
7. Toplanacak günlük türlerinin her biri için **Günlük** altındaki onay kutusunu tıklatın.
8. Tanılamanın Azure Monitor'da günlüğe kaydedilmesini etkinleştirmek için *Kaydet'i* tıklatın.

#### <a name="enable-azure-network-diagnostics-using-powershell"></a>PowerShell'i kullanarak Azure ağ tanılamasını etkinleştirme

Aşağıdaki PowerShell komut dosyası, uygulama ağ geçitleri için kaynak günlüğe kaydetmeyi etkinleştirmeye yönelik bir örnek sağlar.

```powershell
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$gateway = Get-AzApplicationGateway -Name 'ContosoGateway'

Set-AzDiagnosticSetting -ResourceId $gateway.ResourceId  -WorkspaceId $workspaceId -Enabled $true
```

### <a name="use-azure-application-gateway-analytics"></a>Azure Uygulama Ağ Geçidi analizini kullanma
![Azure Uygulama Ağ Geçidi analizi döşemesi görüntüsü](media/azure-networking-analytics/log-analytics-appgateway-tile.png)

Genel Bakış'ta **Azure Uygulama Ağ Geçidi analizi** nitesini tıklattıktan sonra, günlüklerinizin özetlerini görüntüleyebilir ve ardından aşağıdaki kategorilerin ayrıntılarına bakabilirsiniz:

* Uygulama Ağ Geçidi Erişim günlükleri
  * Uygulama Ağ Geçidi erişim günlükleri için istemci ve sunucu hataları
  * Her Uygulama Ağ Geçidi için saat başına istekler
  * Her Uygulama Ağ Geçidi için saat başına başarısız istekler
  * Uygulama Ağ Geçitleri için kullanıcı aracısı tarafından hatalar
* Uygulama Ağ Geçidi performansı
  * Uygulama Ağ Geçidi için ana bilgisayar durumu
  * Uygulama Ağ Geçidi için maksimum ve 95 yüzdelik başarısız istekleri

![Azure Uygulama Ağ Geçidi analiz panosunun görüntüsü](media/azure-networking-analytics/log-analytics-appgateway01.png)

![Azure Uygulama Ağ Geçidi analiz panosunun görüntüsü](media/azure-networking-analytics/log-analytics-appgateway02.png)

Azure **Uygulama Ağ Geçidi analiz** panosunda, bıçaklardan birinde özet bilgileri gözden geçirin ve ardından günlük arama sayfasındaki ayrıntılı bilgileri görüntülemek için bir tanesini tıklatın.

Günlük arama sayfalarından herhangi birinde, sonuçları zamana, ayrıntılı sonuçlara ve günlük arama geçmişinize göre görüntüleyebilirsiniz. Sonuçları daraltmak için de yüzlere göre filtre uygulayabilirsiniz.


## <a name="azure-network-security-group-analytics-solution-in-azure-monitor"></a>Azure Monitor'da Azure Ağ Güvenliği Grubu analiz çözümü

![Azure Ağ Güvenliği Grubu Analytics simgesi](media/azure-networking-analytics/azure-analytics-symbol.png)

> [!NOTE]
> Ağ Güvenliği Grubu analiz çözümü, işlevselliği [Trafik Analitiği](../../network-watcher/traffic-analytics.md)ile değiştirildiğinden topluluk desteğine geçiyor.
> - Çözüm artık Azure [Quickstart Şablonları'nda](https://azure.microsoft.com/resources/templates/oms-azurensg-solution/) kullanılabilir ve yakında Azure Marketi'nde kullanılamaz.
> - Çözümü çalışma alanına zaten ekleyen varolan müşteriler için, hiçbir değişiklik olmadan çalışmaya devam eder.
> - Microsoft, Tanılama Ayarları'nı kullanarak Çalışma alanınıza NSG kaynak günlüklerinin gönderilmesini desteklemeye devam edecektir.

Aşağıdaki günlükler ağ güvenlik grupları için desteklenir:

* AğGüvenliğiGrubuEtkinlik
* AğSecurityGroupRuleCounter

### <a name="install-and-configure-the-solution"></a>Çözümü yükleme ve yapılandırma
Azure Ağ Analizi çözümlerini yüklemek ve yapılandırmak için aşağıdaki yönergeleri kullanın:

1. Azure Ağ Güvenliği Grubu analiz çözümünü [Azure pazaralanından](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.AzureNSGAnalyticsOMS?tab=Overview) veya [Çözüm Galerisi'nden Azure Monitörü Ekle çözümlerinde](../../azure-monitor/insights/solutions.md)açıklanan işlemi kullanarak etkinleştirin.
2. İzlemek istediğiniz [Ağ Güvenlik Grubu](../../virtual-network/virtual-network-nsg-manage-log.md) kaynakları için tanılama günlüğe kaydetmeyi etkinleştirin.

### <a name="enable-azure-network-security-group-diagnostics-in-the-portal"></a>Portalda Azure ağ güvenlik grubu tanılamasını etkinleştirme

1. Azure portalında, izlemek için Ağ Güvenliği Grubu kaynağına gidin
2. Aşağıdaki sayfayı açmak için *Tanılama günlüklerini* seçin

   ![Azure Ağ Güvenlik Grubu kaynağının görüntüsü](media/azure-networking-analytics/log-analytics-nsg-enable-diagnostics01.png)
3. Aşağıdaki sayfayı açmak için *tanılamayı Aç'ı* tıklatın

   ![Azure Ağ Güvenlik Grubu kaynağının görüntüsü](media/azure-networking-analytics/log-analytics-nsg-enable-diagnostics02.png)
4. Tanılamayı açmak için Durum altında *Aç'ı* tıklatın *Status*
5. *Günlük Analitiğine Gönder* için onay kutusunu tıklatın
6. Varolan bir Log Analytics çalışma alanı seçin veya bir çalışma alanı oluşturun
7. Toplamak için günlük türlerinin her biri için **Günlük** altındaki onay kutusunu tıklatın
8. Günlük Analizi'nde tanılamanın günlüğe kaydedilmesini etkinleştirmek için *Kaydet'i* tıklatın

### <a name="enable-azure-network-diagnostics-using-powershell"></a>PowerShell'i kullanarak Azure ağ tanılamasını etkinleştirme

Aşağıdaki PowerShell komut dosyası, ağ güvenlik grupları için kaynak günlüğe kaydetmeyi etkinleştirme nize bir örnek sağlar
```powershell
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$nsg = Get-AzNetworkSecurityGroup -Name 'ContosoNSG'

Set-AzDiagnosticSetting -ResourceId $nsg.ResourceId  -WorkspaceId $workspaceId -Enabled $true
```

### <a name="use-azure-network-security-group-analytics"></a>Azure Ağ Güvenliği Grubu analizini kullanma
Genel Bakış'ta **Azure Ağ Güvenlik Grubu analiz** döşemesini tıklattıktan sonra, günlüklerinizin özetlerini görüntüleyebilir ve ardından aşağıdaki kategorilerin ayrıntılarını inceleyebilirsiniz:

* Ağ güvenlik grubu akışları engelledi
  * Engellenen akışları olan ağ güvenliği grubu kuralları
  * Engellenen akışlara sahip MAC adresleri
* Ağ güvenlik grubu akışlarıizin
  * İzin verilen akışları içeren ağ güvenliği grubu kuralları
  * İzin verilen akışlara sahip MAC adresleri

![Azure Ağ Güvenliği Grubu analiz panosunun görüntüsü](media/azure-networking-analytics/log-analytics-nsg01.png)

![Azure Ağ Güvenliği Grubu analiz panosunun görüntüsü](media/azure-networking-analytics/log-analytics-nsg02.png)

Azure **Ağ Güvenlik Grubu analiz** panosunda, bıçaklardan birinde özet bilgileri gözden geçirin ve ardından günlük arama sayfasındaki ayrıntılı bilgileri görüntülemek için bir tanesini tıklatın.

Günlük arama sayfalarından herhangi birinde, sonuçları zamana, ayrıntılı sonuçlara ve günlük arama geçmişinize göre görüntüleyebilirsiniz. Sonuçları daraltmak için de yüzlere göre filtre uygulayabilirsiniz.

## <a name="migrating-from-the-old-networking-analytics-solution"></a>Eski Networking Analytics çözümünden geçiş
Ocak 2017'de, Azure Uygulama Ağ Geçitleri ve Azure Ağ Güvenlik Gruplarından Günlük Analizi çalışma alanına günlük göndermenin desteklenen yolu değişti. Bu değişiklikler aşağıdaki avantajları sağlar:
+ Günlükler, depolama hesabı kullanmaya gerek kalmadan doğrudan Azure Monitor'a yazılır
+ Günlüklerin oluşturulduğu andan itibaren Azure Monitör'de kullanılabilir hale gelen daha az gecikme süresi
+ Daha az yapılandırma adımı
+ Tüm Azure tanılama türleri için ortak bir biçim

Güncelleştirilmiş çözümleri kullanmak için:

1. [Azure Uygulama Ağ Geçitlerinden doğrudan Azure Monitor'a gönderilecek tanılamaları yapılandırma](#enable-azure-application-gateway-diagnostics-in-the-portal)
2. [Azure Ağ Güvenlik Gruplarından doğrudan Azure Monitor'a gönderilecek tanılamaları yapılandırma](#enable-azure-network-security-group-diagnostics-in-the-portal)
2. [Çözümler Galerisi'nden Azure Monitörü Ekle çözümlerinde](solutions.md) açıklanan işlemi kullanarak *Azure Uygulama Ağ Geçidi Analizi* ve Azure Ağ Güvenliği Grubu *Analizi* çözümünü etkinleştirin
3. Kaydedilen sorguları, panoları veya uyarıları yeni veri türünü kullanmak için güncelleştirme
   + Türü AzureDiagnostics içindir. Azure ağ günlüklerine filtre açmak için Kaynak Türü'ni kullanabilirsiniz.

     | Onun yerine: | Kullanın: |
     | --- | --- |
     | NetworkApplicationağ geçitleri &#124; nerede OperationName=="ApplicationGatewayAccess" | AzureDiagnostics &#124; nerede KaynakType=="APPLICATIONGATEWAYS" ve OperationName=="ApplicationGatewayAccess" |
     | NetworkApplicationağ ağ geçitleri &#124; nerede OperationName=="ApplicationGatewayPerformance" | AzureDiagnostics &#124; nerede KaynakType=="APPLICATIONGATEWAYS" ve OperationName=="ApplicationGatewayPerformance" |
     | AğGüvenlik Grupları | AzureDiagnostics &#124; nerede Kaynak Type=="NETWORKSECURITYGROUPS" |

   + Addaki \_s, \_d veya \_g eki olan herhangi bir alan için, ilk karakteri küçük harfle değiştirin
   + Adı o soneki \_olan herhangi bir alan için, veriler iç içe geçen alan adlarını temel alan olarak tek tek alanlara ayrılır.
4. Azure *Ağ Analizi (Deprecated)* çözümlerini kaldırın.
   + PowerShell kullanıyorsanız,`Set-AzureOperationalInsightsIntelligencePack -ResourceGroupName <resource group that the workspace is in> -WorkspaceName <name of the log analytics workspace> -IntelligencePackName "AzureNetwork" -Enabled $false`

Değişiklikten önce toplanan veriler yeni çözümde görünmez. Eski Tür ve alan adlarını kullanarak bu verileri sorgulamaya devam edebilirsiniz.

## <a name="troubleshooting"></a>Sorun giderme
[!INCLUDE [log-analytics-troubleshoot-azure-diagnostics](../../../includes/log-analytics-troubleshoot-azure-diagnostics.md)]

## <a name="next-steps"></a>Sonraki adımlar
* Ayrıntılı Azure tanılama verilerini görüntülemek için [Azure Monitor'da Günlük sorgularını](../log-query/log-query-overview.md) kullanın.
