---
title: Kurtarma Hizmetleri kasaları için tanılama ayarlarını kullanma
description: Azure Backup için eski ve yeni tanılama olaylarının nasıl kullanılacağını açıklayan bir makale
ms.topic: conceptual
ms.date: 10/30/2019
ms.openlocfilehash: e3919d120e5f741af6cd30dd27e5a1dfa2b06cf2
ms.sourcegitcommit: 05a650752e9346b9836fe3ba275181369bd94cf0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2020
ms.locfileid: "79136948"
---
# <a name="using-diagnostics-settings-for-recovery-services-vaults"></a>Kurtarma Hizmetleri Kasaları için Tanılama Olaylarını kullanma

Azure Backup, analiz, uyarı ve raporlama amacıyla toplanabilecek ve kullanılabilecek tanılama olayları gönderir. 

Bir kurtarma hizmetleri Kasası için tanılama ayarlarını, kasaya gidip **Tanılama ayarları** menü öğesine tıklayarak Azure Portal aracılığıyla yapılandırabilirsiniz. **+ Tanılama ayarı Ekle** ' ye tıkladığınızda bir depolama hesabına, Olay Hub 'ına veya log ANALYTICS (La) çalışma alanına bir veya daha fazla tanılama olayı gönderebilirsiniz.

![Tanılama ayarları dikey penceresi](./media/backup-azure-diagnostics-events/diagnostics-settings-blade.png)

## <a name="diagnostics-events-available-for-azure-backup-users"></a>Azure Backup kullanıcılar için kullanılabilen tanılama olayları

Azure Backup, her biri belirli bir yedekleme ile ilgili yapıtlar kümesi üzerinde ayrıntılı veriler sağlayan aşağıdaki tanılama olaylarını sağlar:

* CoreAzureBackup
* AddonAzureBackupAlerts
* AddonAzureBackupProtectedInstance
* AddonAzureBackupJobs
* AddonAzureBackupPolicy
* AddonAzureBackupStorage

[Azure Backup tanılama olayları için veri modeli](https://docs.microsoft.com/azure/backup/backup-azure-reports-data-model)

Bu olaylara yönelik veriler bir depolama hesabına, LA alanına veya bir olay hub 'ına gönderilebilir. Bu verileri bir LA çalışma alanına gönderiyorsanız, **Tanılama ayarı** ekranında **kaynağa özgü** geçişi seçmeniz gerekir (aşağıdaki bölümlerde daha fazla bilgi görüntüleyin).

## <a name="using-diagnostics-settings-with-log-analytics-la"></a>Tanılama ayarlarını Log Analytics kullanma (LA)

Azure Log Analytics yol haritası ile hizalama, Azure Backup Artık kasa tanılama verilerini yedekleme için adanmış LA tablolarına göndermenize olanak tanır. Bunlara [kaynağa özgü tablolar](https://docs.microsoft.com/azure/azure-monitor/platform/resource-logs-collect-workspace#resource-specific)adı verilir.

Kasa tanılama verilerinizi LA 'ya göndermek için:

1.  Kasanıza gidin ve **Tanılama ayarları**' na tıklayın. **+ Tanılama ayarı Ekle**' ye tıklayın.
2.  Tanılama ayarına bir ad verin.
3.  **Log Analytics gönder** kutusunu işaretleyin ve bir Log Analytics çalışma alanı seçin.
4.  Geçiş sırasında **kaynak** ' ı seçin ve aşağıdaki altı olayı- **Coreazurebackup**, **AddonAzureBackupAlerts**, **AddonAzureBackupProtectedInstance**, **AddonAzureBackupJobs**, **AddonAzureBackupPolicy**ve **AddonAzureBackupStorage**denetleyin.
5.  **Kaydet**'e tıklayın.

![Kaynağa özgü mod](./media/backup-azure-diagnostics-events/resource-specific-blade.png)

Veriler LA çalışma alanına akar, bu olayların her biri için adanmış tablolar çalışma alanınızda oluşturulur. Bu tablolardan herhangi birini doğrudan sorgulayabilir ve gerekirse bu tablolar arasında birleşimler veya birleşimler da yapabilirsiniz.

> [!IMPORTANT]
> Yukarıdaki altı olay, yani CoreAzureBackup, AddonAzureBackupAlerts, AddonAzureBackupProtectedInstance, AddonAzureBackupJobs, AddonAzureBackupPolicy ve AddonAzureBackupStorage **yalnızca** kaynağa özgü modda desteklenir. **Bu altı olayla ilgili verileri Azure Tanılama modunda göndermeye çalışırsanız, hiçbir veri, LA çalışma alanına akamaz.**

## <a name="legacy-event"></a>Eski olay

Geleneksel olarak, bir kasaya yönelik yedeklemeyle ilgili tüm Tanılama verileri ' AzureBackupReport ' adlı tek bir olayda yer alır. Yukarıda açıklanan altı olay, AzureBackupReport ' deki tüm verilerin bir ayrışmasıdır. 

Şu anda, kullanıcıların bu olay üzerinde var olan özel sorgulara sahip olduğu durumlarda (örneğin, özel günlük uyarıları, özel görselleştirmeler vb.), geriye doğru uyumluluk için AzureBackupReport olayını desteklemeye devam ediyoruz. Bununla birlikte, verileri günlük sorgularında daha kolay bir şekilde daha kolay hale getiren, şemaları ve bunların yapısını daha iyi keşfedilmesini sağlayan **Yeni olaylara mümkün olduğunca erken geçmeyi öneririz**. bu sayede, hem alma gecikmesi hem de sorgu sürelerinde performans artar. **Azure tanılama modunun kullanımı Için destek sonunda zaman aşımına uğrar ve bu nedenle yeni olayların seçilmesi, daha sonraki bir tarihte karmaşık geçişlere engel olmanıza yardımcı olabilir**.

Belirli bir kapsamdaki tüm kasalarınız için 6 yeni olayla yeni bir tanılama ayarı eklemek için Azure Backup yerleşik ilkesini kullanın: [kasa tanılama ayarlarını ölçeklendirerek yapılandırma](https://docs.microsoft.com/azure/backup/azure-policy-configure-diagnostics)

Yeni tablolardaki verileri kullanmak üzere tüm özel sorgularınızı geçirene kadar, AzureBackupReport için ayrı Tanılama ayarları ve altı yeni olay oluşturmayı tercih edebilirsiniz. Aşağıdaki görüntüde iki tanılama ayarı olan bir kasaya örnek gösterilmektedir. **Setting1** adlı ilk ayar, AzureBackupReport olayının verilerini AzureDiagnostics MODUNDAKI bir La çalışma alanına gönderir. **Setting2** adlı ikinci ayar, altı yeni Azure Backup olayının verilerini kaynağa özgü modda bir La çalışma alanına gönderir.

![İki ayar](./media/backup-azure-diagnostics-events/two-settings-example.png)

> [!IMPORTANT]
> AzureBackupReport olayı **yalnızca** Azure tanılama modunda desteklenir. **Bu olay için verileri kaynağa özgü modda göndermeye çalışırsanız, hiçbir veri, LA çalışma alanına akamaz.**

> [!NOTE]
> Azure Tanılama/kaynağa özgü geçiş yalnızca Kullanıcı **Log Analytics gönder**' i denetlediğinde görünür. Bir depolama hesabına veya bir olay hub 'ına veri göndermek için, bir Kullanıcı yalnızca gerekli hedefi seçebilir ve herhangi bir ek giriş yapmadan istenen olayları denetleyebilir. Yine, eski Event AzureBackupReport, ileri ' yi seçmemelidir.

## <a name="users-sending-azure-site-recovery-events-to-log-analytics-la"></a>Log Analytics Azure Site Recovery olayları gönderen kullanıcılar (LA)

Azure Backup ve Azure Site Recovery olaylar aynı kurtarma hizmetleri kasasından gönderilir. Azure Site Recovery Şu anda kaynağa özgü tablolar üzerinde eklendi, LA 'ya Azure Site Recovery olayları göndermek isteyen kullanıcılar **yalnızca** Azure tanılama moduna yönlendirilir (aşağıdaki resme bakın). **Azure Site Recovery olayları Için kaynağa özgü mod seçilmesi, gerekli VERILERIN La çalışma alanına gönderilmesini engeller**.

![Site Recovery olaylar](./media/backup-azure-diagnostics-events/site-recovery-settings.png)

Yukarıdaki nusileri özetlemek için:

* Azure Tanılama ile birlikte ayarlanmış ve üzerinde özel sorgular varsa, yeni etkinliklerden veri kullanmak için sorgularınızı geçirene kadar bu ayarı **değiştirmeden**tutun.
* Yeni tablolara (önerildiği gibi) eklemek istiyorsanız, **Yeni** bir tanılama ayarı oluşturun, **kaynağa özel** ' i seçin ve yukarıda belirtilen altı yeni olayı seçin.
* Şu anda LA 'ya Azure Site Recovery olayları gönderiyorsanız, bu olaylar için kaynağa özgü **mod ' u seçmeyin** , aksi takdirde bu olaylara yönelik VERILER, La çalışma alanınıza akamaz. Bunun yerine, **ek bir tanılama ayarı**oluşturun, **Azure tanılama**' yi seçin ve ilgili Azure Site Recovery olaylarını seçin.

Aşağıdaki görüntüde, bir kasa için üç tanılama ayarı olan bir kullanıcının örneği gösterilmektedir. **Setting1** adlı ilk ayar, AzureBackupReport olayından verileri AzureDiagnostics MODUNDAKI bir La çalışma alanına gönderir. **Setting2** adlı ikinci ayar, altı yeni Azure Backup olaydan kaynağa özgü MODDAKI bir La çalışma alanına veri gönderir. **Setting3**adlı üçüncü ayar, Azure Site Recovery olaylarından Azure tanılama MODUNDAKI bir La çalışma alanına veri gönderir.

![Üç ayar](./media/backup-azure-diagnostics-events/three-settings-example.png)

## <a name="next-steps"></a>Sonraki adımlar

[Tanılama olayları için Log Analytics veri modelini öğrenin](https://docs.microsoft.com/azure/backup/backup-azure-reports-data-model)
