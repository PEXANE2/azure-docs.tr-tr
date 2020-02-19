---
title: Azure Otomasyonu durum yapılandırması raporlama verilerini Azure Izleyici günlüklerine iletme
description: Bu makalede, Azure Otomasyonu durum yapılandırmasından Istenen durum yapılandırması (DSC) raporlama verilerinin, ek Öngörüler ve yönetim sağlamak için Azure Izleyici günlüklerine gönderilmesi gösterilmektedir.
services: automation
ms.service: automation
ms.subservice: dsc
author: mgoedtel
ms.author: magoedte
ms.date: 11/06/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 578fcf4cd03a2d4fc8400b9e84f53206750a588c
ms.sourcegitcommit: dfa543fad47cb2df5a574931ba57d40d6a47daef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/18/2020
ms.locfileid: "77430729"
---
# <a name="forward-azure-automation-state-configuration-reporting-data-to-azure-monitor-logs"></a>Azure Otomasyonu durum yapılandırması raporlama verilerini Azure Izleyici günlüklerine iletme

Azure Otomasyonu durum yapılandırması, düğüm durumu verilerini 30 gün boyunca tutar.
Bu verileri daha uzun bir süre için tutmayı tercih ediyorsanız, Log Analytics çalışma alanınıza düğüm durumu verileri gönderebilirsiniz.
Uyumluluk durumu, düğümler ve düğüm yapılandırmalarında tek DSC kaynakları için Azure portal veya PowerShell ile görünür.
Azure Izleyici günlükleri ile şunları yapabilirsiniz:

- Yönetilen düğümler ve bireysel kaynaklar için uyumluluk bilgileri alın
- Uyumluluk durumuna göre bir e-posta veya uyarı tetikleyin
- Yönetilen düğümleriniz genelinde gelişmiş sorgular yazma
- Otomasyon hesapları arasında uyumluluk durumu ile bağıntı
- Düğüm uyumluluk geçmişinizi zaman içinde görselleştirin

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites"></a>Önkoşullar

Automation durum yapılandırma raporlarınızı Azure Izleyici günlüklerine göndermeye başlamak için şunlar gerekir:

- [Azure PowerShell](/powershell/azure/overview) Kasım 2016 veya sonraki bir sürümü (v 2.3.0).
- Azure Otomasyonu hesabı. Daha fazla bilgi için bkz. [Azure Otomasyonu 'na giriş](automation-intro.md).
- Otomasyon & denetim hizmeti teklifiyle Log Analytics çalışma alanı. Daha fazla bilgi için bkz. [Azure izleyici 'de Log Analytics kullanmaya başlama](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal).
- En az bir Azure Otomasyonu durum yapılandırması düğümü. Daha fazla bilgi için bkz. [Azure Otomasyonu durum yapılandırmasına göre yönetim için makineleri ekleme](automation-dsc-onboarding.md).
- [Xdscdiagnostics](https://www.powershellgallery.com/packages/xDscDiagnostics/2.7.0.0) modülü, sürüm 2.7.0.0 veya üstü. Yükleme adımları için bkz. [Azure Otomasyonu Istenen durum yapılandırması sorunlarını giderme](./troubleshoot/desired-state-configuration.md).

## <a name="set-up-integration-with-azure-monitor-logs"></a>Azure Izleyici günlükleri ile tümleştirmeyi ayarlama

Azure Automation DSC Azure Izleyici günlüklerine veri aktarmaya başlamak için aşağıdaki adımları izleyin:

1. PowerShell 'de Azure hesabınızda oturum açın. Bkz. [Azure PowerShell oturum açma](https://docs.microsoft.com/powershell/azure/authenticate-azureps).
1. Aşağıdaki PowerShell cmdlet 'ini çalıştırarak Otomasyon hesabınızın kaynak KIMLIĞINI alın. Birden fazla otomasyon hesabınız varsa, yapılandırmak istediğiniz hesabın kaynak KIMLIĞINI seçin.

   ```powershell
   # Find the ResourceId for the Automation account
   Get-AzResource -ResourceType 'Microsoft.Automation/automationAccounts'
   ```

1. Aşağıdaki PowerShell cmdlet 'ini çalıştırarak Log Analytics çalışma alanınızın kaynak KIMLIĞINI alın. Birden fazla çalışma alanınız varsa, yapılandırmak istediğiniz çalışma alanının kaynak KIMLIĞINI seçin.

   ```powershell
   # Find the ResourceId for the Log Analytics workspace
   Get-AzResource -ResourceType 'Microsoft.OperationalInsights/workspaces'
   ```

1. Aşağıdaki PowerShell cmdlet 'ini çalıştırın, `<AutomationResourceId>` ve `<WorkspaceResourceId>` önceki adımların her bir yanındaki *RESOURCEID* değerleri ile değiştirin.

   ```powershell
   Set-AzDiagnosticSetting -ResourceId <AutomationResourceId> -WorkspaceId <WorkspaceResourceId> -Enabled $true -Category 'DscNodeStatus'
   ```

1. Azure Otomasyonu durum yapılandırmasından Azure Izleyici günlüklerine veri aktarmayı durdurmak istiyorsanız aşağıdaki PowerShell cmdlet 'ini çalıştırın.

   ```powershell
   Set-AzDiagnosticSetting -ResourceId <AutomationResourceId> -WorkspaceId <WorkspaceResourceId> -Enabled $false -Category 'DscNodeStatus'
   ```

## <a name="view-the-state-configuration-logs"></a>Durum yapılandırma günlüklerini görüntüleme

Otomasyon durumu yapılandırma verilerinize yönelik Azure Izleyici günlükleri ile tümleştirmeyi ayarladıktan sonra, durum yapılandırması (DSC) sayfasının sol bölmesindeki **izleme** bölümünde **Günlükler** ' i seçerek bunları görüntüleyebilirsiniz.

![Günlükler](media/automation-dsc-diagnostics/automation-dsc-logs-toc-item.png)

**Günlük araması** bölmesi, Otomasyon hesabı kaynağınız kapsamındaki bir sorgu bölgesi ile açılır. Azure Izleyici günlüklerinde arama yaparak DSC işlemlerine yönelik durum yapılandırma günlüklerinde arama yapabilirsiniz. DSC işlemlerine yönelik kayıtlar AzureDiagnostics tablosunda depolanır. Örneğin, uyumlu olmayan düğümleri bulmak için aşağıdaki sorguyu yazın.

```AzureDiagnostics
| where Category == 'DscNodeStatus' 
| where OperationName contains 'DSCNodeStatusData'
| where ResultType != 'Compliant'
```
Filtreleme ayrıntıları:

* Her durum yapılandırma düğümü için işlem döndürmek üzere *Dscnodestatusdata* öğesine filtre uygulayın.
* Bu kaynağa uygulanan düğüm yapılandırmasında çağrılan her DSC kaynağına yönelik işlemleri döndürmek için *Dscresourcestatusdata* üzerinde filtre uygulayın. 
* Başarısız olan tüm DSC kaynakları için hata bilgilerini döndürmek üzere *Dscresourcestatusdata* üzerinde filtreleme yapın.

Verileri bulmak için günlük sorguları oluşturma hakkında daha fazla bilgi edinmek için bkz. [Azure izleyici 'de günlük sorgularına genel bakış](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview).

### <a name="send-an-email-when-a-state-configuration-compliance-check-fails"></a>Bir durum yapılandırması uyumluluk denetimi başarısız olduğunda e-posta gönder

En popüler müşteri isteklerimizden biri, bir DSC yapılandırmasıyla bir sorun olduğunda e-posta veya metin gönderebilme özelliğine yöneliktir.

Uyarı kuralı oluşturmak için, uyarıyı çağırması gereken durum yapılandırma raporu kayıtları için bir günlük araması oluşturarak başlayın. Uyarı kuralını oluşturmak ve yapılandırmak için **+ Yeni uyarı kuralı** düğmesine tıklayın.

1. Log Analytics çalışma alanına genel bakış sayfasında **Günlükler**' e tıklayın.
1. Sorgu alanına aşağıdaki aramayı yazarak Uyarınız için bir günlük arama sorgusu oluşturun: `Type=AzureDiagnostics Category='DscNodeStatus' NodeName_s='DSCTEST1' OperationName='DscNodeStatusData' ResultType='Failed'`

   Çalışma alanınıza birden fazla otomasyon hesabından veya aboneliğine ait Günlükler ayarladıysanız, uyarılarınızı abonelik ve otomasyon hesabına göre gruplandırabilirsiniz. DscNodeStatusData aramasında kaynak alanından Otomasyon hesabı adını türetirsiniz.
1. **Kural oluştur** ekranını açmak için sayfanın üst kısmındaki **+ Yeni uyarı kuralı** ' na tıklayın. 

Uyarıyı yapılandırma seçenekleri hakkında daha fazla bilgi için bkz. [bir uyarı kuralı oluşturma](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md).

### <a name="find-failed-dsc-resources-across-all-nodes"></a>Tüm düğümlerde başarısız DSC kaynaklarını bul

Azure Izleyici günlüklerini kullanmanın avantajlarından biri, düğümler genelinde başarısız denetimleri arayabilmenizi sağlayabilir.
Başarısız olan DSC kaynaklarının tüm örneklerini bulmak için:

1. Log Analytics çalışma alanına genel bakış sayfasında **Günlükler**' e tıklayın.
1. Sorgu alanına aşağıdaki aramayı yazarak Uyarınız için bir günlük arama sorgusu oluşturun: `Type=AzureDiagnostics Category='DscNodeStatus' OperationName='DscResourceStatusData' ResultType='Failed'`

### <a name="view-historical-dsc-node-status"></a>Geçmiş DSC düğümü durumunu görüntüle

DSC düğüm durumu geçmişinizi zaman içinde görselleştirmek için bu sorguyu kullanabilirsiniz:

`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=DscNodeStatus NOT(ResultType="started") | measure Count() by ResultType interval 1hour`

Bu sorgu zaman içinde düğüm durumunun bir grafiğini görüntüler.

## <a name="azure-monitor-logs-records"></a>Azure Izleyici günlük kayıtları

Azure Otomasyonu tanılama, Azure Izleyici günlüklerinde iki kayıt kategorisi oluşturur:

* Düğüm durumu verileri (DscNodeStatusData)
* Kaynak durumu verileri (DscResourceStatusData)

### <a name="dscnodestatusdata"></a>DscNodeStatusData

| Özellik | Açıklama |
| --- | --- |
| TimeGenerated |Uyumluluk denetiminin çalıştırıldığı tarih ve saat. |
| için abonelik sınırlarını aştıysanız Hizmet Azaltma gerçekleşir |DscNodeStatusData. |
| ResultType |Düğümün uyumlu olup olmadığı. |
| NodeName_s |Yönetilen düğümün adı. |
| NodeComplianceStatus_s |Düğümün uyumlu olup olmadığı. |
| DscReportStatus |Uyumluluk denetiminin başarıyla çalışıp çalışmadığını belirtir. |
| ConfigurationMode | Yapılandırma düğüme nasıl uygulanır. Olası değerler şunlardır: <ul><li>*Yalnızca Apply:* DSC yapılandırmayı uygular ve hedef düğüme yeni bir yapılandırma itilemez veya bir sunucudan yeni bir yapılandırma çekilmediği takdirde hiçbir şey yapmaz. Yeni yapılandırmanın ilk uygulamasından sonra DSC, daha önce yapılandırılmış bir durumdan DRFT 'yi denetlemez. DSC, *yalnızca applyvalue* yürürlüğe girmeden önce başarılı olana kadar yapılandırmayı uygulamaya çalışır. </li><li>*Applmanagermonitor*: Bu, varsayılan değerdir. LCM, yeni yapılandırma uygular. Yeni yapılandırmanın ilk uygulamasından sonra, hedef düğüm istenen durumdan Drifts, DSC, günlüklerde tutarsızlığı raporlar. , *Applyandizleyici* değeri yürürlüğe girmeden önce başarılı olana kadar DSC 'nin yapılandırmayı uygulamaya çalışır.</li><li>*Applyandadutocorrect*: DSC yeni yapılandırma uygular. Yeni bir yapılandırmanın ilk uygulamasından sonra, hedef düğüm istenen durumdan Drifts, DSC günlüklerde tutarsızlığı raporlar ve ardından geçerli yapılandırmayı yeniden uygular.</li></ul> |
| HostName_s | Yönetilen düğümün adı. |
| IPAddress | Yönetilen düğümün IPv4 adresi. |
| Kategori | DscNodeStatus. |
| Kaynak | Azure Otomasyonu hesabının adı. |
| Tenant_g | Çağıran için kiracıyı tanımlayan GUID. |
| NodeId_g |Yönetilen düğümü tanımlayan GUID. |
| DscReportId_g |Raporu tanımlayan GUID. |
| LastSeenTime_t |Raporun en son görüntülendiği tarih ve saat. |
| ReportStartTime_t |Raporun başlatıldığı tarih ve saat. |
| ReportEndTime_t |Raporun tamamlandığı tarih ve saat. |
| NumberOfResources_d |Düğümde uygulanan yapılandırmada çağrılan DSC kaynaklarının sayısı. |
| SourceSystem | Azure Izleyici günlükleri verileri nasıl topladı. Azure Tanılama için her zaman "Azure". |
| ResourceId |Azure Otomasyonu hesabının tanımlayıcısı. |
| ResultDescription | Bu işlemin açıklaması. |
| kaynak grubundaki | Otomasyon hesabı için Azure abonelik KIMLIĞI (GUID). |
| adlı yönetilen örnek, | Otomasyon hesabı için kaynak grubunun adı. |
| ResourceProvider | MICROSOFT. Otomasyon. |
| ResourceType | AUTOMATIONACCOUNTS. |
| CorrelationId |Uyumluluk raporunun bağıntı tanımlayıcısı olan GUID. |

### <a name="dscresourcestatusdata"></a>DscResourceStatusData

| Özellik | Açıklama |
| --- | --- |
| TimeGenerated |Uyumluluk denetiminin çalıştırıldığı tarih ve saat. |
| için abonelik sınırlarını aştıysanız Hizmet Azaltma gerçekleşir |DscResourceStatusData|
| ResultType |Kaynağın uyumlu olup olmadığı. |
| NodeName_s |Yönetilen düğümün adı. |
| Kategori | DscNodeStatus. |
| Kaynak | Azure Otomasyonu hesabının adı. |
| Tenant_g | Çağıran için kiracıyı tanımlayan GUID. |
| NodeId_g |Yönetilen düğümü tanımlayan GUID. |
| DscReportId_g |Raporu tanımlayan GUID. |
| DscResourceId_s |DSC kaynak örneğinin adı. |
| DscResourceName_s |DSC kaynağının adı. |
| DscResourceStatus_s |DSC kaynağının uyumlu olup olmadığı. |
| DscModuleName_s |DSC kaynağını içeren PowerShell modülünün adı. |
| DscModuleVersion_s |DSC kaynağını içeren PowerShell modülünün sürümü. |
| DscConfigurationName_s |Düğüme uygulanan yapılandırmanın adı. |
| ErrorCode_s | Kaynak başarısız olursa hata kodu. |
| ErrorMessage_s |Kaynak başarısız olursa hata iletisi. |
| DscResourceDuration_d |DSC kaynağının çalıştırıldığı saniye cinsinden süre. |
| SourceSystem | Azure Izleyici günlükleri verileri nasıl topladı. Azure için her zaman *Azure* tanılama. |
| ResourceId |Azure Otomasyonu hesabını belirtir. |
| ResultDescription | Bu işlemin açıklaması. |
| kaynak grubundaki | Otomasyon hesabı için Azure abonelik KIMLIĞI (GUID). |
| adlı yönetilen örnek, | Otomasyon hesabı için kaynak grubunun adı. |
| ResourceProvider | MICROSOFT. Otomasyon. |
| ResourceType | AUTOMATIONACCOUNTS. |
| CorrelationId |Uyumluluk raporunun bağıntı KIMLIĞI olan GUID. |

## <a name="summary"></a>Özet

Otomasyon durumu yapılandırma verilerinizi Azure Izleyici günlüklerine göndererek, otomasyon durumu yapılandırma düğümlerinizin durumu hakkında daha iyi öngörüler elde edebilirsiniz:

- Sorun olduğunda sizi bilgilendirmek için Uyarılar ayarlama
- Runbook sonuçlarınızı, runbook iş durumunu ve diğer ilgili anahtar göstergelerini veya ölçümlerini görselleştirmek için özel görünümler ve arama sorguları kullanma.

Azure Izleyici günlükleri, otomasyon durumu yapılandırma verilerinize daha fazla işletimsel görünürlük sağlar ve olayların daha hızlı bir şekilde ele sağlanmasına yardımcı olabilir.

## <a name="next-steps"></a>Sonraki adımlar

- Genel bakış için bkz. [Azure Otomasyonu durum yapılandırması](automation-dsc-overview.md)
- Başlamak için bkz. [Azure Otomasyonu durum yapılandırması ile çalışmaya](automation-dsc-getting-started.md) başlama
- Hedef düğümlere atayabilmeniz için DSC yapılandırmalarını derleme hakkında bilgi edinmek için bkz. [Azure Otomasyonu durum yapılandırmasında yapılandırmaları derleme](automation-dsc-compile.md)
- PowerShell cmdlet başvurusu için bkz. [Azure Otomasyonu durum yapılandırması cmdlet 'leri](/powershell/module/azurerm.automation/#automation)
- Fiyatlandırma bilgileri için bkz. [Azure Otomasyonu durum yapılandırması fiyatlandırması](https://azure.microsoft.com/pricing/details/automation/)
- Azure Otomasyonu durum yapılandırması 'nı sürekli bir dağıtım ardışık düzeninde kullanmaya ilişkin bir örnek görmek için bkz. [Azure Otomasyonu durum yapılandırması ve Chocolatey kullanarak sürekli dağıtım](automation-dsc-cd-chocolatey.md)
- Farklı arama sorguları oluşturma ve Azure Izleyici günlükleriyle Otomasyon durumu yapılandırma günlüklerini İnceleme hakkında daha fazla bilgi edinmek için bkz. [Azure izleyici günlüklerinde günlük aramaları](../log-analytics/log-analytics-log-searches.md)
- Azure Izleyici günlükleri ve veri toplama kaynakları hakkında daha fazla bilgi edinmek için bkz. Azure [depolama verilerini Azure izleyici günlüklerine toplama genel bakış](../azure-monitor/platform/collect-azure-metrics-logs.md)
