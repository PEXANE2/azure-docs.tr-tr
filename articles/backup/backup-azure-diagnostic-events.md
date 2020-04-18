---
title: Kurtarma Hizmetleri kasaları için tanılama ayarlarını kullanma
description: Bu makalede, Azure Yedekleme için eski ve yeni tanılama olaylarının nasıl kullanılacağı açıklanmaktadır.
ms.topic: conceptual
ms.date: 10/30/2019
ms.openlocfilehash: f11c9d2a5b48b9cd27ac6e6f8a00eb5ac0ac7a9d
ms.sourcegitcommit: eefb0f30426a138366a9d405dacdb61330df65e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81617305"
---
# <a name="use-diagnostics-settings-for-recovery-services-vaults"></a>Kurtarma Hizmetleri kasaları için tanılama ayarlarını kullanma

Azure Yedekleme, analiz, uyarı ve raporlama amacıyla toplanıp kullanılabilen tanılama olayları gönderir.

Azure portalı üzerinden Kurtarma Hizmetleri kasası için tanılama ayarlarını kasaya giderek **Tanılama ayarlarını**seçerek yapılandırabilirsiniz. Selecting **+ Add Diagnostic Ayarı,** bir depolama hesabına, etkinlik merkezine veya Log Analytics çalışma alanına bir veya daha fazla tanıolayı göndermenize olanak tanır.

![Tanılama ayarları bölmesi](./media/backup-azure-diagnostics-events/diagnostics-settings-blade.png)

## <a name="diagnostics-events-available-for-azure-backup-users"></a>Azure Yedekleme kullanıcıları için tanılama etkinlikleri

Azure Yedekleme aşağıdaki tanılama olaylarını sağlar. Her olay, yedeklemeyle ilgili belirli bir yapıt kümesi hakkında ayrıntılı veri sağlar:

* CoreAzureYedekleme
* AddonAzureBackupAlerts
* AddonAzureBackupProtectedInstance
* AddonAzureBackupJobs
* AddonAzureBackupPolicy
* AddonAzureBackupDepolama

Daha fazla bilgi için [Azure Yedekleme tanılama olayları için Veri modeline](https://docs.microsoft.com/azure/backup/backup-azure-reports-data-model)bakın.

Bu olaylara ait veriler bir depolama hesabına, Log Analytics çalışma alanına veya bir etkinlik merkezine gönderilebilir. Bu verileri Bir Log Analytics çalışma alanına gönderiyorsanız, **Tanılama ayarları** ekranında **Kaynağa özel** geçiş seçeneğini belirleyin. Daha fazla bilgi için aşağıdaki bölümlere bakın.

## <a name="use-diagnostics-settings-with-log-analytics"></a>Log Analytics ile tanılama ayarlarını kullanma

Artık yedekleme için özel Log Analytics tablolarına kasa tanılama verilerini göndermek için Azure Yedekleme'yi kullanabilirsiniz. Bu [tablolara kaynağa özgü tablolar](https://docs.microsoft.com/azure/azure-monitor/platform/resource-logs-collect-workspace#resource-specific)denir.

Kasa tanılama verilerinizi Log Analytics'e göndermek için:

1. Kasanıza gidin ve **Tanılama Ayarları'nı**seçin. Seçin **+ Tanılama Ayarı Ekle**.
1. Tanılama ayarına bir ad verin.
1. Günlük **Analitiği** Gönder onay kutusunu seçin ve Bir Günlük Analizi çalışma alanı seçin.
1. Geçişte **özel Kaynak'ı** seçin ve aşağıdaki altı olayı seçin: **CoreAzureBackup**, **AddonAzureBackupJobs,** **AddonAzureBackupAlerts,** **AddonAzureBackupPolicy**, **AddonAzureBackupStorage, addonAzureBackupStorage**, ve **AddonAzureBackupProtectedInstance**.
1. **Kaydet**’i seçin.

   ![Kaynağa özel mod](./media/backup-azure-diagnostics-events/resource-specific-blade.png)

Günlük Analitiği çalışma alanına veri akışı ndan sonra, bu olayların her biri için özel tablolar çalışma alanınızda oluşturulur. Bu tablolardan herhangi birini doğrudan sorgulayabilirsiniz. Gerekirse bu tablolar arasında birleştirme veya birleştirme gerçekleştirebilirsiniz.

> [!IMPORTANT]
> Altı olaylar, yani, CoreAzureBackup, AddonAzureBackupJobss, AddonAzureBackupAlerts, AddonAzureBackupPolicy, AddonAzureBackupStorage ve AddonAzureBackupBackupStorage, yedek [raporlarda](https://docs.microsoft.com/azure/backup/configure-reports)kaynak özel modunda *yalnızca* desteklenir. *Azure tanılama modunda bu altı olay için veri göndermeye çalışırsanız, Yedekleme raporlarında hiçbir veri görünmez.*

## <a name="legacy-event"></a>Eski olay

Geleneksel olarak, bir kasaiçin yedeklemeyle ilgili tüm tanılama verileri AzureBackupReport adı verilen tek bir etkinlikte yer alıyordu. Burada açıklanan altı olay, özünde AzureBackupReport'da yer alan tüm verilerin ayrıştırıldığıbir olaydır. 

Şu anda, kullanıcıların bu olayla ilgili varolan özel sorguları olduğu durumlarda geriye dönük uyumluluk için AzureBackupReport etkinliğini desteklemeye devam ediyoruz. Örnekler özel günlük uyarıları ve özel görselleştirmeler vardır. * [Yeni etkinliklere](https://docs.microsoft.com/azure/backup/backup-azure-diagnostic-events#diagnostics-events-available-for-azure-backup-users) mümkün olduğunca erken geçmenizi öneririz.* Yeni olaylar:

- Günlük sorgularında verilerin daha kolay çalışılamasını kolaylaştırın.
- Şemaların ve yapılarının daha iyi keşfedilebilirliğini sağlar.
- Hem yutma gecikmesi hem de sorgu süreleri boyunca performansı artırın. 

*Azure tanılama modundaki eski olay sonunda amortismana tabi olacaktır. Yeni olayları seçmek, daha sonraki bir tarihte karmaşık geçişleri önlemenize yardımcı olabilir.* Log Analytics'i kullanan [raporlama çözümümüz,](https://docs.microsoft.com/azure/backup/configure-reports) eski etkinlikteki verileri desteklemeyi de durdurur.

### <a name="steps-to-move-to-new-diagnostics-settings-for-a-log-analytics-workspace"></a>Log Analytics çalışma alanı için yeni tanılama ayarlarına geçme adımları

1. Eski olayı ve ait oldukları abonelikleri kullanarak log Analytics çalışma alanlarına hangi kasaların veri gönderdiğini belirleyin. Bu kasaları ve abonelikleri tanımlamak için aşağıdaki çalışma alanlarını çalıştırın.

    ````Kusto
    let RangeStart = startofday(ago(3d));
    let VaultUnderAzureDiagnostics = (){
        AzureDiagnostics
        | where TimeGenerated >= RangeStart | where Category == "AzureBackupReport" and OperationName == "Vault" and SchemaVersion_s == "V2"
        | summarize arg_max(TimeGenerated, *) by ResourceId    
        | project ResourceId, Category};
    let VaultUnderResourceSpecific = (){
        CoreAzureBackup
        | where TimeGenerated >= RangeStart | where OperationName == "Vault" 
        | summarize arg_max(TimeGenerated, *) by ResourceId
        | project ResourceId, Category};
        // Some Workspaces will not have AzureDiagnostics Table, hence you need to use isFuzzy
    let CombinedVaultTable = (){
        CombinedTable | union isfuzzy = true 
        (VaultUnderAzureDiagnostics() ),
        (VaultUnderResourceSpecific() )
        | distinct ResourceId, Category};
    CombinedVaultTable | where Category == "AzureBackupReport"
    | join kind = leftanti ( 
    CombinedVaultTable | where Category == "CoreAzureBackup"
    ) on ResourceId
    | parse ResourceId with * "SUBSCRIPTIONS/" SubscriptionId:string "/RESOURCEGROUPS" * "MICROSOFT.RECOVERYSERVICES/VAULTS/" VaultName:string
    | project ResourceId, SubscriptionId, VaultName
    ````

1. Belirli bir kapsamdaki tüm kasalar için yeni bir tanılama ayarı eklemek için Azure Yedekleme'deki [yerleşik Azure ilkesini](https://docs.microsoft.com/azure/backup/azure-policy-configure-diagnostics) kullanın. Bu ilke, tanılama ayarı olmayan veya yalnızca eski bir tanılama ayarı olan kasalara yeni bir tanılama ayarı ekler. Bu ilke, aynı anda tüm bir abonelik veya kaynak grubuna atanabilir. İlkenin atandığı her aboneye Sahip erişiminiz olmalıdır.

AzureBackupReport için ayrı tanılama ayarlarına ve altı yeni olaya sahip olmayı seçebilirsiniz, ta ki yeni tablolardaki verileri kullanmak için tüm özel sorgularınızı geçirene kadar. Aşağıdaki resimde, iki tanılama ayarı olan bir kasa örneği gösterilmektedir. **Ayar1**adlı ilk ayar, Azure Yedekleme Raporu etkinliğinin verilerini Azure tanılama modundaki Log Analytics çalışma alanına gönderir. **Ayar2**adlı ikinci ayar, altı yeni Azure Yedekleme olayının verilerini kaynağa özgü moddaki bir Log Analytics çalışma alanına gönderir.

![İki ayar](./media/backup-azure-diagnostics-events/two-settings-example.png)

> [!IMPORTANT]
> AzureBackupReport etkinliği *yalnızca* Azure tanılama modunda desteklenir. *Bu olay için kaynağa özel modda veri göndermeye çalışırsanız, Log Analytics çalışma alanına hiçbir veri akmaz.*

> [!NOTE]
> **Azure tanılama** veya **Kaynak özel** geçiş yalnızca kullanıcı **Günlük Analytics'e Gönder'i**seçerse görüntülenir. Bir depolama hesabına veya olay merkezine veri göndermek için, kullanıcı gerekli hedefi seçer ve herhangi bir ek giriş olmadan istenen olaylardan herhangi biri için onay kutularını seçer. Yine, ileriye dönük eski azurebackupreport olayını seçmemenizi öneririz.

## <a name="send-azure-site-recovery-events-to-log-analytics"></a>Azure Site Kurtarma etkinliklerini Günlük Analitiğine Gönderme

Azure Yedekleme ve Azure Site Kurtarma olayları aynı Kurtarma Hizmetleri kasasından gönderilir. Azure Site Kurtarma şu anda kaynağa özel tablolar için kullanılamıyor. Azure Site Kurtarma olaylarını Log Analytics'e göndermek isteyen kullanıcılar, resimde gösterildiği gibi *yalnızca*Azure tanılama modunu kullanmaya yönlendirilir. *Azure Site Kurtarma etkinlikleri için kaynağa özel modun seçilmesi, gerekli verilerin Günlük Analizi çalışma alanına gönderilmesini engeller.*

![Site Kurtarma etkinlikleri](./media/backup-azure-diagnostics-events/site-recovery-settings.png)

Özetlersek:

* Azure Diagnostics ile ayarlanmış Log Analytics tanılama zaten niz varsa ve bunun üzerine özel sorgular yazdıysanız, sorgularınızı yeni olaylardan elde edilen verileri kullanmak üzere geçiş yapana kadar bu ayarı *sağlam* tutun.
* Ayrıca, tavsiye ettiğimiz gibi yeni tablolara dahil olmak istiyorsanız, **yeni** bir tanılama ayarı oluşturun, **Kaynak'a özgü'yi**seçin ve altı yeni olayı seçin.
* Şu anda Azure Site Kurtarma etkinliklerini Log Analytics'e gönderiyorsanız, bu etkinlikler için kaynağa özel modu *seçmeyin.* Aksi takdirde, bu etkinliklere ait veriler Log Analytics çalışma alanınıza akmaz. Bunun yerine, ek bir tanılama ayarı oluşturun, **Azure tanılama'yı**seçin ve ilgili Azure Site Kurtarma olaylarını seçin.

Aşağıdaki resim, bir kasa için üç tanılama ayarı olan bir kullanıcının bir örneğini gösterir. **Ayar1**adlı ilk ayar, bir AzureBackupReport etkinliğinden Azure tanılama modunda bir Log Analytics çalışma alanına veri gönderir. **Ayar2**adlı ikinci ayar, altı yeni Azure Yedekleme olayından verileri kaynağa özgü moddaki Bir Log Analytics çalışma alanına gönderir. **Ayar3**adlı üçüncü ayar, Azure Site Kurtarma etkinliklerinden verileri Azure tanılama modundaki Log Analytics çalışma alanına gönderir.

![Üç ayar](./media/backup-azure-diagnostics-events/three-settings-example.png)

## <a name="next-steps"></a>Sonraki adımlar

[Tanılama olayları için Log Analytics veri modelini öğrenin](https://docs.microsoft.com/azure/backup/backup-azure-reports-data-model)
