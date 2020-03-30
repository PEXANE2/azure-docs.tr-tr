---
title: SSS - Azure VM'lerde SQL Server veritabanlarını yedekleme
description: Azure Yedekleme ile Azure VM'lerde SQL Server veritabanlarını yedekleme yle ilgili sık sorulan soruların yanıtlarını bulun.
ms.reviewer: vijayts
ms.topic: conceptual
ms.date: 04/23/2019
ms.openlocfilehash: a973761bf16e2d271d718e4a8b29e08624276987
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79247715"
---
# <a name="faq-about-sql-server-databases-that-are-running-on-an-azure-vm-backup"></a>Azure VM yedeklemesi üzerinde çalışan SQL Server veritabanları hakkında SSS

Bu makalede, Azure sanal makinelerde (VM) çalışan ve [Azure Yedekleme](backup-overview.md) hizmetini kullanan SQL Server veritabanlarını yedekleme yle ilgili sık sorulan soruları yanıtlar.

## <a name="can-i-use-azure-backup-for-iaas-vm-as-well-as-sql-server-on-the-same-machine"></a>IaaS VM ve SQL Server için Azure yedeklemesini aynı makinede kullanabilir miyim?

Evet, aynı VM'de hem VM yedeklemehem de SQL yedeklemesi olabilir. Bu durumda, günlükleri burdamamak için VM'de yalnızca kopyalı tam yedeklemeyi dahili olarak tetikleriz.

## <a name="does-the-solution-retry-or-auto-heal-the-backups"></a>Çözüm yedekleri yeniden dener veya otomatik olarak iyileştirir mi?

Bazı durumlarda Azure Yedekleme hizmeti düzeltici yedeklemeleri tetikler. Otomatik iyileşme aşağıda belirtilen altı koşuldan herhangi biri için gerçekleşebilir:

- LSN Doğrulama Hatası nedeniyle günlük veya diferansiyel yedekleme başarısız olursa, sonraki günlük veya diferansiyel yedekleme bunun yerine tam yedeklemeye dönüştürülür.
- Günlük veya diferansiyel yedeklemeden önce tam yedekleme gerçekleşmemişse, bu günlük veya diferansiyel yedekleme bunun yerine tam yedeklemeye dönüştürülür.
- En son tam yedeklemenin giriş süresi 15 günden daha eskiyse, bir sonraki günlük veya diferansiyel yedekleme bunun yerine tam yedeklemeye dönüştürülür.
- Bir uzantı yükseltmesi nedeniyle iptal edilen tüm yedekleme işleri, yükseltme tamamlandıktan ve uzantı başlatıldıktan sonra yeniden tetiklenir.
- Geri Yükleme sırasında veritabanının üzerine yazmayı seçerseniz, bir sonraki günlük/diferansiyel yedekleme başarısız olur ve bunun yerine tam bir yedekleme tetiklenir.
- Veritabanı kurtarma modelindeki değişiklik nedeniyle günlük zincirlerini sıfırlamak için tam yedekleme nin gerekli olduğu durumlarda, tam bir sonraki zamanlamada otomatik olarak tetiklenir.

Bir yetenek olarak otomatik iyileşme varsayılan olarak tüm kullanıcı için etkinleştirilir; Ancak, bu durumu devre dışı bırakmayı seçerseniz, aşağıdakileri gerçekleştirin:

- SQL Server örneğinde, *C:\Program Files\Azure İş Yükü Yedekleme\bin* **klasöründe, ExtensionSettingsOverrides.json** dosyasını oluşturun veya düzenleme.
- **ExtensionSettingsOverrides.json'da** *{"EnableAutoHealer": false}* ayarlayın.
- Değişikliklerinizi kaydedin ve dosyayı kapatın.
- SQL Server örneğinde **Görev Yönet'i** açın ve **ardından AzureWLBackupCoordinatorSvc** hizmetini yeniden başlatın.

## <a name="can-i-control-how-many-concurrent-backups-run-on-the-sql-server"></a>SQL sunucusunda kaç eşzamanlı yedeklemenin çalıştırıldığını denetleyebilir miyim?

Evet. Bir SQL Server örneğindeki etkiyi en aza indirmek için yedekleme ilkesinin çalıştığı hızı azaltabilirsiniz. Ayarı değiştirmek için:

1. SQL Server örneğinde, *C:\Program Files\Azure İş Yükü Yedekleme\bin* *klasöründe, ExtensionSettingsOverrides.json* dosyasını oluşturun.
2. *ExtensionSettingsOverrides.json* **dosyasında, VarsayılanBackupTasksThreshold** ayarını daha düşük bir değere (örneğin, 5) değiştirin. <br>
  `{"DefaultBackupTasksThreshold": 5}`
<br>
VarsayılanBackupTasksThreshold varsayılan değeri **20'dir.**

3. Değişikliklerinizi kaydedin ve dosyayı kapatın.
4. SQL Server örneğinde **Görev Yöneticisi'ni**açın. **AzureWLBackupCoordinatorSvc** hizmetini yeniden başlatın.<br/> <br/>
 Bu yöntem, yedekleme uygulaması büyük miktarda kaynak tüketiyorsa yardımcı olsa da, SQL Server [Resource Governor](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor?view=sql-server-2017) gelen uygulama isteklerinin kullanabileceği CPU, fiziksel IO ve bellek miktarına sınırlamalar belirtmenin daha genel bir yoludur.

> [!NOTE]
> UX'de herhangi bir zamanda birçok yedekleme olarak devam edebilir ve zamanlayabilirsiniz, ancak yukarıdaki örneğe göre 5'in sürgülü penceresinde işlenirler.

## <a name="can-i-run-a-full-backup-from-a-secondary-replica"></a>İkincil bir yinelemeden tam yedekleme çalıştırabilir miyim?

SQL sınırlamalarına göre, İkincil Yineleme'de Yalnızca Tam Yedekleme kopyala yı çalıştırabilirsiniz; ancak Tam yedeklemeye izin verilmez.

## <a name="can-i-protect-availability-groups-on-premises"></a>Şirket içinde kullanılabilirlik gruplarını koruyabilir miyim?

Hayır. Azure Yedekleme, Azure'da çalışan SQL Server veritabanlarını korur. Kullanılabilirlik grubu (AG) Azure ve şirket içi makineler arasında yayılıyorsa, AG yalnızca birincil yineleme Azure'da çalışıyorsa korunabilir. Ayrıca, Azure Yedekleme yalnızca Kurtarma Hizmetleri kasası ile aynı Azure bölgesinde çalışan düğümleri korur.

## <a name="can-i-protect-availability-groups-across-regions"></a>Kullanılabilirlik gruplarını bölgeler arasında koruyabilir miyim?

Azure Yedekleme Kurtarma Hizmetleri kasası, kasayla aynı bölgede bulunan tüm düğümleri algılayabilir ve koruyabilir. SQL Server Always On kullanılabilirlik grubunuz birden çok Azure bölgesine yayılıyorsa, birincil düğüme sahip bölgeden yedeklemeyi ayarlayın. Azure Yedekleme, yedekleme tercihinize göre kullanılabilirlik grubundaki tüm veritabanlarını algılayabilir ve koruyabilir. Yedekleme tercihiniz karşılanmadığında, yedeklemeler başarısız olur ve hata uyarısı alırsınız.

## <a name="do-successful-backup-jobs-create-alerts"></a>Başarılı yedekleme işleri sonucunda uyarı oluşturulur mu?

Hayır. Başarılı yedekleme işleri uyarı oluşturmaz. Uyarılar yalnızca başarısız olan yedekleme işleri için gönderilir. Portal uyarıları için ayrıntılı davranış [burada](backup-azure-monitoring-built-in-monitor.md)belgelenmiştir. Ancak, başarılı işler için bile uyarılar varsa, ilginizi çekebilir, [Azure Monitor'u kullanarak İzleme'yi](backup-azure-monitoring-use-azuremonitor.md)kullanabilirsiniz.

## <a name="can-i-see-scheduled-backup-jobs-in-the-backup-jobs-menu"></a>Yedek İşler menüsünde zamanlanmış yedekleme işleri görebilir miyim?

**Yedek İş** menüsü yalnızca isteğe bağlı yedekleme işleri gösterir. [Azure Monitör'ü kullanarak](backup-azure-monitoring-use-azuremonitor.md)zamanlanmış iş kullanımı İzleme için.

## <a name="are-future-databases-automatically-added-for-backup"></a>Gelecekteki veritabanları yedekleme için otomatik olarak eklenir mi?

Evet, [otomatik koruma](backup-sql-server-database-azure-vms.md#enable-auto-protection)ile bu özelliği elde edebilirsiniz.  

## <a name="if-i-delete-a-database-from-an-autoprotected-instance-what-will-happen-to-the-backups"></a>Otomatik korumalı bir örnekten bir veritabanını silersem, yedeklemelere ne olur?

Bir veritabanı otomatik korumalı bir örnekten bırakılırsa, veritabanı yedeklemeleri yine de denenir. Bu, silinen veritabanının **Yedekleme Öğeleri** altında sağlıksız olarak görünmeye başladığı ve hala korunduğu anlamına gelir.

Bu veritabanını korumayı durdurmanın en doğru yolu, bu veritabanındaki **verileri silerek** **Yedeklemeyi Durdur'u** yapmaktır.  

## <a name="if-i-do-stop-backup-operation-of-an-autoprotected-database-what-will-be-its-behavior"></a>Otomatik korumalı bir veritabanının yedekleme işlemini durdurursam davranışı ne olacak?

Yedeklemeyi **tutma verileriyle durdurursanız,** gelecekteki yedeklemeler gerçekleşmez ve varolan kurtarma noktaları bozulmadan kalır. Veritabanı yine de korumalı olarak kabul edilecek ve **Yedekleme öğeleri**altında gösterilecek.

**Silme verileriyle yedeklemeyi durdurursanız,** gelecekteki yedeklemeler gerçekleşmez ve varolan kurtarma noktaları da silinir. Veritabanı korumasız olarak kabul edilir ve Yapılaşı Yedekleme'deki örnek altında gösterilir. Ancak, el ile seçilebilen veya otomatik koruma alabilecek diğer yukarı korumalı veritabanlarının aksine, bu veritabanı soluk renkte görünür ve seçilemez. Bu veritabanını yeniden korumanın tek yolu, örnekte otomatik korumayı devre dışı betmektir. Artık bu veritabanını seçebilir ve üzerinde koruma yapılandırabilir veya örnekte otomatik korumayı yeniden etkinleştirebilirsiniz.

## <a name="if-i-change-the-name-of-the-database-after-it-has-been-protected-what-will-be-the-behavior"></a>Veritabanı nın adını korumaaltına alındıktan sonra değiştirirsem, davranış nedir?

Yeniden adlandırılmış bir veritabanı yeni bir veritabanı olarak kabul edilir. Bu nedenle, hizmet veritabanı bulunamadı ve başarısız yedekleri ile bu durumu ele alacaktır.

Şimdi yeniden adlandırılmış olan veritabanını seçebilir ve üzerinde koruma yapılandırabilirsiniz. Örnekte otomatik korumanın etkinleştirilmesi durumunda, yeniden adlandırılmış veritabanı otomatik olarak algılanır ve korunur.

## <a name="why-cant-i-see-an-added-database-for-an-autoprotected-instance"></a>Otomatik korumalı bir örnek için ekbir veritabanını neden göremiyorum?

[Otomatik korumalı bir örneğe eklediğiniz bir](backup-sql-server-database-azure-vms.md#enable-auto-protection) veritabanı, korumalı öğelerin hemen altında görünmeyebilir. Bunun nedeni, keşfin genellikle her 8 saatte bir çalıştırıştur. Ancak, aşağıdaki resimde gösterildiği gibi, **DB'leri Yeniden Keşfet'i**seçerek bir keşfi el ile çalıştırırsanız, yeni veritabanlarını hemen keşfedebilir ve koruyabilirsiniz:

  ![Yeni eklenen bir veritabanını el ile keşfedin](./media/backup-azure-sql-database/view-newly-added-database.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure VM'de çalışan [bir SQL Server veritabanını](backup-azure-sql-database.md) nasıl yedekleyeceğimiz öğrenin.
