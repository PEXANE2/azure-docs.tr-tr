---
title: Azure Otomasyon Durumu Yapılandırması raporlama verilerini Azure Monitor günlüklerine iletme
description: Bu makalede, ek öngörü ve yönetim sunmak için Azure Otomasyon Durum Yapılandırmasından Azure Monitor günlüklerine veri raporlaması istenen durum yapılandırması (DSC) nasıl gönderilen gösterilmektedir.
services: automation
ms.service: automation
ms.subservice: dsc
author: mgoedtel
ms.author: magoedte
ms.date: 11/06/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: a75b71d43b072d366ef2fcb15bf4c901680d48fb
ms.sourcegitcommit: ea006cd8e62888271b2601d5ed4ec78fb40e8427
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81383226"
---
# <a name="forward-azure-automation-state-configuration-reporting-data-to-azure-monitor-logs"></a>Azure Otomasyon Durumu Yapılandırması raporlama verilerini Azure Monitor günlüklerine iletme

Azure Otomasyon Durumu Yapılandırması düğüm durumu verilerini 30 gün boyunca saklar. Bu verileri daha uzun süre saklamayı tercih ederseniz, düğüm durumu verilerini Log Analytics çalışma alanınıza gönderebilirsiniz. Uyumluluk durumu Azure portalında veya PowerShell'de, düğümler için ve düğüm yapılandırmalarında tek tek DSC kaynakları için görünür. 

Azure Monitor günlükleri, Otomasyon Durumu Yapılandırma verileriniz için daha fazla operasyonel görünürlük sağlar ve olayları daha hızlı ele almanıza yardımcı olabilir. Azure Monitor günlükleri ile şunları yapabilirsiniz:

- Yönetilen düğümler ve tek tek kaynaklar için uyumluluk bilgileri alın.
- Uyumluluk durumuna göre bir e-posta veya uyarı tetikleme.
- Yönetilen düğümlerinize gelişmiş sorgular yazın.
- Otomasyon hesapları arasında uyumluluk durumunu ilişkilendirin.
- Runbook sonuçlarınızı, runbook iş durumunuzu ve diğer ilgili önemli göstergeleri veya ölçümleri görselleştirmek için özel görünümler ve arama sorguları kullanın.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma Runbook Worker'ınızdaki Az modül yükleme yönergeleri için Azure [PowerShell Modül'üne](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)bakın. Otomasyon hesabınız için, Azure Otomasyonu'nda Azure [PowerShell modüllerini nasıl güncelleştirebileceğinizi](automation-update-azure-modules.md)kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Otomasyon Durumu Yapılandırma raporlarınızı Azure Monitor günlüklerine göndermeye başlamak için şunları yapmanız gerekir:

- Kasım 2016 veya daha sonra [Azure PowerShell](/powershell/azure/overview) sürümü (v2.3.0).
- Azure Otomasyonu hesabı. Daha fazla bilgi için Azure [Otomasyonuna giriş](automation-intro.md)için bkz.
- Otomasyon & Kontrol hizmeti sunan Bir Log Analytics çalışma alanı. Daha fazla bilgi için Azure [Monitor'da Günlük Analizi'ne başlayın' a](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal)bakın.
- En az bir Azure Otomasyon Durumu Yapılandırma düğümü. Daha fazla bilgi için Azure [Otomasyon Durumu Yapılandırması'na göre yönetim için Onboarding makinelerine](automation-dsc-onboarding.md)bakın.
- [xDscDiagnostics](https://www.powershellgallery.com/packages/xDscDiagnostics/2.7.0.0) modülü, sürüm 2.7.0.0 veya daha büyük. Yükleme adımları için bkz: [Sorun Giderme Azure Otomasyonu İstenen Durum Yapılandırması.](./troubleshoot/desired-state-configuration.md)

## <a name="set-up-integration-with-azure-monitor-logs"></a>Azure Monitor günlükleriyle tümleştirme ayarlama

Azure Otomasyon Durumu Yapılandırmasından Azure Monitor günlüklerine veri almaya başlamak için aşağıdaki adımları tamamlayın:

1. PowerShell'de Azure hesabınızda oturum açın. [Azure PowerShell ile Oturum Aç'ı](https://docs.microsoft.com/powershell/azure/authenticate-azureps)görün.
1. Aşağıdaki PowerShell cmdlet'i çalıştırarak Otomasyon hesabınızın kaynak kimliğini alın. Birden fazla otomasyon hesabınız varsa, yapılandırmak istediğiniz hesabın kaynak kimliğini seçin.

   ```powershell
   # Find the ResourceId for the Automation account
   Get-AzResource -ResourceType 'Microsoft.Automation/automationAccounts'
   ```

1. Aşağıdaki PowerShell cmdlet'i çalıştırarak Log Analytics çalışma alanınızın kaynak kimliğini alın. Birden fazla çalışma alanınız varsa, yapılandırmak istediğiniz çalışma alanı için kaynak kimliğini seçin.

   ```powershell
   # Find the ResourceId for the Log Analytics workspace
   Get-AzResource -ResourceType 'Microsoft.OperationalInsights/workspaces'
   ```

1. Aşağıdaki PowerShell cmdlet'i `<AutomationResourceId>` çalıştırın, `ResourceId` önceki adımların her birinin değerlerini değiştirin ve `<WorkspaceResourceId>` değiştirin.

   ```powershell
   Set-AzDiagnosticSetting -ResourceId <AutomationResourceId> -WorkspaceId <WorkspaceResourceId> -Enabled $true -Category 'DscNodeStatus'
   ```

1. Azure Otomasyon Durumu Yapılandırmasından Azure Monitor günlüklerine veri almamayı durdurmak istiyorsanız, aşağıdaki PowerShell cmdlet'i çalıştırın.

   ```powershell
   Set-AzDiagnosticSetting -ResourceId <AutomationResourceId> -WorkspaceId <WorkspaceResourceId> -Enabled $false -Category 'DscNodeStatus'
   ```

## <a name="view-the-state-configuration-logs"></a>Durum Yapılandırma günlüklerini görüntüleme

Otomasyon Durumu Yapılandırma verileriniz için Azure Monitor günlükleriyle tümleştirme ayarladıktan sonra, Durum yapılandırması (DSC) sayfasının sol bölmesindeki **İzleme** bölümündeki **Günlükleri** seçerek bunları görüntüleyebilirsiniz.

![Günlükler](media/automation-dsc-diagnostics/automation-dsc-logs-toc-item.png)

Günlük Arama bölmesi, Otomasyon hesap kaynağınıza kapsamlı bir sorgu bölgesiyle açılır. Azure Monitor günlüklerinde arama yaparak DSC işlemleri için Durum Yapılandırma günlüklerinde arama yapabilirsiniz. DSC işlemlerinin kayıtları `AzureDiagnostics` tabloda depolanır. Örneğin, uyumlu olmayan düğümleri bulmak için aşağıdaki sorguyu yazın.

```AzureDiagnostics
| where Category == 'DscNodeStatus' 
| where OperationName contains 'DSCNodeStatusData'
| where ResultType != 'Compliant'
```
Filtreleme ayrıntıları:

* Her `DscNodeStatusData` Durum Yapılandırma düğümü için işlemleri döndürmek için filtre uygulayın.
* Bu `DscResourceStatusData` kaynağa uygulanan düğüm yapılandırmasında çağrılan her DSC kaynağıiçin işlemleri döndürmek için filtre uygulayın. 
* Başarısız `DscResourceStatusData` olan tüm DSC kaynakları için hata bilgilerini döndürmek için filtre uygulayın.

Verileri bulmak için günlük sorguları oluşturma hakkında daha fazla bilgi edinmek için [Azure Monitor'daki günlük sorgularına genel bakış](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview)bölümüne bakın.

### <a name="send-an-email-when-a-state-configuration-compliance-check-fails"></a>Durum Yapılandırması uyumluluk denetimi başarısız olduğunda e-posta gönderme

En iyi müşteri isteklerimizden biri, DSC yapılandırmasıyla ilgili bir sorun olduğunda e-posta veya metin gönderebilme yeteneğidir.

Bir uyarı kuralı oluşturmak için, uyarıyı çağırması gereken Durum Yapılandırması rapor kayıtları için günlük araması oluşturarak başlayın. Uyarı **kuralını** oluşturmak ve yapılandırmak için Yeni Uyarı Kuralı düğmesini tıklatın.

1. Günlük Analizi çalışma alanına Genel Bakış sayfasından **Günlükler'i**tıklatın.
1. Sorgu alanına aşağıdaki aramayı yazarak uyarınız için bir günlük arama sorgusu oluşturun:`Type=AzureDiagnostics Category='DscNodeStatus' NodeName_s='DSCTEST1' OperationName='DscNodeStatusData' ResultType='Failed'`

   Birden fazla Otomasyon hesabından veya çalışma alanınıza abonelikten günlükler ayarladıysanız, uyarılarınızı abonelik ve Otomasyon hesabına göre gruplayabilirsiniz. **DscNodeStatusData** `Resource` kayıtlarında aramada Otomasyon hesap adını alandan türetin.
1. **Oluştur kuralı** ekranını açmak için sayfanın üst kısmında Yeni **Uyarı Kuralı'nı** tıklatın. 

Uyarıyı yapılandırma seçenekleri hakkında daha fazla bilgi için [bkz.](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md)

### <a name="find-failed-dsc-resources-across-all-nodes"></a>Tüm düğümlerde başarısız DSC kaynaklarını bulma

Azure Monitor günlüklerini kullanmanın bir avantajı, düğümler arasında başarısız denetimleri arayabilirsiniz. Başarısız olan DSC kaynaklarının tüm örneklerini bulmak için:

1. Günlük Analizi çalışma alanına Genel Bakış sayfasında **Günlükler'i**tıklatın.
1. Sorgu alanına aşağıdaki aramayı yazarak uyarınız için bir günlük arama sorgusu oluşturun:`Type=AzureDiagnostics Category='DscNodeStatus' OperationName='DscResourceStatusData' ResultType='Failed'`

### <a name="view-historical-dsc-node-status"></a>Geçmiş DSC düğüm durumunu görüntüleme

DSC düğüm durum geçmişinizi zaman içinde görselleştirmek için bu sorguyı kullanabilirsiniz:

`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=DscNodeStatus NOT(ResultType="started") | measure Count() by ResultType interval 1hour`

Bu sorgu, zaman içinde düğüm durumunun bir grafiğini görüntüler.

## <a name="azure-monitor-logs-records"></a>Azure Monitör kayıtları günlükleri

Azure Otomasyon tanılama, Azure Monitor günlüklerinde iki kayıt kategorisi oluşturur:

* Düğüm durum verileri (**DscNodeStatusData**)
* Kaynak durumu verileri (**DscResourceStatusData**)

### <a name="dscnodestatusdata"></a>DscNodeStatusData

| Özellik | Açıklama |
| --- | --- |
| TimeGenerated |Uyumluluk denetiminin çalıştırılandığı tarih ve saat. |
| ThrottledRequests |DscNodeStatusData. |
| Sonuç Türü |Düğümün uyumlu olup olmadığı. |
| NodeName_s |Yönetilen düğümün adı. |
| NodeComplianceStatus_s |Düğümün uyumlu olup olmadığı. |
| DscReportStatus |Uyumluluk denetiminin başarılı çalışıp çalıştırılmayacağı. |
| Yapılandırma Modu | Yapılandırmanın düğüme nasıl uygulandığı. Olası değerler şunlardır: <ul><li>`ApplyOnly`: DSC yapılandırmayı uygular ve hedef düğüme yeni bir yapılandırma itilmedikçe veya sunucudan yeni bir yapılandırma çekildiğinde başka bir şey yapmaz. Yeni bir yapılandırmanın ilk uygulamasından sonra, DSC önceden yapılandırılmış bir durumdan sürüklenme olup olmadığını denetlemez. `ApplyOnly` DSC, değer etkinleştirilmeden önce yapılandırmayı başarılı olana kadar uygulamaya çalışır. </li><li>`ApplyAndMonitor`: Bu varsayılan değerdir. LCM yeni yapılandırmalar uygular. Yeni bir yapılandırmanın ilk uygulamasından sonra, hedef düğüm istenilen durumdan sürükleniyorsa, DSC günlüklerde tutarsızlık bildirir. `ApplyAndMonitor` DSC, değer etkinleştirilmeden önce yapılandırmayı başarılı olana kadar uygulamaya çalışır.</li><li>`ApplyAndAutoCorrect`: DSC yeni yapılandırmalar uygular. Yeni bir yapılandırmanın ilk uygulamasından sonra, hedef düğüm istenilen durumdan sürükleniyorsa, DSC günlüklerde tutarsızlığı bildirir ve ardından geçerli yapılandırmayı yeniden uygular.</li></ul> |
| HostName_s | Yönetilen düğümün adı. |
| IPAddress | Yönetilen düğümün IPv4 adresi. |
| Kategori | DscNodeDurum. |
| Kaynak | Azure Otomasyon hesabının adı. |
| Tenant_g | Arayan için kiracıyı tanımlayan GUID. |
| NodeId_g |Yönetilen düğümü tanımlayan GUID. |
| DscReportId_g |Raporu tanımlayan GUID. |
| LastSeenTime_t |Raporun en son görüntülenilen tarih ve saat. |
| ReportStartTime_t |Raporun başlatıldıı tarih ve saat. |
| ReportEndTime_t |Raporun tamamlandığı tarih ve saat. |
| NumberOfResources_d |Düğüme uygulanan yapılandırmada çağrılan DSC kaynaklarının sayısı. |
| SourceSystem | Azure Monitor günlüklerinin verileri nasıl topladığı. Azure tanılama için her zaman "Azure" |
| ResourceId |Azure Otomasyon hesabının tanımlayıcısı. |
| Sonuç Açıklaması | Bu operasyonun açıklaması. |
| SubscriptionId | Otomasyon hesabı için Azure abonelik kimliği (GUID). |
| ResourceGroup | Otomasyon hesabı için kaynak grubunun adı. |
| ResourceProvider | Microsoft. Otomasyon. |
| ResourceType | OTOMASYON HESAPLARI. |
| CorrelationId |GUID, uyumluluk raporunun korelasyon tanımlayıcısIdır. |

### <a name="dscresourcestatusdata"></a>DscResourceStatusData

| Özellik | Açıklama |
| --- | --- |
| TimeGenerated |Uyumluluk denetiminin çalıştırılandığı tarih ve saat. |
| ThrottledRequests |DscResourceStatusData.|
| Sonuç Türü |Kaynağın uyumlu olup olmadığı. |
| NodeName_s |Yönetilen düğümün adı. |
| Kategori | DscNodeDurum. |
| Kaynak | Azure Otomasyon hesabının adı. |
| Tenant_g | Arayan için kiracıyı tanımlayan GUID. |
| NodeId_g |Yönetilen düğümü tanımlayan GUID. |
| DscReportId_g |Raporu tanımlayan GUID. |
| DscResourceId_s |DSC kaynak örneğinin adı. |
| DscResourceName_s |DSC kaynağının adı. |
| DscResourceStatus_s |DSC kaynağının uyumlu olup olmadığı. |
| DscModuleName_s |DSC kaynağını içeren PowerShell modülünün adı. |
| DscModuleVersion_s |DSC kaynağını içeren PowerShell modülünün sürümü. |
| DscConfigurationName_s |Düğüme uygulanan yapılandırmanın adı. |
| ErrorCode_s | Kaynak başarısız olduysa hata kodu. |
| ErrorMessage_s |Kaynak başarısız olduysa hata iletisi. |
| DscResourceDuration_d |DSC kaynağının saniyeler içinde çalıştırdığı saat. |
| SourceSystem | Azure Monitor günlüklerinin verileri nasıl topladığı. Azure `Azure` tanılama için her zaman. |
| ResourceId |Azure Otomasyon hesabını belirtir. |
| Sonuç Açıklaması | Bu operasyonun açıklaması. |
| SubscriptionId | Otomasyon hesabı için Azure abonelik kimliği (GUID). |
| ResourceGroup | Otomasyon hesabı için kaynak grubunun adı. |
| ResourceProvider | Microsoft. Otomasyon. |
| ResourceType | OTOMASYON HESAPLARI. |
| CorrelationId |GUID, uyumluluk raporunun korelasyon kimliğidir. |


## <a name="next-steps"></a>Sonraki adımlar

- Genel bakış için [bkz.](automation-dsc-overview.md)
- Başlamak için Azure [Otomasyon Durumu Yapılandırması'na başlarken](automation-dsc-getting-started.md)bkz.
- DSC yapılandırmalarını hedef düğümlerine atayabilmeniz için derleme hakkında bilgi edinmek için [bkz.](automation-dsc-compile.md)
- PowerShell cmdlet referansı için [Az.Automation'a](https://docs.microsoft.com/powershell/module/az.automation/?view=azps-3.7.0#automation
)bakın.
- Fiyatlandırma bilgileri için Azure [Otomasyon Durumu Yapılandırma fiyatlandırması'na](https://azure.microsoft.com/pricing/details/automation/)bakın.
- Sürekli dağıtım ardışık bir şekilde Azure Otomasyon Durumu Yapılandırmasını kullanma örneğini görmek için [bkz.](automation-dsc-cd-chocolatey.md)
- Farklı arama sorguları oluşturma ve Azure Monitor günlükleriyle Otomasyon Durumu Yapılandırma günlüklerini gözden geçirme hakkında daha fazla bilgi edinmek için [Azure Monitor günlüklerinde Günlük aramaları'na](../log-analytics/log-analytics-log-searches.md)bakın.
- Azure Monitor günlükleri ve veri toplama kaynakları hakkında daha fazla bilgi edinmek için [bkz.](../azure-monitor/platform/collect-azure-metrics-logs.md)
