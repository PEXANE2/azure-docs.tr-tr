---
title: Kurtarma Hizmetleri Vault'ları için tanılama ayarlarını kullanma
description: Azure Yedekleme için eski ve yeni tanılama olaylarının nasıl kullanılacağını açıklayan bir makale
ms.topic: conceptual
ms.date: 10/30/2019
ms.openlocfilehash: d10bedf3818559971eff12624152d0e797f6c3cc
ms.sourcegitcommit: b129186667a696134d3b93363f8f92d175d51475
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80672792"
---
# <a name="using-diagnostics-settings-for-recovery-services-vaults"></a>Kurtarma Hizmetleri Kasaları için Tanılama Olaylarını kullanma

Azure Yedekleme, analiz, uyarı ve raporlama amacıyla toplanıp kullanılabilen tanılama olayları gönderir. 

Azure portalı üzerinden, kasaya gidip **Tanı ayarları** menü öğesini tıklatarak Kurtarma Hizmetleri Kasası için tanılama ayarlarını yapılandırabilirsiniz. **+ Ekle Tanılama Ayarı'nı** tıklattığınızda, bir Depolama Hesabı,Olay Hub'ına veya Günlük Analizi (LA) Çalışma alanına bir veya daha fazla tanılama olayı göndermenize olanak tanır.

![Teşhis Ayarları Blade](./media/backup-azure-diagnostics-events/diagnostics-settings-blade.png)

## <a name="diagnostics-events-available-for-azure-backup-users"></a>Azure Yedekleme Kullanıcıları Için Tanılama Etkinlikleri Kullanılabilir

Azure Yedekleme, her biri yedeklemeyle ilgili belirli bir yapıt kümesi hakkında ayrıntılı veri sağlayan aşağıdaki tanılama olaylarını sağlar:

* CoreAzureYedekleme
* AddonAzureBackupAlerts
* AddonAzureBackupProtectedInstance
* AddonAzureBackupJobs
* AddonAzureBackupPolicy
* AddonAzureBackupDepolama

[Azure Yedekleme Tanılama Etkinlikleri için Veri Modeli](https://docs.microsoft.com/azure/backup/backup-azure-reports-data-model)

Bu olaylara ait veriler bir depolama hesabına, LA çalışma alanına veya Olay Hub'ına gönderilebilir. Bu verileri bir LA Çalışma alanına gönderiyorsanız, **Tanılama Ayarı** ekranında **Kaynağa Özel** geçiş seçeneğini seçmeniz gerekir (aşağıdaki bölümlerde daha fazla bilgi görün).

## <a name="using-diagnostics-settings-with-log-analytics-la"></a>Günlük Analitiği (LA) ile Tanılama Ayarlarını Kullanma

Azure Log Analytics yol haritasıyla uyumlu olan Azure Yedekleme, artık yedekleme için ayrılmış LA tablolarına kasa tanılama verileri göndermenize olanak tanır. Bunlara [Kaynağa Özel tablolar](https://docs.microsoft.com/azure/azure-monitor/platform/resource-logs-collect-workspace#resource-specific)denir.

Kasa tanılama verilerinizi LA'ye göndermek için:

1.    Kasanıza gidin ve **Tanı ayarları'na**tıklayın. + **Tanı ayarını ekle'yi**tıklatın.
2.    Tanılama ayarına bir ad verin.
3.    **Günlük Analitiği Gönder** kutusunu işaretleyin ve bir Log Analytics Çalışma Alanı seçin.
4.    Geçişte **Özel Kaynak'ı** seçin ve aşağıdaki altı olayı kontrol edin - **CoreAzureBackup ,** **AddonAzureBackupAlerts**, **AddonAzureBackupProtectedInstance**, **AddonAzureBackupJobsJobs**, **AddonAzureBackupPolicy**ve **AddonAzureBackupStorage**.
5.    **Kaydet**'e tıklayın.

![Kaynağa Özel mod](./media/backup-azure-diagnostics-events/resource-specific-blade.png)

Veriler LA Çalışma alanına aktında, bu olayların her biri için ayrılmış tablolar çalışma alanınızda oluşturulur. Bu tablolardan herhangi birini doğrudan sorgulayabilir ve gerekirse bu tablolar arasında birleştirme veya birleştirme gerçekleştirebilirsiniz.

> [!IMPORTANT]
> Yukarıdaki altı olay, yani, CoreAzureBackup, AddonAzureBackupAlerts, AddonAzureBackupProtectedInstance, AddonAzureBackupJobs, AddonAzureBackupBackupPolicy ve AddonAzureBackupStorage, [Yedek Raporlar'da](https://docs.microsoft.com/azure/backup/configure-reports) **yalnızca** Kaynak Özel Modu'nda desteklenir. **Azure Tanılama Modu'nda bu altı olay için veri göndermeye çalışırsanız, Yedekleme Raporları'nda hiçbir veri görünmez.**

## <a name="legacy-event"></a>Eski Etkinlik

Geleneksel olarak, bir kasaiçin yedeklemeyle ilgili tüm tanılama verileri 'AzureBackupReport' adlı tek bir etkinlikte yer almaktadır. Yukarıda açıklanan altı olay, özünde AzureBackupReport'da yer alan tüm verilerin ayrıştırıldığıbir olaydır. 

Şu anda, kullanıcıların bu etkinlikte (örneğin, özel günlük uyarıları, özel görselleştirmeler vb.) varolan özel sorguları olduğu durumlarda, geriye dönük uyumluluk için AzureBackupReport etkinliğini desteklemeye devam ediyoruz. Ancak, ** [yeni olaylara](https://docs.microsoft.com/azure/backup/backup-azure-diagnostic-events#diagnostics-events-available-for-azure-backup-users) mümkün olduğunca erken taşınmanızı öneririz**, çünkü bu günlük sorgularında verileri çok daha kolay hale getirir, şemaların ve yapılarının daha iyi keşfedilebilirliğini sağlar, hem yutma gecikmesi hem de sorgu süreleri boyunca performansı artırır. 

**Azure Tanılama modundaki eski olay sonunda küçümsenecek ve bu nedenle yeni etkinlikleri seçmek daha sonraki bir tarihte karmaşık geçişleri önlemenize yardımcı olabilir.** Log Analytics'ten yararlanan [raporlama çözümümüz,](https://docs.microsoft.com/azure/backup/configure-reports) eski etkinlikteki verileri desteklemeyi de durduracaktır.

### <a name="steps-to-move-to-new-diagnostics-settings-to-log-analytics-workspace"></a>Yeni tanılama ayarlarına geçme adımları (Log Analytics çalışma alanına)

1. Eski olayı ve ait oldukları abonelikleri kullanarak hangi kasaların Log Analytics Workspace(ler) adresine veri gönderdiğini belirleyin. Bu kasaları ve abonelikleri tanımlamak için aşağıdaki çalışma alanlarını çalıştırın:

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

2. Belirli bir kapsamdaki tüm kasalar için yeni bir tanılama ayarı eklemek için Azure Yedekleme'nin yerleşik Azure İlkesi'ni kullanın. [built-in Azure Policy](https://docs.microsoft.com/azure/backup/azure-policy-configure-diagnostics) Bu ilke, tanılama ayarı (veya) yalnızca eski bir tanılama ayarı olmayan bu tonozlara yeni bir tanılama ayarı ekler. Bu ilke, aynı anda tüm bir abonelik veya kaynak grubuna atanabilir. İlkenin atandığı her abonelik için 'Sahip' erişimine ihtiyacınız olacağını unutmayın.

AzureBackupReport ve altı yeni olay için ayrı tanılama ayarlarına sahip olmayı seçebilirsiniz, ta ki yeni tablolardaki verileri kullanmak için tüm özel sorgularınızı geçirene kadar. Aşağıdaki resimde, iki tanılama ayarı olan bir tonoz örneği gösterilmektedir. **Ayar1** adlı ilk ayar, AzureBackupReport olayının verilerini AzureDiagnostics modundabir LA Çalışma alanına gönderir. **Ayar2** adlı ikinci ayar, kaynak özel modunda bir LA Çalışma alanı için altı yeni Azure Yedekleme olayların veri gönderir.

![İki Ayar](./media/backup-azure-diagnostics-events/two-settings-example.png)

> [!IMPORTANT]
> AzureBackupReport etkinliği **yalnızca** Azure Tanılama Modunda desteklenir. **Kaynak Özel Modu'nda bu olay için veri göndermeye çalışırsanız, LA Çalışma Alanına hiçbir veri akmaz.**

> [!NOTE]
> Azure Tanılama / Kaynak Özel için geçiş yalnızca kullanıcı **Günlük Analizine Gönder'i**denetlerse görüntülenir. Bir Depolama Hesabı'na veya Etkinlik Hub'ına veri göndermek için, kullanıcı gerekli hedefi seçebilir ve herhangi bir ek girdi olmadan istenen olaylardan herhangi birini denetleyebilir. Yine, ileriye dönük eski olay AzureBackupReport'u seçmemenizi önerilir.

## <a name="users-sending-azure-site-recovery-events-to-log-analytics-la"></a>Azure Site Kurtarma Etkinliklerini Günlük Analitiğine (LA) gönderen kullanıcılar

Azure Yedekleme ve Azure Site Kurtarma Etkinlikleri aynı Kurtarma Hizmetleri Kasası'ndan gönderilir. Azure Site Kurtarma şu anda Kaynağa Özel tablolarda yer almadığı için, Azure Site Kurtarma Etkinlikleri'ni LA'ye göndermek isteyen kullanıcılar **yalnızca** Azure Tanılama Modu'nu kullanmaya yönlendirilir (aşağıdaki resme bakın). **Azure Sitesi Kurtarma Etkinlikleri için Kaynak Özel Modu'nu seçmek, gerekli verilerin LA Çalışma Alanına gönderilmesini engeller.**

![Site Kurtarma Etkinlikleri](./media/backup-azure-diagnostics-events/site-recovery-settings.png)

Yukarıdaki nüansları özetlemek gerekirse:

* Azure Tanılama ile ayarlanmış la tanılama zaten varsa ve bunun üzerine özel sorgular yazdıysanız, sorgularınızı yeni olaylardan gelen verileri kullanmak üzere geçiş yapana kadar bu ayarı **bozulmadan**tutun.
* Ayrıca yeni tablolara (önerilen) dahil olmak istiyorsanız, **yeni** bir tanılama ayarı oluşturun, **Kaynak Özel'i** seçin ve yukarıda belirtildiği gibi altı yeni olayı seçin.
* Şu anda Azure Site Kurtarma Etkinlikleri'ni LA'ye gönderiyorsanız, bu olaylar için Kaynağa Özel modunu **seçmeyin,** aksi takdirde bu olaylara ait veriler LA Çalışma Alanınıza akmaz. Bunun yerine, ek bir **tanılama ayarı**oluşturun, **Azure Tanılama'yı**seçin ve ilgili Azure Site Kurtarma olaylarını seçin.

Aşağıdaki resimde, bir tonoz için üç tanılama ayarı olan bir kullanıcı örneği gösterilmektedir. **Ayar1** adlı ilk ayar, AzureBackupReport etkinliğinden AzureDiagnostics modunda bir LA Çalışma alanına veri gönderir. **Ayar2** adlı ikinci ayar, altı yeni Azure Yedekleme olayından verileri Kaynak Özel modunda bir LA Çalışma alanına gönderir. **Ayar3**adlı üçüncü ayar, Azure Site Kurtarma etkinliklerinden verileri Azure Tanılama Modu'ndaki bir LA Çalışma alanına gönderir.

![Üç Ayar](./media/backup-azure-diagnostics-events/three-settings-example.png)

## <a name="next-steps"></a>Sonraki adımlar

[Teşhis Etkinlikleri için Log Analytics Veri Modelini öğrenin](https://docs.microsoft.com/azure/backup/backup-azure-reports-data-model)
