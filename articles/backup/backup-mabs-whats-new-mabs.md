---
title: Microsoft Azure Backup Server 'daki yenilikler
description: Microsoft Azure Backup sunucusu, VM 'Leri, dosyaları ve klasörleri, iş yüklerini ve daha fazlasını korumak için gelişmiş yedekleme olanakları sağlar.
ms.topic: conceptual
ms.date: 05/24/2020
ms.openlocfilehash: 5f8d0aa83f6d54575b76847efa892864b32c456d
ms.sourcegitcommit: d7fba095266e2fb5ad8776bffe97921a57832e23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84629083"
---
# <a name="whats-new-in-microsoft-azure-backup-server-mabs"></a>Microsoft Azure Backup Server 'daki yenilikler (MABS)

## <a name="whats-new-in-mabs-v3-ur1"></a>MABS v3 UR1 'deki yenilikler

Microsoft Azure Backup Server (MABS) sürüm 3 UR1, en son güncelleştirmedir ve kritik hata düzeltmelerini ve diğer özellikleri ve geliştirmeleri içerir. Düzeltilen hataların listesini ve MABS v3 UR1 yükleme yönergelerini görüntülemek için bkz. KB makalesi [4534062](https://support.microsoft.com/help/4534062).

>[!NOTE]
>32 bit koruma aracısına yönelik destek MABS v3 UR1 ile kullanım dışıdır. Bkz. [32 bit koruma aracısının kullanımdan kaldırılması](#32-bit-protection-agent-deprecation).

### <a name="faster-backups-with-tiered-storage-using-ssds"></a>SSD 'Ler kullanılarak katmanlı depolama ile daha hızlı yedeklemeler

MABS v2 [modern yedekleme alanı](backup-mabs-add-storage.md) (MB) tanıtılan, depolama kullanımını ve performansı geliştirir. MB 'ler, temel dosya sistemi olarak ReFS kullanır ve katmanlı depolama gibi karma depolamayı kullanmak için tasarlanmıştır.

MB ile ölçek ve performans elde etmek için, MABS v3 UR1 ile birlikte, bir katmanlı birim olarak (genel depolama alanının %4 ' i), DPM HDD depolaması ile birlikte bir katmanlı birim olarak kullanmanızı öneririz. Katmanlı depolama ile MABS v3 UR1,% 50-70 daha hızlı yedeklemeler sağlar. Katmanlı depolamayı yapılandırma adımları için [katmanlı depolamada MB olarak ayarlanan](https://docs.microsoft.com/system-center/dpm/add-storage?view=sc-dpm-2019#set-up-mbs-with-tiered-storage) DPM makalesine başvurun.

### <a name="support-for-refs-volumes-and-refs-volumes-with-deduplication-enabled"></a>Yinelenenleri kaldırma özelliği etkinken ReFS birimleri ve ReFS birimleri için destek

MABS v3 UR1 ile ReFS hacminde dağıtılan ReFS birimlerini ve iş yüklerini yedekleyebilirsiniz. ReFS birimlerinde dağıtılan aşağıdaki iş yüklerini yedekleyebilirsiniz:

* İşletim sistemi (64 bit): Windows Server 2019, 2016, 2012 R2, 2012.
* SQL Server: SQL Server 2019, SQL Server 2017, 2016.
* Exchange: Exchange 2019, 2016.
* SharePoint: SharePoint 2019, 2016 ve en son SP.

>[!NOTE]
> ReFS biriminde depolanan Hyper-V VM 'lerinin yedeklenmesi MABS v3 ile desteklenir

### <a name="azure-vmware-solution-protection-support"></a>Azure VMware Çözüm koruması desteği

MABS v3 UR1 ile artık [Azure VMware çözümünde](https://docs.microsoft.com/azure/azure-vmware/)dağıtılan sanal makineleri koruyabilirsiniz.

### <a name="vmware-parallel-backups"></a>VMware paralel yedeklemeleri

MABS v3 UR1 ile tek bir koruma grubundaki tüm VMware VM yedeklemeleriniz paralel olacak ve %25 daha hızlı VM yedeklemesine neden olur.
MABS 'nin önceki sürümlerinde, paralel yedeklemeler yalnızca koruma grupları arasında gerçekleştirildi. MABS v3 UR1 ile VMware Delta çoğaltma işleri paralel olarak çalışır. Varsayılan olarak, paralel olarak çalıştırılacak işlerin sayısı 8 ' e ayarlanır. [VMware paralel yedeklemeleri](backup-azure-backup-server-vmware.md#vmware-parallel-backups)hakkında daha fazla bilgi edinin.

### <a name="disk-exclusion-for-vmware-vm-backup"></a>VMware VM yedeklemesi için disk dışlama

MABS v3 UR1 ile belirli diskleri bir VMware VM yedeğinden dışlayabilirsiniz. [VMware VM yedeğinden disk dışlama](backup-azure-backup-server-vmware.md#exclude-disk-from-vmware-vm-backup)hakkında daha fazla bilgi edinin.  

### <a name="support-for-additional-layer-of-authentication-to-delete-online-backup"></a>Çevrimiçi yedeklemeyi silmek için ek kimlik doğrulama katmanı desteği

MABS v3 UR1 ile kritik işlemler için ek bir kimlik doğrulama katmanı eklenmiştir. **Verileri silme işlemleri Ile korumayı durdur** işlemini gerçekleştirdiğinizde bir güvenlik PIN kodu girmeniz istenir.

### <a name="offline-backup-improvements"></a>Çevrimdışı yedekleme geliştirmeleri

MABS v3 UR1, Azure Içeri/dışarı aktarma hizmeti ile çevrimdışı yedekleme deneyimini geliştirir. Daha fazla bilgi için [buradaki](https://docs.microsoft.com/azure/backup/backup-azure-backup-server-import-export)güncelleştirilmiş adımlara bakın.

>[!NOTE]
>Güncelleştirme ayrıca MABS 'teki Azure Data Box kullanarak çevrimdışı yedekleme için önizleme sağlar. [SystemCenterFeedback@microsoft.com](mailto:SystemCenterFeedback@microsoft.com)Daha fazla bilgi edinmek için iletişim kurun.

### <a name="new-cmdlet-parameter"></a>Yeni cmdlet parametresi

MABS v3 UR1 yeni bir **[-checkreplicaparçalanma]** parametresi içerir. Yeni parametre bir çoğaltmanın parçalanma yüzdesini hesaplar ve **Copy-DPMDatasourceReplica** cmdlet 'ine dahil edilir.

### <a name="32-bit-protection-agent-deprecation"></a>32-bit koruma aracısının kullanımdan kaldırılması

MABS v3 UR1 ile 32 bit koruma Aracısı desteği artık desteklenmemektedir. MABS v3 sunucusunu UR1 'e yükselttikten sonra 32 bit iş yüklerini koruyamazsınız. Mevcut 32-bit koruma aracılarının hiçbiri devre dışı durumda olur ve zamanlanan yedeklemeler **Aracı devre dışı** bırakılır hatasıyla başarısız olur. Bu aracıların yedekleme verilerini sürdürmek istiyorsanız, verileri sakla seçeneği ile korumayı durdurabilirsiniz. Aksi takdirde, koruma Aracısı kaldırılabilir.

>[!NOTE]
>MABS UR 1 ile koruma için desteklenen iş yüklerini öğrenmek üzere [güncelleştirilmiş koruma matrisini](https://docs.microsoft.com/azure/backup/backup-mabs-protection-matrix) gözden geçirin.

## <a name="whats-new-in-mabs-v3-rtm"></a>MABS v3 RTM 'deki yenilikler

Microsoft Azure Backup Server sürüm 3 (MABS v3), kritik hata düzeltmeleri, Windows Server 2019 desteği, SQL 2017 desteği ve diğer özellikleri ve geliştirmeleri içerir. Düzeltilen hataların listesini ve MABS v3 için yükleme yönergelerini görüntülemek için bkz. KB makalesi [4457852](https://support.microsoft.com/help/4457852/microsoft-azure-backup-server-v3).

MABS v3 'de aşağıdaki özellikler bulunur:

### <a name="volume-to-volume-migration"></a>Birim geçişi hacmi

MABS v2 'de Modern Yedekleme Alanı (MB) ile, belirli iş yüklerini depolama özelliklerine göre belirli bir depolama alanına yedeklenecek şekilde yapılandırdığınız Iş yükü algılayan depolama duyurduk. Ancak, yapılandırmadan sonra, belirli veri kaynaklarının yedeklerini iyileştirilmiş kaynak kullanımı için başka bir depolama alanına taşımanız gerektiğini görebilirsiniz. MABS v3, yedeklemelerinizi geçirme ve bunları [üç adımda](https://techcommunity.microsoft.com/t5/system-center-blog/sc-2016-dpm-ur4-migrate-backup-storage-in-3-simple-steps/ba-p/351842)farklı bir birime depolanacak şekilde yapılandırma olanakları sağlar.

### <a name="prevent-unexpected-data-loss"></a>Beklenmeyen veri kaybını önleme

Kurumlar ' de, MABS bir Yöneticiler ekibi tarafından yönetilir. Depolama üzerinde yedeklemeler için kullanılması gereken yönergeler olsa da, yedek depolama olarak MABS 'e verilen yanlış bir birim kritik verilerin kaybedilmesine neden olabilir. MABS v3 ile, [Bu birimleri bu PowerShell cmdlet 'lerini](https://docs.microsoft.com/azure/backup/backup-mabs-add-storage)kullanarak depolama için kullanılamayan şekilde yapılandırarak bu senaryolara engel olabilirsiniz.

### <a name="custom-size-allocation"></a>Özel boyut ayırma

Modern Yedekleme Alanı (MB), gerektiğinde depolama ölçülü kaynak kullanır. Bunu yapmak için, MABS, koruma için yapılandırıldığında yedeklenen verilerin boyutunu hesaplar. Ancak, bir dosya sunucusu durumunda olduğu gibi birçok dosya ve klasör birlikte yedekleniyorsa, boyut hesaplaması uzun zaman alabilir. MABS v3 ile, MABS 'yi, zaman tasarrufu sağlayan her bir dosyanın boyutunu hesaplamak yerine varsayılan olarak birim boyutunu kabul edecek şekilde yapılandırabilirsiniz.

### <a name="optimized-cc-for-rct-vms"></a>RCT VM 'Leri için iyileştirilmiş CC

MABS, sanal makine Kilitlenmelerinde senaryolarda zaman alan tutarlılık denetimleri gereksinimini azaltan RCT (Hyper-V ' d a yerel değişiklik izleme) kullanır. RCT, VSS anlık görüntü tabanlı yedeklemelerin sağladığı değişiklik izlemeden daha dayanıklıdır. MABS v3, herhangi bir tutarlılık denetimi sırasında yalnızca değiştirilen verileri aktararak ağ ve depolama tüketimini iyileştirir.

### <a name="support-to-tls-12"></a>TLS 1,2 desteği

TLS 1,2, Microsoft tarafından en iyi sınıf şifrelemesi ile önerilen iletişimin güvenli bir yoludur. MABS artık MABS ve korunan sunucular arasındaki TLS 1,2 iletişimini, sertifika tabanlı kimlik doğrulaması ve bulut yedeklemeleri için desteklemektedir.

### <a name="vmware-vm-protection-support"></a>VMware VM koruması desteği

VMware VM yedeklemesi artık üretim dağıtımları için desteklenmektedir. MABS v3, VMware VM koruması için aşağıdakileri sunar:

* VCenter ve ESXi 6,5 desteği ve 5,5 ve 6,0 desteğiyle birlikte.
* VMware VM 'lerinin buluta otomatik koruması. Korumalı bir klasöre yeni VMware VM 'Leri eklenirse, bunlar otomatik olarak disk ve buluta korunur.
* VMware alternatif konum kurtarma için kurtarma verimliliği iyileştirmeleri.

### <a name="sql-2017-support"></a>SQL 2017 desteği

MABS v3, MABS veritabanı olarak SQL 2017 ile yüklenebilir. SQL Server 'ı SQL 2016 ' den SQL 2017 ' den yükseltebilir veya onu bir daha önce yeniden yükleyebilirsiniz. Ayrıca, SQL 2017 iş yükünü hem kümelenmiş hem de kümelenmemiş ortamda MABS v3 ile yedekleyebilirsiniz.

### <a name="windows-server-2019-support"></a>Windows Server 2019 desteği

MABS v3, Windows Server 2019 ' ye yüklenebilir. MABS v3 'i WS2019 ile birlikte kullanmak için, MABS v3 'e yüklemeden/yükseltmeden önce işletim sistemini WS2019 'e yükseltebilir ya da işletim sistemi sonrası v3 'yi yükleme/yükseltme işlemini WS2016 üzerinde yükseltmeniz sağlayabilirsiniz.

MABS v3 tam bir sürümdür ve doğrudan Windows Server 2016, Windows Server 2019 üzerine yüklenebilir veya MABS v2 'den yükseltilebilir. Yedekleme sunucusu v3 sürümüne yükseltmeden veya yüklemeden önce, yükleme önkoşulları hakkında bilgi edinin.
[Burada](https://docs.microsoft.com/azure/backup/backup-azure-microsoft-azure-backup#software-package)mabs yükleme/yükseltme adımları hakkında daha fazla bilgi bulabilirsiniz.

> [!NOTE]
>
> MABS, System Center Data Protection Manager ile aynı kod tabanına sahiptir. MABS v3, Data Protection Manager 1807 ile eşdeğerdir. MABS v3 UR1, Data Protection Manager 2019 UR1 eşdeğerdir.

## <a name="next-steps"></a>Sonraki adımlar

Sunucunuzu nasıl hazırlayacağınızı veya bir iş yükünü korumaya nasıl başlayacağınızı öğrenin:

* [Yedekleme sunucusu iş yüklerini hazırlama](backup-azure-microsoft-azure-backup.md)
* [Bir VMware sunucusunu yedeklemek için yedekleme sunucusu kullanma](backup-azure-backup-server-vmware.md)
* [Yedekleme sunucusunu kullanarak yedekleme SQL Server](backup-azure-sql-mabs.md)
* [Yedekleme sunucusu ile Modern Yedekleme Alanı kullanma](backup-mabs-add-storage.md)
