---
title: Azure Site Recovery ile Azure VM çoğaltmasıyla ilgili sorunları giderme
description: Azure Site Recovery ile Azure VM olağanüstü durum kurtarma 'da çoğaltma sorunlarını giderme
author: sideeksh
manager: rochakm
ms.topic: troubleshooting
ms.date: 04/03/2020
ms.openlocfilehash: c27bf9a29bdb6e75e10fcafc597f40a88f995461
ms.sourcegitcommit: 1f48ad3c83467a6ffac4e23093ef288fea592eb5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2020
ms.locfileid: "84196084"
---
# <a name="troubleshoot-replication-in-azure-vm-disaster-recovery"></a>Azure VM olağanüstü durum kurtarma 'da çoğaltma sorunlarını giderme

Bu makalede, Azure sanal makinelerini (VM) bir bölgeden başka bir bölgeye çoğaltırken ve kurtarırken Azure Site Recovery karşılaşılan yaygın sorunlar açıklanmaktadır. Ayrıca, yaygın sorunların nasıl giderileceği açıklanmaktadır. Desteklenen konfigürasyonlar hakkında daha fazla bilgi için bkz. [Azure VM 'leri çoğaltmak için destek matrisi](site-recovery-support-matrix-azure-to-azure.md).

Azure Site Recovery, verileri düzenli olarak kaynak bölgesinden olağanüstü durum kurtarma bölgesine çoğaltır. Ayrıca, 5 dakikada bir çökme ile tutarlı bir kurtarma noktası oluşturur. Site Recovery 60 dakika boyunca kurtarma noktaları oluşturamıyorum, şu bilgilerle sizi uyarır:

```plaintext
Error message: "No crash consistent recovery point available for the VM in the last 60 minutes."

Error ID: 153007
```

Aşağıdaki bölümlerde nedenler ve çözümler açıklanır.

## <a name="high-data-change-rate-on-the-source-virtual-machine"></a>Kaynak sanal makinede yüksek veri değişim oranı

Kaynak sanal makinedeki veri değişim oranı desteklenen limitlerden fazlaysa Azure Site Recovery bir olay oluşturur. Sorunun yüksek dalgalanma göre olup olmadığını görmek için **çoğaltılan öğeler**  >  **VM**  >  **olayları-Son 72 saat**öğesine gidin.
Olay **veri değişikliği oranını desteklenen limitlerin ötesinde**görmeniz gerekir:

:::image type="content" source="./media/site-recovery-azure-to-azure-troubleshoot/data_change_event.png" alt-text="Çok yüksek veri değişim hızını gösteren Azure Site Recovery sayfası.":::

Olayı seçerseniz, tam disk bilgilerini görmeniz gerekir:

:::image type="content" source="./media/site-recovery-azure-to-azure-troubleshoot/data_change_event2.png" alt-text="Veri değişim oranı olay ayrıntılarını gösteren sayfa.":::

### <a name="azure-site-recovery-limits"></a>Azure Site Recovery limitleri

Aşağıdaki tablo, Azure Site Recovery sınırlarını sağlar. Bu sınırlar, testlerimize dayalıdır, ancak olası tüm uygulama giriş çıkışı (g/ç) kombinasyonlarını kapsayamazlar. Gerçek sonuçlar, uygulamanızın G/Ç karışımına göre değişebilir.

Göz önünde bulundurulması gereken iki sınır vardır: disk başına veri değişim ve sanal makine başına veri değişim. Bir örnek için aşağıdaki tablodaki Premium P20 diskine göz atalım. Tek bir VM için Site Recovery, en fazla beş disk ile disk başına 5 MB/sn 'lik değişim işleyebilir. Site Recovery, VM başına toplam karmaşıklık 54 MB/s sınırına sahiptir.

**Çoğaltma depolama hedefi** | **Kaynak disk için Ortalama g/ç boyutu** |**Kaynak disk için Ortalama veri karmaşası** | **Kaynak veri diski için günlük toplam veri değişim sıklığı**
---|---|---|---
Standart depolama | 8 KB    | 2 MB/sn | Disk başına 168 GB
Premium P10 veya P15 disk | 8 KB    | 2 MB/sn | Disk başına 168 GB
Premium P10 veya P15 disk | 16 KB | 4 MB/sn |    Disk başına 336 GB
Premium P10 veya P15 disk | 32 KB veya daha büyük | 8 MB/sn | Disk başına 672 GB
Premium P20 veya P30 veya P40 veya P50 disk | 8 KB    | 5 MB/sn | Disk başına 421 GB
Premium P20 veya P30 veya P40 veya P50 disk | 16 KB veya daha büyük |20 MB/sn | disk başına 1684 GB

### <a name="solution"></a>Çözüm

Azure Site Recovery, disk türüne bağlı olarak veri değişikliği ücretleri üzerinde sınırlara sahiptir. Bu sorunun yinelenen veya geçici olup olmadığını görmek için etkilenen sanal makinenin veri değişikliği oranını bulun. Kaynak sanal makineye gidin, **izleme**bölümündeki ölçümleri bulun ve bu ekran görüntüsünde gösterildiği gibi ölçümleri ekleyin:

:::image type="content" source="./media/site-recovery-azure-to-azure-troubleshoot/churn.png" alt-text="Veri değişikliği oranını bulmak için üç adımlı süreci gösteren sayfa.":::

1. **Ölçüm Ekle**' yi seçin ve **Işletim sistemi diski yazma bayt/sn** ve **veri diski yazma bayt/sn**ekleyin.
1. Ekran görüntüsünde gösterildiği gibi ani artış izleyin.
1. İşletim sistemi disklerinde oluşan toplam yazma işlemlerini ve tüm veri disklerini görüntüleyin. Bu ölçümler size disk başına düzeyde bilgi veremeyebilir, ancak toplam veri değişim modelini gösterir.

Veri değişikliği hızının bir ani artış, zaman zaman veri bloğu aracılığıyla gelebilir. Veri değişim hızı 10 MB/sn 'den (Premium için) veya 2 MB/sn 'den büyükse (Standart için) ve daha sonra geliyorsa çoğaltma gerçekleştirilir. Karmaşıklık, desteklenen sınırın ötesinde sürekli olarak iyi olursa, şu seçeneklerden birini göz önünde bulundurun:

- Yüksek veri değişikliği hızına neden olan diski hariç tutun: Ilk olarak çoğaltmayı devre dışı bırakın. Daha sonra, [PowerShell](azure-to-azure-exclude-disks.md)kullanarak diski dışlayabilirsiniz.
- Olağanüstü durum kurtarma depolama diskinin katmanını değiştirme: Bu seçenek yalnızca disk verileri karmaşıklığı 20 MB/sn 'den az olduğunda mümkündür. Örneğin, P10 diskine sahip bir VM, 8 MB/sn 'den büyük, ancak 10 MB/sn 'tan küçük bir veri dalgalanmasına sahiptir. Müşteri, koruma sırasında hedef depolama için bir P30 disk kullanıyorsa, sorun çözülebilir. Bu çözüm yalnızca Premium tarafından yönetilen diskleri kullanan makineler için mümkündür. Şu adımları uygulayın:

  1. Etkilenen çoğaltılan makinenin **disklerine** gidin ve çoğaltma diski adını kopyalayın.
  1. Yönetilen diskin bu çoğaltmasına gidin.
  1. **Genel bakışta** BIR SAS URL 'si oluşturulduğunu belirten bir başlık görebilirsiniz. Bu başlık ' ı seçin ve dışarı aktarmayı iptal edin. Başlığı görmüyorsanız bu adımı yoksayın.
  1. SAS URL 'SI iptal edildiğinde, yönetilen disk **yapılandırması** ' na gidin. Site Recovery, kaynak diskte gözlemlenen dalgalanma oranını desteklemesi için boyutunu artırın.

## <a name="network-connectivity-problems"></a>Ağ bağlantısı sorunları

### <a name="network-latency-to-a-cache-storage-account"></a>Önbellek depolama hesabına yönelik ağ gecikmesi

Site Recovery, çoğaltılan verileri önbellek depolama hesabına gönderir. Verileri bir sanal makineden önbellek depolama hesabına yüklemek 3 saniye içinde 4 MB 'den yavaşsa ağ gecikmesi yaşayabilirsiniz.

Gecikmeyle ilgili bir sorun olup olmadığını denetlemek için [AzCopy](/azure/storage/common/storage-use-azcopy)kullanın. Sanal makineden önbellek depolama hesabına veri yüklemek için bu komut satırı yardımcı programını kullanabilirsiniz. Gecikme yüksekse, VM 'lerden giden ağ trafiğini denetlemek için bir ağ sanal gereci (NVA) kullanıp kullanmayacağınızı kontrol edin. Tüm çoğaltma trafiği NVA üzerinden geçerse gereç azalmasıyla karşılaşabilirsiniz.

Çoğaltma trafiğinin NVA 'ya gitmemesi için, sanal ağınızda "depolama" için bir ağ hizmeti uç noktası oluşturmanız önerilir. Daha fazla bilgi için bkz. [ağ sanal gereç yapılandırması](azure-to-azure-about-networking.md#network-virtual-appliance-configuration).

### <a name="network-connectivity"></a>Ağ bağlantısı

Site Recovery çoğaltmanın çalışması için, VM 'nin belirli URL 'Lere veya IP aralıklarına giden bağlantı sağlaması gerekir. VM 'niz bir güvenlik duvarının arkasında olabilir veya giden bağlantıyı denetlemek için ağ güvenlik grubu (NSG) kurallarını kullanabilirsiniz. Bu durumda sorunlarla karşılaşabilirsiniz. Tüm URL 'Lerin bağlı olduğundan emin olmak için, bkz. [URL 'ler Için giden bağlantı](azure-to-azure-about-networking.md#outbound-connectivity-for-urls).

## <a name="error-id-153006---no-app-consistent-recovery-point-available-for-the-vm-in-the-past-x-minutes"></a>Hata KIMLIĞI 153006-VM için son "X" dakika içinde uygulamayla tutarlı bir kurtarma noktası yok

En yaygın sorunlardan bazıları aşağıda verilmiştir.

### <a name="known-issue-in-sql-server-20082008-r2"></a>SQL Server 2008/2008 R2 'de bilinen sorun

**Nasıl düzeltilir:** SQL Server 2008/2008 R2 ile ilgili bilinen bir sorun var. [Azure Site Recovery Aracısı veya bileşen olmayan DIĞER VSS yedeklemesi SQL Server 2008 R2 barındıran bir sunucu için başarısız oldu](https://support.microsoft.com/help/4504103/non-component-vss-backup-fails-for-server-hosting-sql-server-2008-r2)makalesine başvurun.

### <a name="azure-site-recovery-jobs-fail-on-servers-hosting-any-version-of-sql-server-instances-with-auto_close-dbs"></a>Azure Site Recovery işleri AUTO_CLOSE DBs ile SQL Server örneklerinin herhangi bir sürümünü barındıran sunucularda başarısız oluyor

**Nasıl düzeltilir:** [Auto_close DBs ile SQL Server örnekleri barındıran sunucularda Azure Site Recovery işleri başarısız gibi bileşen olmayan VSS yedeklemelerine](https://support.microsoft.com/help/4504104/non-component-vss-backups-such-as-azure-site-recovery-jobs-fail-on-ser)bakın.

### <a name="known-issue-in-sql-server-2016-and-2017"></a>SQL Server 2016 ve 2017'de bilinen sorun

**Nasıl düzeltilir**: [SQL Server 2017 için toplu güncelleştirme 16](https://support.microsoft.com/help/4508218/cumulative-update-16-for-sql-server-2017)makalesine başvurun.

### <a name="youre-using-azure-storage-spaces-direct-configuration"></a>Azure Depolama Alanları Doğrudan yapılandırması kullanıyorsunuz

**Nasıl düzeltilir**: Azure Site Recovery depolama alanları doğrudan yapılandırma için uygulamayla tutarlı kurtarma noktası oluşturamıyor. [Çoğaltma Ilkesini yapılandırın](azure-to-azure-how-to-enable-replication-s2d-vms.md).

### <a name="app-consistency-not-enabled-on-linux-servers"></a>Uygulama tutarlılığı Linux sunucularında etkinleştirilmemiş

**Nasıl düzeltilir** : Linux işlem sistemi için Azure Site Recovery, uygulama tutarlılığı için uygulama özel komut dosyalarını destekler. Ön ve gönderi seçenekleriyle özel betik, uygulama tutarlılığı için Azure Site Recovery Mobility Aracısı tarafından kullanılır. Etkinleştirme adımları [aşağıda](https://docs.microsoft.com/azure/site-recovery/site-recovery-faq#replication) verilmiştir.

### <a name="more-causes-because-of-vss-related-issues"></a>VSS ile ilgili sorunlar nedeniyle daha fazla neden:

Daha fazla sorun gidermek için, hata kodunu tam olarak almak için kaynak makinedeki dosyaları kontrol edin:

`C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\Application Data\ApplicationPolicyLogs\vacp.log`

Hataları bulmak için _, boş dize_ **dizesini bir**metin düzenleyicisinde açın.

```plaintext
Ex: vacpError:220#Following disks are in FilteringStopped state [\\.\PHYSICALDRIVE1=5, ]#220|^|224#FAILED: CheckWriterStatus().#2147754994|^|226#FAILED to revoke tags.FAILED: CheckWriterStatus().#2147754994|^|
```

Yukarıdaki örnekte **2147754994** , bu cümleyi takip eden hatayı belirten hata kodudur.

#### <a name="vss-writer-is-not-installed---error-2147221164"></a>VSS yazıcısı yüklü değil-hata 2147221164

**Nasıl düzeltilir**: uygulama tutarlılığı etiketi oluşturmak için, Azure Site Recovery bırım gölge KOPYASı HIZMETI (VSS) kullanır. Site Recovery, uygulama tutarlılığı anlık görüntülerini alma işlemi için bir VSS sağlayıcısı yüklüyor. Azure Site Recovery, bu VSS sağlayıcısını bir hizmet olarak yüklüyor. VSS sağlayıcısı yüklü değilse, uygulama tutarlılığı anlık görüntüsü oluşturma işlemi başarısız olur. Bir **hata kimliği 0x80040154 sınıfının kayıtlı değil**olduğunu gösterir. [VSS yazıcı yükleme sorunlarını giderme](vmware-azure-troubleshoot-push-install.md#vss-installation-failures)makalesine bakın.

#### <a name="vss-writer-is-disabled---error-2147943458"></a>VSS yazıcı devre dışı-hata 2147943458

**Nasıl düzeltilir**: uygulama tutarlılığı etiketi oluşturmak IÇIN Azure Site Recovery VSS kullanır. Site Recovery, uygulama tutarlılığı anlık görüntülerini alma işlemi için bir VSS sağlayıcısı yüklüyor. Bu VSS sağlayıcısı bir hizmet olarak yüklendi. VSS sağlayıcısı hizmeti etkin değilse, uygulama tutarlılığı anlık görüntüsü oluşturma işlemi başarısız olur. Hatayı gösterir: **belirtilen hizmet devre dışı bırakıldı ve başlatılamıyor (0x80070422)**.

VSS devre dışıysa:

- VSS sağlayıcı hizmetinin başlangıç türünün **Otomatik**olarak ayarlandığını doğrulayın.
- Aşağıdaki hizmetleri yeniden başlatın:
  - VSS hizmeti.
  - VSS sağlayıcısı Azure Site Recovery.
  - VDS hizmeti.

#### <a name="vss-provider-not_registered---error-2147754756"></a>VSS sağlayıcısı NOT_REGISTERED-hata 2147754756

**Nasıl düzeltilir**: uygulama tutarlılığı etiketi oluşturmak IÇIN Azure Site Recovery VSS kullanır. Azure Site Recovery VSS sağlayıcısı hizmeti 'nin yüklü olup olmadığını denetleyin.

VSS sağlayıcısı 'nı yeniden yüklemek için aşağıdaki komutları kullanın:

1. Mevcut sağlayıcıyı kaldır:

   `"C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\InMageVSSProvider_Uninstall.cmd"`

1. VSS sağlayıcısını yeniden yükle:

   `"C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\InMageVSSProvider_Install.cmd"`

VSS sağlayıcı hizmetinin başlangıç türünün **Otomatik**olarak ayarlandığını doğrulayın.

Aşağıdaki hizmetleri yeniden başlatın:

- VSS hizmeti.
- VSS sağlayıcısı Azure Site Recovery.
- VDS hizmeti.
