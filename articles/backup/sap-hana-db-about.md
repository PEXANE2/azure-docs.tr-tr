---
title: Azure VM 'lerinde SAP HANA veritabanı yedeklemesi hakkında
description: Bu makalede, Azure sanal makinelerinde çalışan SAP HANA veritabanlarını yedekleme hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 12/11/2019
ms.openlocfilehash: 52c235c95cea73a0c51c62fcb55f7f711d2eff21
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "79476466"
---
# <a name="about-sap-hana-database-backup-in-azure-vms"></a>Azure VM 'lerinde SAP HANA veritabanı yedeklemesi hakkında

SAP HANA veritabanları, düşük kurtarma noktası hedefi (RPO) ve hızlı kurtarma süresi hedefi (RTO) gerektiren görev açısından kritik iş yükleridir. Artık, [Azure Backup](https://docs.microsoft.com/azure/backup/backup-overview)kullanarak [Azure vm 'lerinde çalışan SAP HANA veritabanlarını yedekleyebilirsiniz](https://docs.microsoft.com/azure/backup/tutorial-backup-sap-hana-db) .

Azure Backup, SAP tarafından, SAP HANA 'ın yerel API 'Lerinden yararlanarak yerel yedekleme desteği sağlamak için [biriktirme tarafından sertifikalandırilmiştir](https://www.sap.com/dmc/exp/2013_09_adpd/enEN/#/d/solutions?id=8f3fd455-a2d7-4086-aa28-51d8870acaa5) . Azure Backup bu teklif, **sıfır altyapı** yedeklemelerinin Azure Backup mantarafıyla hizalanır, yedekleme altyapısını dağıtma ve yönetme ihtiyacını ortadan kaldırır. Artık Azure VM 'lerinde çalışan SAP HANA veritabanlarını sorunsuz bir şekilde yedekleyebilir ve geri yükleyebilirsiniz (artık[d serisi VM 'ler](../virtual-machines/m-series.md) de desteklenir!) ve Azure Backup sağladığı kurumsal yönetim özelliklerinden yararlanabilirsiniz.

## <a name="added-value"></a>Değer eklendi

SAP HANA veritabanlarını yedeklemek ve geri yüklemek için Azure Backup kullanarak aşağıdaki avantajları sağlar:

* **15 dakikalık kurtarma noktası hedefi (RPO)**: en fazla 15 dakikalık kritik verilerin kurtarılması artık mümkündür.
* **Tek tıklamayla, bir noktadan sonra geri yüklemeler**: üretim VERILERININ alternatif Hana sunucularına geri yüklenmesi kolay hale getirilir. Yedeklemenin ve katalogların yedeklenme zincirlerinin yedeklenme işlemi, Azure tarafından arka planda yönetilir.
* **Uzun süreli saklama**: sıkı uyumluluk ve denetim ihtiyaçları için. Kurtarma noktalarının yerleşik yaşam döngüsü yönetimi özelliği tarafından otomatik olarak ayıklanacağı saklama süresine bağlı olarak, yedeklemelerinizi yıllarca koruyun.
* **Azure 'Dan yedekleme yönetimi**: gelişmiş yönetim deneyimi için Azure Backup yönetim ve izleme yeteneklerini kullanın. Azure CLı de desteklenir.

Günümüzde destekdiğimiz yedekleme ve geri yükleme senaryolarını görüntülemek için [SAP HANA senaryo desteği matrisine](https://docs.microsoft.com/azure/backup/sap-hana-backup-support-matrix#scenario-support)bakın.

## <a name="backup-architecture"></a>Backup mimarisi

![Yedekleme mimarisi diyagramı](./media/sap-hana-db-about/backup-architecture.png)

* Yedekleme işlemi, Azure 'da [bir kurtarma hizmetleri Kasası oluşturarak](https://docs.microsoft.com/azure/backup/tutorial-backup-sap-hana-db#create-a-recovery-service-vault) başlar. Bu kasa, zaman içinde oluşturulan yedeklemeleri ve kurtarma noktalarını depolamak için kullanılacaktır.
* SAP HANA Server çalıştıran Azure VM kasayla kaydedilir ve yedeklenecek veritabanları [bulunur.](https://docs.microsoft.com/azure/backup/tutorial-backup-sap-hana-db#discover-the-databases) Azure Backup hizmetinin veritabanlarını bulmasına olanak tanımak için, bir [preregistration betiğinin](https://aka.ms/scriptforpermsonhana) , bir kök kullanıcı olarak Hana sunucusunda çalıştırılması gerekir.
* Bu betik, **AZUREWLBACKUPHANAUSER** DB kullanıcısı ve **hdbuserstore**'da aynı ada sahip karşılık gelen bir anahtar oluşturur. Betiğin ne yaptığını hakkında daha fazla bilgi edinmek için [ön kayıt betiğinin ne olduğunu](tutorial-backup-sap-hana-db.md#what-the-pre-registration-script-does) öğrenin bölümüne bakın.
* Azure Backup hizmeti artık, kayıtlı SAP HANA sunucusuna **Hana için Azure Backup eklentisini** yüklüyor.
* Preregistration betiği tarafından oluşturulan **AZUREWLBACKUPHANAUSER** DB kullanıcısı, tüm yedekleme ve geri yükleme işlemlerini GERÇEKLEŞTIRMEK üzere **Hana için Azure Backup eklentisi** tarafından kullanılır. Bu betiği çalıştırmadan SAP HANA DBs için yedeklemeyi yapılandırmaya çalışırsanız, şu hatayı alabilirsiniz: **Usererrorhanascriptnotrun**.
* Bulunan veritabanlarında [yedeklemeyi yapılandırmak](https://docs.microsoft.com/azure/backup/tutorial-backup-sap-hana-db#configure-backup) için gerekli yedekleme ilkesini seçin ve yedeklemeleri etkinleştirin.

* Yedekleme yapılandırıldıktan sonra Azure Backup hizmeti, korunan SAP HANA sunucusundaki VERITABANı düzeyinde aşağıdaki Backınt parametrelerini ayarlar:
  * [catalog_backup_using_backint: true]
  * [enable_accumulated_catalog_backup: false]
  * [parallel_data_backup_backint_channels: 1]
  * [log_backup_timeout_s: 900)]
  * [backint_response_timeout: 7200]

>[!NOTE]
>Bu parametrelerin konak düzeyinde mevcut *olmadığından* emin olun. Ana bilgisayar düzeyindeki Parametreler bu parametreleri geçersiz kılacak ve beklenmeyen davranışlara neden olabilir.
>

* **Hana için Azure Backup eklentisi** tüm yedekleme zamanlamalarını ve ilke ayrıntılarını saklar. Zamanlanan yedeklemeleri tetikler ve Backınt API 'Leri aracılığıyla **Hana yedekleme altyapısıyla** iletişim kurar.
* **Hana yedekleme altyapısı** , yedeklenecek verilerle birlikte bir backınt akışı döndürür.
* Tam veya fark olan tüm zamanlanmış yedeklemeler ve isteğe bağlı yedeklemeler (Azure portal tetiklenir), **Hana için Azure Backup eklentisi**tarafından başlatılır. Ancak, günlük yedeklemeleri **Hana yedekleme altyapısının** kendisi tarafından yönetilir ve tetiklenir.
* SAP HANA için Azure Backup, bir BackInt sertifikalı çözüm olması, temel diske veya VM türlerine bağlı değildir. Yedekleme, HANA tarafından oluşturulan akışlar tarafından gerçekleştirilir.

## <a name="using-azure-vm-backup-with-azure-sap-hana-backup"></a>Azure SAP HANA yedekleme ile Azure VM yedeklemesi kullanma

Azure 'da veritabanı düzeyinde yedekleme ve kurtarma sağlayan SAP HANA yedeklemesini kullanmanın yanı sıra, işletim sistemi ve veritabanı olmayan diskleri yedeklemek için Azure VM yedekleme çözümünü kullanabilirsiniz.

[Backınt sertifikalı Azure SAP HANA yedekleme çözümü](#backup-architecture) veritabanı yedekleme ve kurtarma için kullanılabilir.

[Azure VM yedeklemesi](backup-azure-vms-introduction.md) , işletim sistemini ve diğer veritabanı olmayan diskleri yedeklemek için kullanılabilir. VM yedeklemesi her gün bir kez alınır ve tüm diskleri ( **yazma hızlandırıcısı (WA) diskleri** ve **UltraDisks**hariç) yedekler. Veritabanı Azure SAP HANA yedekleme çözümü kullanılarak yedeklendiğinden, şu anda önizleme aşamasında olan diski hariç tut özelliğini kullanarak yalnızca işletim sistemi ve veritabanı olmayan disklerin dosya ile tutarlı bir yedeğini alabilirsiniz.

>[!NOTE]
> Azure VM yedekleme ile post öncesi betikleri kullanılması, veritabanının veri birimlerinin uygulamayla tutarlı yedeklemelere izin verir. Ancak, günlük alanı WA disklerinde bulunuyorsa, bu disklerin anlık görüntüsünü almak günlük alanı tutarlılığını garanti edemeyebilir. HANA, bu tam neden için günlük yedeklemeleri oluşturmanın açık bir yoludur. SAP HANA aynı şekilde etkinleştirin ve Azure SAP HANA yedekleme kullanılarak yedeklenebilir.

SAP HANA çalıştıran bir VM 'yi geri yüklemek için şu adımları izleyin:

* [Azure VM yedeğinden yeni BIR VM](backup-azure-arm-restore-vms.md) 'yi en son kurtarma noktasından geri yükleyin. Veya yeni bir boş VM oluşturun ve diskleri en son kurtarma noktasından bağlayın.
* WA diskleri yedeklenmeden bu yana geri yüklenmez. Boş WA diskleri ve günlük alanı oluşturun.
* Diğer tüm yapılandırmaların (IP, sistem adı, vb.) ayarlanabilmesi için VM, Azure Backup 'tan DB verisi alacak şekilde ayarlanır.
* Şimdi VERITABANıNı [Azure SAP HANA DB YEDEĞINDEN](sap-hana-db-restore.md#restore-to-a-point-in-time-or-to-a-recovery-point) sanal makineye istediğiniz zaman noktasına geri yükleyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure VM 'de çalışan bir SAP HANA veritabanını nasıl geri](https://docs.microsoft.com/azure/backup/sap-hana-db-restore) yükleyeceğinizi öğrenin
* [Kullanarak yedeklenen SAP HANA veritabanlarını yönetmeyi öğrenin Azure Backup](https://docs.microsoft.com/azure/backup/sap-hana-db-manage)
