---
title: Azure Monitörü ile Azure Yedekleme'yi izleme
description: Azure Yedekleme iş yüklerini izleyin ve Azure Monitor'u kullanarak özel uyarılar oluşturun.
ms.topic: conceptual
ms.date: 06/04/2019
ms.assetid: 01169af5-7eb0-4cb0-bbdb-c58ac71bf48b
ms.openlocfilehash: 547cef66be9902468f4e2755c31e5f586eccad5e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79459523"
---
# <a name="monitor-at-scale-by-using-azure-monitor"></a>Azure Monitör'ü kullanarak ölçekte izleme

Azure Yedekleme, Kurtarma Hizmetleri kasasında [yerleşik izleme ve uyarı özellikleri](backup-azure-monitoring-built-in-monitor.md) sağlar. Bu yetenekler herhangi bir ek yönetim altyapısı olmadan kullanılabilir. Ancak bu yerleşik hizmet aşağıdaki senaryolarda sınırlıdır:

- Abonelikler arasında birden çok Kurtarma Hizmetleri kasasından gelen verileri izlerseniz
- Tercih edilen bildirim kanalı e-posta *değilse*
- Kullanıcılar daha fazla senaryo için uyarı istiyorsa
- Azure'daki System Center Data Protection Manager gibi şirket içi bir bileşenden bilgi görüntülemek istiyorsanız ve portal [**Yedekleme İşleri**](backup-azure-monitoring-built-in-monitor.md#backup-jobs-in-recovery-services-vault) veya [**Yedekleme Uyarıları'nda**](backup-azure-monitoring-built-in-monitor.md#backup-alerts-in-recovery-services-vault) görünmüyor

## <a name="using-log-analytics-workspace"></a>Günlük Analizi çalışma alanını kullanma

### <a name="create-alerts-by-using-log-analytics"></a>Log Analytics'i kullanarak uyarılar oluşturma

Azure Monitör'de, Bir Günlük Analizi çalışma alanında kendi uyarılarınızı oluşturabilirsiniz. Çalışma alanında, tercih ettiğiniz bildirim mekanizmasını seçmek için *Azure eylem gruplarını* kullanırsınız.

> [!IMPORTANT]
> Bu sorguyu oluşturma maliyeti hakkında daha fazla bilgi için [Azure Monitor fiyatlandırması](https://azure.microsoft.com/pricing/details/monitor/)bölümüne bakın.

Log Analytics çalışma alanının **Günlükler** bölümünü açın ve kendi Günlükleriniz için bir sorgu oluşturun. **Yeni Uyarı Kuralı'nı**seçtiğinizde, Aşağıdaki resimde gösterildiği gibi Azure Monitor uyarı oluşturma sayfası açılır.

![Log Analytics çalışma alanında uyarı oluşturma](media/backup-azure-monitoring-laworkspace/custom-alert.png)

Burada kaynak zaten Log Analytics çalışma alanı olarak işaretlenir ve eylem grubu tümleştirmesi sağlanır.

![Log Analytics uyarı oluşturma sayfası](media/backup-azure-monitoring-laworkspace/inkedla-azurebackup-createalert.jpg)

#### <a name="alert-condition"></a>Uyarı durumu

Bir uyarının belirleyici özelliği tetikleyici durumudur. Kusto sorgusunu aşağıdaki resimde gösterildiği gibi **Günlükler** sayfasında otomatik olarak yüklemek için **Koşul'u** seçin. Burada, gereksinimlerinize uygun durumu edinebilirsiniz. Daha fazla bilgi için [Örnek Kusto sorgularına](#sample-kusto-queries)bakın.

![Uyarı koşulu ayarlama](media/backup-azure-monitoring-laworkspace/la-azurebackup-alertlogic.png)

Gerekirse, Kusto sorgusunu da yapabilirsiniz. Bir eşik, nokta ve sıklık seçin. Eşik, uyarının ne zaman yükseltileceğini belirler. Dönem, sorgunun çalıştırıldığı zaman penceresidir. Örneğin, eşik 0'dan büyükse, dönem 5 dakika ve sıklık 5 dakikaysa, kural sorguyu her 5 dakikada bir çalıştırır ve önceki 5 dakikayı gözden geçirilir. Sonuç sayısı 0'dan büyükse, seçili eylem grubu tarafından bildirilirsiniz.

#### <a name="alert-action-groups"></a>Eylem gruplarını uyar

Bildirim kanalı belirtmek için bir eylem grubu kullanın. **Eylem grupları**altında kullanılabilir bildirim mekanizmalarını görmek için **Yeni Oluştur'u**seçin.

!["Eylem grubu ekle" penceresinde kullanılabilir bildirim mekanizmaları](media/backup-azure-monitoring-laworkspace/LA-AzureBackup-ActionGroup.png)

Log Analytics'in tüm uyarı ve izleme gereksinimlerini tek başına karşılayabilir veya yerleşik bildirimleri tamamlamak için Log Analytics'i kullanabilirsiniz.

Daha fazla bilgi için azure portalında Azure Monitor ve [Create ve Manage eylem gruplarını](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups) [kullanarak günlük uyarıları oluştur, görüntüle ve yönet'](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log) e bakın.

### <a name="sample-kusto-queries"></a>Örnek Kusto sorguları

Varsayılan grafikler, üzerinde uyarılar oluşturabileceğiniz temel senaryolar için Kusto sorguları verir. Ayrıca, uyarılmayı istediğiniz verileri almak için sorguları değiştirebilirsiniz. **Günlükler** sayfasına aşağıdaki örnek Kusto sorgularını yapıştırın ve ardından sorgularda uyarılar oluşturun:

- Tüm başarılı yedekleme işleri

    ````Kusto
    AddonAzureBackupJobs
    | where JobOperation=="Backup"
    | where JobStatus=="Completed"
    ````

- Tüm başarısız yedekleme işleri

    ````Kusto
    AddonAzureBackupJobs
    | where JobOperation=="Backup"
    | where JobStatus=="Failed"
    ````

- Tüm başarılı Azure VM yedekleme işleri

    ````Kusto
    AddonAzureBackupJobs
    | where JobOperation=="Backup"
    | where JobStatus=="Completed"
    | join kind=inner
    (
        CoreAzureBackup
        | where OperationName == "BackupItem"
        | where BackupItemType=="VM" and BackupManagementType=="IaaSVM"
        | distinct BackupItemUniqueId, BackupItemFriendlyName
    )
    on BackupItemUniqueId
    ````

- Tüm başarılı SQL günlük yedekleme işleri

    ````Kusto
    AddonAzureBackupJobs
    | where JobOperation=="Backup" and JobOperationSubType=="Log"
    | where JobStatus=="Completed"
    | join kind=inner
    (
        CoreAzureBackup
        | where OperationName == "BackupItem"
        | where BackupItemType=="SQLDataBase" and BackupManagementType=="AzureWorkload"
        | distinct BackupItemUniqueId, BackupItemFriendlyName
    )
    on BackupItemUniqueId
    ````

- Tüm başarılı Azure Yedekleme aracı işleri

    ````Kusto
    AddonAzureBackupJobs
    | where JobOperation=="Backup"
    | where JobStatus=="Completed"
    | join kind=inner
    (
        CoreAzureBackup
        | where OperationName == "BackupItem"
        | where BackupItemType=="FileFolder" and BackupManagementType=="MAB"
        | distinct BackupItemUniqueId, BackupItemFriendlyName
    )
    on BackupItemUniqueId
    ````

- Yedekleme Maddesi Başına Tüketilen Yedek Depolama

    ````Kusto
    CoreAzureBackup
    //Get all Backup Items
    | where OperationName == "BackupItem"
    //Get distinct Backup Items
    | distinct BackupItemUniqueId, BackupItemFriendlyName
    | join kind=leftouter
    (AddonAzureBackupStorage
    | where OperationName == "StorageAssociation"
    //Get latest record for each Backup Item
    | summarize arg_max(TimeGenerated, *) by BackupItemUniqueId 
    | project BackupItemUniqueId , StorageConsumedInMBs)
    on BackupItemUniqueId
    | project BackupItemUniqueId , BackupItemFriendlyName , StorageConsumedInMBs 
    | sort by StorageConsumedInMBs desc
    ````


### <a name="diagnostic-data-update-frequency"></a>Tanısal veri güncelleştirme sıklığı

Kasadaki tanılama verileri Log Analytics çalışma alanına biraz gecikmeyle pompalanır. Her etkinlik, Recovery Services kasasından itildikten *20 ila 30 dakika* sonra Log Analytics çalışma alanına gelir. Burada gecikme hakkında daha fazla bilgi vardır:

- Tüm çözümlerde, yedekleme hizmetinin yerleşik uyarıları oluşturuldukları anda itilir. Bu nedenle genellikle Log Analytics çalışma alanında 20 ila 30 dakika sonra görünürler.
- Tüm çözümler de, isteğe bağlı yedekleme işleri ve geri yükleme işleri *bitirir bitirmez*itilir.
- SQL yedeklemesi dışındaki tüm çözümler için, zamanlanmış yedekleme işleri *biter bitmez*itilir.
- SQL yedeklemesi için, günlük yedeklemeleri her 15 dakikada bir gerçekleşebildiği için, günlükler de dahil olmak üzere tamamlanan tüm zamanlanmış yedekleme işlerinin bilgileri toplu olarak doldurularak her 6 saatte bir itilir.
- Tüm çözümlerde, yedekleme öğesi, ilke, kurtarma noktaları, depolama ve benzeri diğer bilgiler günde en az *bir kez itilir.*
- Yedekleme yapılandırmasındaki bir değişiklik (ilke değiştirme veya düzenleme ilkesi gibi) ilgili tüm yedekleme bilgilerinin itme sini tetikler.

## <a name="using-the-recovery-services-vaults-activity-logs"></a>Kurtarma Hizmetleri kasasının etkinlik günlüklerini kullanma

> [!CAUTION]
> Aşağıdaki adımlar yalnızca *Azure VM yedeklemeleri* için geçerlidir. Bu adımları Azure Yedekleme aracısı, Azure içindeki SQL yedeklemeleri veya Azure Dosyaları gibi çözümler için kullanamazsınız.

Yedekleme başarısı gibi olaylar için bildirim almak için etkinlik günlüklerini de kullanabilirsiniz. Başlamak için aşağıdaki adımları izleyin:

1. Azure portalda oturum açın.
1. İlgili Kurtarma Hizmetleri kasasını açın.
1. Kasanın özelliklerinde **Etkinlik günlüğü** bölümünü açın.

Uygun günlüğü tanımlamak ve bir uyarı oluşturmak için:

1. Aşağıdaki resimde gösterilen filtreleri uygulayarak başarılı yedeklemeler için etkinlik günlükleri aldığınızı doğrulayın. Kayıtları görüntülemek için **Zaman Alanı** değerini gerektiği gibi değiştirin.

   ![Azure VM yedeklemeleri için etkinlik günlüklerini bulmak için filtreleme](media/backup-azure-monitoring-laworkspace/activitylogs-azurebackup-vmbackups.png)

1. İlgili ayrıntıları görmek için işlem adını seçin.
1. **Oluştur kuralı** sayfasını açmak için Yeni **uyarı kuralını** seçin.
1. Azure Monitor'u kullanarak [etkinlik günlüğü uyarılarını oluştur, görüntüle ve yönet'teki](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log)adımları izleyerek bir uyarı oluşturun.

   ![Yeni uyarı kuralı](media/backup-azure-monitoring-laworkspace/new-alert-rule.png)

Burada kaynak Kurtarma Hizmetleri kasasının kendisidir. Etkinlik günlükleri aracılığıyla bilgilendirilmek istediğiniz tüm kasalar için aynı adımları yineleyin. Bu uyarı olayları temel aldığı için durum eşik, nokta veya sıklık olmaz. İlgili etkinlik günlüğü oluşturulur oluşturulmaz, uyarı yükseltilir.

## <a name="using-log-analytics-to-monitor-at-scale"></a>Ölçekte izlemek için Log Analytics'i kullanma

Azure Monitor'da etkinlik günlükleri ve Günlük Analizi çalışma alanlarından oluşturulan tüm uyarıları görüntüleyebilirsiniz. Soldaki **Uyarılar** bölmesini açın.

Bildirimleri etkinlik günlükleri aracılığıyla alabiliyor olsanız da, ölçekte izleme için etkinlik günlükleri yerine Log Analytics'i kullanmanızı şiddetle öneririz. Nedeni:

- **Sınırlı senaryolar**: Etkinlik günlükleri aracılığıyla yapılan bildirimler yalnızca Azure VM yedeklemeleri için geçerlidir. Bildirimler her Kurtarma Hizmetleri kasası için ayarlanmalıdır.
- **Tanım sığdırma**: Zamanlanan yedekleme etkinliği, etkinlik günlüklerinin en son tanımına uymuyor. Bunun yerine, [kaynak günlükleri](https://docs.microsoft.com/azure/azure-monitor/platform/resource-logs-collect-workspace#what-you-can-do-with-platform-logs-in-a-workspace)ile hizalar. Bu hizalama, etkinlik günlüğü kanalı nda akan veriler değiştiğinde beklenmeyen etkilere neden olur.
- **Etkinlik günlüğü kanalındaki sorunlar**: Kurtarma Hizmetleri kasalarında, Azure Yedekleme'den pompalanan etkinlik günlükleri yeni bir modeli izler. Ne yazık ki, bu değişiklik Azure Kamu, Azure Almanya ve Azure China 21Vianet'teki etkinlik günlüklerinin oluşumunu etkiler. Bu bulut hizmetlerinin kullanıcıları Azure Monitor'daki etkinlik günlüklerinden herhangi bir uyarı oluşturur veya yapılandırırsa, uyarılar tetiklenmez. Ayrıca, tüm Azure ortak bölgelerinde, bir kullanıcı [Kurtarma Hizmetleri etkinliği bir Log Analytics çalışma alanında oturum açar toplarsa,](https://docs.microsoft.com/azure/azure-monitor/platform/collect-activity-logs)bu günlükler görünmez.

Azure Yedekleme tarafından korunan tüm iş yükleriniz için ölçekte izleme ve uyarı için Günlük Analizi çalışma alanı kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Özel sorgular oluşturmak [için, Günlük Analizi veri modeline](backup-azure-reports-data-model.md)bakın.
