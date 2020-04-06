---
title: Azure Site Kurtarma ile Azure VM'lerinin çoğaltılması yla sorun giderme
description: Azure Site Kurtarma ile Azure VM olağanüstü durum kurtarmada sorun giderme çoğaltma
author: sideeksh
manager: rochakm
ms.topic: troubleshooting
ms.date: 04/03/2020
ms.openlocfilehash: 8cba02d3c7d1e649853570b199b646b1c4dcce2d
ms.sourcegitcommit: 67addb783644bafce5713e3ed10b7599a1d5c151
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2020
ms.locfileid: "80667411"
---
# <a name="troubleshoot-replication-in-azure-vm-disaster-recovery"></a>Azure VM olağanüstü durum kurtarmada sorun giderme çoğaltma

Bu makalede, Azure sanal makinelerini (VM) bir bölgeden başka bir bölgeye çoğaltırken ve kurtarırken Azure Site Kurtarma'daki sık karşılaşılan sorunlar açıklanmaktadır. Ayrıca, sık karşılaşılan sorunları nasıl gidereceklerini de açıklar. Desteklenen yapılandırmalar hakkında daha fazla bilgi için [Azure VM'lerini çoğaltmak için destek matrisine](site-recovery-support-matrix-azure-to-azure.md)bakın.

Azure Site Kurtarma, kaynak bölgeden olağanüstü durum kurtarma bölgesine kadar verileri sürekli olarak çoğaltır. Ayrıca her 5 dakikada bir çökme tutarlı kurtarma noktası oluşturur. Site Kurtarma 60 dakika boyunca kurtarma noktaları oluşturamazsa, bu bilgilerle sizi uyarır:

```plaintext
Error message: "No crash consistent recovery point available for the VM in the last 60 minutes."

Error ID: 153007
```

Aşağıdaki bölümlerde nedenleri ve çözümleri açıklayınız.

## <a name="high-data-change-rate-on-the-source-virtual-machine"></a>Kaynak sanal makinede yüksek veri değişim oranı

Azure Site Kurtarma, kaynak sanal makinedeki veri değiştirme oranı desteklenen sınırlardan yüksekse bir olay oluşturur. Sorunun yüksek karmaşa dan dolayı olup olmadığını görmek için, **Çoğaltılmış öğeler** > **VM** > Events gidin **- son 72 saat**.
Desteklenen sınırların ötesinde olay **Veri değiştirme oranını**görmelisiniz:

:::image type="content" source="./media/site-recovery-azure-to-azure-troubleshoot/data_change_event.png" alt-text="Çok yüksek veri değiştirme oranını gösteren Azure Site Kurtarma sayfası.":::

Olayı seçerseniz, tam disk bilgilerini görmeniz gerekir:

:::image type="content" source="./media/site-recovery-azure-to-azure-troubleshoot/data_change_event2.png" alt-text="Veri değiştirme oranı olay ayrıntılarını gösteren sayfa.":::

### <a name="azure-site-recovery-limits"></a>Azure Site Recovery limitleri

Aşağıdaki tablo, Azure Site Recovery sınırlarını sağlar. Bu sınırlar testlerimize dayanır, ancak olası tüm uygulama giriş çıktısı (G/Ç) kombinasyonlarını kapsayamaz. Gerçek sonuçlar, uygulamanızın G/Ç karışımına göre değişebilir.

Göz önünde bulundurulması gereken iki sınır vardır: disk başına veri karmaşası ve sanal makine başına veri karmaşası. Örneğin aşağıdaki tablodaki Premium P20 diskine bakalım. Tek bir VM için, Site Kurtarma disk başına en fazla beş tür diskile 5 MB/schurn işleyebilir. Site Kurtarma VM başına toplam churn 25 MB / ssınırına sahiptir.

**Çoğaltma depolama hedefi** | **Kaynak disk için ortalama G/Ç boyutu** |**Kaynak disk için ortalama veri karmaşası** | **Kaynak veri diski için günde toplam veri karmaşası**
---|---|---|---
Standart depolama | 8 KB    | 2 MB/sn | Disk başına 168 GB
Premium P10 veya P15 disk | 8 KB    | 2 MB/sn | Disk başına 168 GB
Premium P10 veya P15 disk | 16 KB | 4 MB/sn |    Disk başına 336 GB
Premium P10 veya P15 disk | 32 KB veya daha büyük | 8 MB/sn | Disk başına 672 GB
Premium P20 veya P30 veya P40 veya P50 disk | 8 KB    | 5 MB/sn | Disk başına 421 GB
Premium P20 veya P30 veya P40 veya P50 disk | 16 KB veya daha büyük |20 MB/sN | Disk başına 1684 GB

### <a name="solution"></a>Çözüm

Azure Site Kurtarma disk türüne bağlı olarak veri değiştirme hızlarında sınırlamalar vardır. Bu sorunun yinelenen veya geçici olup olmadığını görmek için etkilenen sanal makinenin veri değiştirme oranını bulun. Kaynak sanal makineye gidin, **İzleme**altındaki ölçümleri bulun ve bu ekran görüntüsünde gösterildiği gibi ölçümleri ekleyin:

:::image type="content" source="./media/site-recovery-azure-to-azure-troubleshoot/churn.png" alt-text="Veri değiştirme oranını bulmak için üç adımlı işlemi gösteren sayfa.":::

1. **Metrik ekle'yi**seçin ve **İşletim Sistemi DiskI Yazıl Bayt/Sn** ve **Veri Diski Yazım Bayt/Sn**ekleyin.
1. Ekran görüntüsünde gösterildiği gibi başak izleyin.
1. İşletim sistemi diskleri ve tüm veri diskleri arasında gerçekleşen toplam yazma işlemlerini görüntüleyin. Bu ölçümler size disk başına düzeyde bilgi vermeyebilir, ancak toplam veri karmaşası deseni gösterir.

Veri değişim hızındaki ani artış, ara sıra bir veri patlamasından kaynaklanabilir. Veri değiştirme oranı 10 MB/s 'den (Premium için) veya 2 MB/sn'den (Standart için) büyükse ve aşağı iniyorsa, çoğaltma yetişecektir. Çalkalon sürekli olarak desteklenen sınırın çok üzerindeyse, aşağıdaki seçeneklerden birini göz önünde bulundurun:

- Yüksek veri değiştirme hızına neden olan diski hariç tut: İlk olarak, çoğaltmayı devre dışı bırak. Daha sonra [PowerShell'i](azure-to-azure-exclude-disks.md)kullanarak diski dışlayabilirsiniz.
- Olağanüstü durum kurtarma depolama diskinin katmanını değiştirin: Bu seçenek yalnızca disk veri karmaşası 20 MB/s'den azsa mümkündür. Örneğin, P10 diskli bir VM'de 8 MB/s'den büyük ancak 10 MB/s'den az veri karmaşası vardır. Müşteri koruma sırasında hedef depolama için bir P30 disk ikullanabiliyorsa, sorun çözülebilir. Bu çözüm yalnızca Premium Yönetilen Diskler kullanan makineler için mümkündür. Şu adımları uygulayın:

  1. Etkilenen yinelenen makinenin **Diskleri'ne** gidin ve çoğaltma disk adını kopyalayın.
  1. Yönetilen diskin bu yinelemesine gidin.
  1. **Genel Bakış'ta** bir SAS URL'si oluşturulduğunu belirten bir banner görebilirsiniz. Bu banner'ı seçin ve dışa aktarmayı iptal edin. Banner'ı görmüyorsanız bu adımı yoksayın.
  1. SAS URL'si iptal edilir etmez yönetilen disk için **Yapılandırma'ya** gidin. Site Kurtarma kaynak diskte gözlenen çalkalama hızını destekleyecek şekilde boyutunu artırın.

## <a name="network-connectivity-problems"></a>Ağ bağlantısı sorunları

### <a name="network-latency-to-a-cache-storage-account"></a>Önbellek depolama hesabına ağ gecikmesi

Site Kurtarma, çoğaltılan verileri önbellek depolama hesabına gönderir. Verileri sanal bir makineden önbellek depolama hesabına yüklemek 3 saniyede 4 MB'tan daha yavaşsa ağ gecikmesi yaşayabilirsiniz.

Gecikmeyle ilgili bir sorun olup olmadığını kontrol etmek için [AzCopy'i](/azure/storage/common/storage-use-azcopy)kullanın. Sanal makineden önbellek depolama hesabına veri yüklemek için bu komut satırı yardımcı programını kullanabilirsiniz. Gecikme oranı yüksekse, VM'lerden gelen giden ağ trafiğini denetlemek için bir ağ sanal cihazı (NVA) kullanıp kullanmadığınızı kontrol edin. Tüm çoğaltma trafiği NVA'dan geçerse cihaz daraltılmış olabilir.

Çoğaltma trafiğinin NVA'ya gitmemesi için sanal ağımızda "Depolama" için bir ağ hizmeti bitiş noktası oluşturmanızı öneririz. Daha fazla bilgi için [Bkz. Ağ sanal cihaz yapılandırması.](azure-to-azure-about-networking.md#network-virtual-appliance-configuration)

### <a name="network-connectivity"></a>Ağ bağlantısı

Site Kurtarma çoğaltma çalışması için, belirli URL'lere veya IP aralıklarına giden bağlantı sağlamak için VM gerekir. VM'nizi bir güvenlik duvarının arkasında olabilir veya giden bağlantıyı denetlemek için ağ güvenlik grubu (NSG) kurallarını kullanabilirsiniz. Bu ysa, sorunlarla karşılaşabilirsiniz. Tüm URL'lerin bağlı olduğundan emin olmak [için URL'ler için Giden bağlantıya](azure-to-azure-about-networking.md#outbound-connectivity-for-urls)bakın.

## <a name="error-id-153006---no-app-consistent-recovery-point-available-for-the-vm-in-the-past-x-minutes"></a>Hata Kimliği 153006 - Geçmiş "X" dakikalarında VM için uygulama tutarlı kurtarma noktası yok

En sık karşılaşılan sorunlardan bazıları şunlardır.

### <a name="known-issue-in-sql-server-20082008-r2"></a>SQL sunucu 2008/2008 R2 bilinen sorun

**Nasıl düzeltilir:** SQL server 2008/2008 R2 ile bilinen bir sorun var. Sql Server [2008 R2'ye ev sahipliği yapan bir sunucu için Azure Site Kurtarma Aracısı veya diğer bileşen olmayan VSS yedeklemesi](https://support.microsoft.com/help/4504103/non-component-vss-backup-fails-for-server-hosting-sql-server-2008-r2)makaleye bakın.

### <a name="azure-site-recovery-jobs-fail-on-servers-hosting-any-version-of-sql-server-instances-with-auto_close-dbs"></a>Azure Site Kurtarma işleri, AUTO_CLOSE DB'li SQL Server örneklerinin herhangi bir sürümünü barındıran sunucularda başarısız olur

**Nasıl düzeltilir:** Makaleye bakın [Azure Site Kurtarma işleri gibi bileşen dışı VSS yedeklemeleri, AUTO_CLOSE DB'leri olan SQL Server örneklerini barındıran sunucularda başarısız olur.](https://support.microsoft.com/help/4504104/non-component-vss-backups-such-as-azure-site-recovery-jobs-fail-on-ser)

### <a name="known-issue-in-sql-server-2016-and-2017"></a>SQL Server 2016 ve 2017'de bilinen sorun

**Nasıl düzeltilir**: [SQL Server 2017 için Kümülatif Güncelleştirme 16](https://support.microsoft.com/help/4508218/cumulative-update-16-for-sql-server-2017)makalesine bakın.

### <a name="youre-using-azure-storage-spaces-direct-configuration"></a>Azure Depolama Alanları Doğrudan Yapılandırma kullanıyorsunuz

**Nasıl düzeltilir**: Azure Site Kurtarma, Depolama Alanları Doğrudan Yapılandırması için uygulama tutarlı kurtarma noktası oluşturamaz. [Çoğaltma ilkesini yapılandırın.](azure-to-azure-how-to-enable-replication-s2d-vms.md)

### <a name="more-causes-because-of-vss-related-issues"></a>VSS ile ilgili sorunlar nedeniyle daha fazla neden:

Daha fazla sorun gidermek için, hata için tam hata kodunu almak için kaynak makinedeki dosyaları denetleyin:

`C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\Application Data\ApplicationPolicyLogs\vacp.log`

Hataları bulmak için, string **vacpError**için bir metin düzenleyici arama _vacp.log_ dosyasını açın.

```plaintext
Ex: vacpError:220#Following disks are in FilteringStopped state [\\.\PHYSICALDRIVE1=5, ]#220|^|224#FAILED: CheckWriterStatus().#2147754994|^|226#FAILED to revoke tags.FAILED: CheckWriterStatus().#2147754994|^|
```

Önceki örnekte, **2147754994** bu cümleyi izleyen hata hakkında size söyler hata kodudur.

#### <a name="vss-writer-is-not-installed---error-2147221164"></a>VSS yazar yüklü değil - Hata 2147221164

**Nasıl düzeltilir**: Uygulama tutarlılığı etiketi oluşturmak için Azure Site Kurtarma, Birim Gölge Kopyalama Hizmeti 'ni (VSS) kullanır. Site Kurtarma, uygulama tutarlılığı anlık görüntülerini almak için çalışması için bir VSS Sağlayıcısı yükler. Azure Site Kurtarma bu VSS Sağlayıcısını hizmet olarak yükler. VSS Sağlayıcısı yüklenmezse, uygulama tutarlılığı anlık görüntüsü oluşturma başarısız olur. Bu **hata kimliği 0x80040154 Sınıf kayıtlı değil**gösterir. [VSS yazar yükleme sorun giderme](vmware-azure-troubleshoot-push-install.md#vss-installation-failures)için makaleye bakın.

#### <a name="vss-writer-is-disabled---error-2147943458"></a>VSS yazar devre dışı bırakılır - Hata 2147943458

**Nasıl düzeltilir**: Uygulama tutarlılık etiketini oluşturmak için Azure Site Kurtarma VSS kullanır. Site Kurtarma, uygulama tutarlılığı anlık görüntülerini almak için çalışması için bir VSS Sağlayıcısı yükler. Bu VSS Sağlayıcısı bir hizmet olarak yüklenir. VSS Sağlayıcı hizmetini etkinleştirmediyseniz, uygulama tutarlılığı anlık görüntüsü oluşturma başarısız olur. Hatayı gösterir: **Belirtilen hizmet devre dışı bırakılır ve başlatılamaz (0x80070422)**.

VSS devre dışı bırakılırsa:

- VSS Sağlayıcı hizmetinin başlangıç türünün **Otomatik**olarak ayarlı olduğunu doğrulayın.
- Aşağıdaki hizmetleri yeniden başlatın:
  - VSS hizmeti.
  - Azure Site Kurtarma VSS Sağlayıcısı.
  - VDS hizmeti.

#### <a name="vss-provider-not_registered---error-2147754756"></a>VSS Provider NOT_REGISTERED - Hata 2147754756

**Nasıl düzeltilir**: Uygulama tutarlılık etiketini oluşturmak için Azure Site Kurtarma VSS kullanır. Azure Site Kurtarma VSS Sağlayıcısı hizmetinin yüklü olup olmadığını denetleyin.

VSS Sağlayıcısını yeniden yüklemek için aşağıdaki komutları kullanın:

1. Varolan sağlayıcıyı kaldırın:

   `"C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\InMageVSSProvider_Uninstall.cmd"`

1. VSS Sağlayıcısını Yeniden Yükleyin:

   `"C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\InMageVSSProvider_Install.cmd"`

VSS Sağlayıcı hizmetinin başlangıç türünün **Otomatik**olarak ayarlı olduğunu doğrulayın.

Aşağıdaki hizmetleri yeniden başlatın:

- VSS hizmeti.
- Azure Site Kurtarma VSS Sağlayıcısı.
- VDS hizmeti.
