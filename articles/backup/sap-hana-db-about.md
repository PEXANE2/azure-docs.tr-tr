---
title: Azure VM'lerde SAP HANA veritabanı yedeklemesi hakkında
description: Bu makalede, Azure sanal makinelerinde çalışan SAP HANA veritabanlarını yedekleme hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 12/11/2019
ms.openlocfilehash: 52c235c95cea73a0c51c62fcb55f7f711d2eff21
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79476466"
---
# <a name="about-sap-hana-database-backup-in-azure-vms"></a>Azure VM'lerde SAP HANA veritabanı yedeklemesi hakkında

SAP HANA veritabanları, düşük kurtarma noktası hedefi (RPO) ve hızlı kurtarma süresi hedefi (RTO) gerektiren kritik görev yükleridir. Azure [Yedekleme'yi](https://docs.microsoft.com/azure/backup/backup-overview)kullanarak [Azure VM'lerde çalışan SAP HANA veritabanlarını](https://docs.microsoft.com/azure/backup/tutorial-backup-sap-hana-db) artık yedekleyebilirsiniz.

Azure Yedekleme, SAP HANA'nın yerel API'lerinden yararlanarak yerel yedekleme desteği sağlamak için SAP tarafından [Backint sertifikasına](https://www.sap.com/dmc/exp/2013_09_adpd/enEN/#/d/solutions?id=8f3fd455-a2d7-4086-aa28-51d8870acaa5) işlenmiştir. Azure Yedekleme'nin bu teklifi, Azure Yedekleme'nin **sıfır altyapı** yedeklemeleri mantrasıyla aynı hizaya geliyor ve yedekleme altyapısını dağıtma ve yönetme ihtiyacını ortadan kaldırıyor. Artık Azure VM'lerde çalışan SAP HANA veritabanlarını[(M serisi VM'ler](../virtual-machines/m-series.md) de şimdi de desteklendi!) sorunsuz bir şekilde yedekleyebilir ve Azure Yedekleme'nin sağladığı kurumsal yönetim özelliklerinden yararlanabilirsiniz.

## <a name="added-value"></a>Katma değer

SAP HANA veritabanlarını yedeklemek ve geri yüklemek için Azure Yedekleme'yi kullanmak aşağıdaki avantajları sağlar:

* **15 dakikalık Kurtarma Noktası Hedefi (RPO)**: Kritik verilerin 15 dakikaya kadar kurtarılması artık mümkündür.
* **Tek tıklatma, zaman içinde geri yükleme:** Üretim verilerinin alternatif HANA sunucularına geri yüklenir. Geri yüklemeleri gerçekleştirmek için yedekleme ve katalog zincirleme tüm sahne arkasında Azure tarafından yönetilir.
* **Uzun süreli saklama**: Sıkı uyumluluk ve denetim ihtiyaçları için. Yedekleme noktalarının yerleşik yaşam döngüsü yönetimi yeteneği tarafından otomatik olarak budanacağı bekletme süresine bağlı olarak yedeklemelerinizi yıllarca saklar.
* **Azure'dan Yedekleme Yönetimi**: Gelişmiş yönetim deneyimi için Azure Yedekleme'nin yönetim ve izleme özelliklerini kullanın. Azure CLI de desteklenir.

Bugün desteklediğimiz yedekleme ve geri yükleme senaryolarını görüntülemek için [SAP HANA senaryo destek matrisine](https://docs.microsoft.com/azure/backup/sap-hana-backup-support-matrix#scenario-support)bakın.

## <a name="backup-architecture"></a>Backup mimarisi

![Yedek mimari diyagramı](./media/sap-hana-db-about/backup-architecture.png)

* Yedekleme işlemi, Azure'da [bir Kurtarma hizmetleri kasası oluşturarak](https://docs.microsoft.com/azure/backup/tutorial-backup-sap-hana-db#create-a-recovery-service-vault) başlar. Bu kasa, zaman içinde oluşturulan yedeklemeleri ve kurtarma noktalarını depolamak için kullanılacaktır.
* SAP HANA sunucusunu çalıştıran Azure VM kasasına kaydedilir ve yedeklenecek veritabanları [keşfedilir.](https://docs.microsoft.com/azure/backup/tutorial-backup-sap-hana-db#discover-the-databases) Azure Yedekleme hizmetinin veritabanlarını keşfetmesini sağlamak için, HANA sunucusunda kök kullanıcı olarak bir [ön kayıt komut dosyası](https://aka.ms/scriptforpermsonhana) çalıştırılması gerekir.
* Bu komut dosyası **AZUREWLBACKUPHANAUSER** DB kullanıcı ve **hdbuserstore**aynı ada sahip karşılık gelen bir anahtar oluşturur. Komut dosyasının ne yaptığı hakkında daha fazla bilgi almak için [ön kayıt komut dosyasının yaptıkları](tutorial-backup-sap-hana-db.md#what-the-pre-registration-script-does) bölümüne bakın.
* Azure Yedekleme Hizmeti artık **HANA için Azure Yedekleme Eklentisini** kayıtlı SAP HANA sunucusuna yüklüyor.
* Ön kayıt komut dosyası tarafından oluşturulan **AZUREWLBACKUPHANAUSER** DB kullanıcısı, **HANA için Azure Yedekleme Eklentisi** tarafından tüm yedekleme ve geri yükleme işlemlerini gerçekleştirmek için kullanılır. Bu komut dosyasını çalıştırmadan SAP HANA DB'leri için yedekleme yapılandırmaya çalışırsanız, aşağıdaki hatayı alabilirsiniz: **UserErrorHanaScriptNotRun**.
* Keşfedilen veritabanlarında [yedekleme yapılandırmak](https://docs.microsoft.com/azure/backup/tutorial-backup-sap-hana-db#configure-backup) için gerekli yedekleme ilkesini seçin ve yedeklemeleri etkinleştirin.

* Yedekleme yapılandırıldıktan sonra Azure Yedekleme hizmeti, korumalı SAP HANA sunucusunda VERITABANı düzeyinde aşağıdaki Backint parametrelerini ayarlar:
  * [catalog_backup_using_backint:true]
  * [enable_accumulated_catalog_backup:yanlış]
  * [parallel_data_backup_backint_channels:1]
  * [log_backup_timeout_s:900)]
  * [backint_response_timeout:7200]

>[!NOTE]
>Bu parametrelerin HOST düzeyinde *bulunmadığından* emin olun. Ana bilgisayar düzeyi parametreleri bu parametreleri geçersiz kılar ve beklenmeyen davranışlara neden olabilir.
>

* **HANA için Azure Yedekleme Eklentisi** tüm yedekleme zamanlamalarını ve ilke ayrıntılarını korur. Zamanlanmış yedeklemeleri tetikler ve Backint API'leri aracılığıyla **HANA Yedekleme Motoru** ile iletişim kurar.
* **HANA Yedekleme Motoru,** yedeklenecek verilerle birlikte bir Backint akışı döndürür.
* Tam veya diferansiyel olan tüm zamanlanmış yedeklemeler ve isteğe bağlı yedeklemeler (Azure portalından tetiklenir) **HANA için Azure Yedekleme Eklentisi**tarafından başlatılır. Ancak, günlük yedeklemeleri **HANA Yedekleme Motoru** tarafından yönetilir ve tetiklenir.
* BackInt sertifikalı bir çözüm olan SAP HANA için Azure Yedekleme, temel disk veya VM türlerine bağlı değildir. Yedekleme HANA tarafından oluşturulan akışlar tarafından gerçekleştirilir.

## <a name="using-azure-vm-backup-with-azure-sap-hana-backup"></a>Azure SAP HANA yedeklemesi ile Azure VM yedeklemesi kullanma

Veritabanı düzeyinde yedekleme ve kurtarma sağlayan Azure'daki SAP HANA yedeklemesini kullanmanın yanı sıra, işletim sistemi ve veritabanı olmayan diskleri yedeklemek için Azure VM yedekleme çözümlerini kullanabilirsiniz.

[Backint sertifikalı Azure SAP HANA yedekleme çözümü](#backup-architecture) veritabanı yedekleme ve kurtarma için kullanılabilir.

[Azure VM yedekleme,](backup-azure-vms-introduction.md) işletim sistemi ve diğer veritabanı olmayan diskleri yedeklemek için kullanılabilir. VM yedeklemesi her gün bir kez alınır ve tüm diskleri yedekler **(Yazma Hızlandırıcı (WA) diskleri** ve **UltraDisk'ler**hariç). Veritabanı Azure SAP HANA yedekleme çözümünün kullanılarak yedeklendiği için, şu anda önizlemede olan dışlama diski özelliğini kullanarak yalnızca işletim sistemi ve veritabanı olmayan disklerin dosya tutarlı yedeklemesini alabilirsiniz.

>[!NOTE]
> Azure VM yedeklemesi ile önceden posta komut dosyaları nın kullanılması, veritabanının veri birimlerinin uygulama tutarlı yedeklemelerine olanak tanır. Ancak, günlük alanı WA disklerinde yaşıyorsa, bu disklerin anlık görüntüsünü almak günlük alanı tutarlılığını garanti etmeyebilir. HANA bu nedenle günlük yedekleme oluşturma nın açık bir yolu vardır. SAP HANA'nızda da aynısını etkinleştirin ve Azure SAP HANA yedeklemesi kullanılarak yedeklenebilirler.

SAP HANA çalıştıran bir VM'yi geri yüklemek için aşağıdaki adımları izleyin:

* [Azure VM yedeklemesinden en](backup-azure-arm-restore-vms.md) son kurtarma noktasından yeni bir VM geri yükleyin. Veya yeni bir boş VM oluşturun ve diskleri en son kurtarma noktasından takın.
* WA diskleri yedeklenmediğinden, geri yüklenmez. Boş WA diskleri ve günlük alanı oluşturun.
* Diğer tüm yapılandırmalar (IP, sistem adı vb. gibi) ayarlandıktan sonra, VM Azure yedeklemesinden DB verileri alacak şekilde ayarlanır.
* Şimdi DB'yi Azure SAP [HANA DB yedeklemesinden](sap-hana-db-restore.md#restore-to-a-point-in-time-or-to-a-recovery-point) istenilen zaman noktaya kadar VM'ye geri yükleyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure VM'de çalışan bir SAP HANA veritabanını nasıl geri yükleyeceğimiz](https://docs.microsoft.com/azure/backup/sap-hana-db-restore) öğrenin
* [Azure Yedekleme'yi kullanarak yedeklenen SAP HANA veritabanlarını nasıl yöneteceğimize](https://docs.microsoft.com/azure/backup/sap-hana-db-manage) öğrenin
