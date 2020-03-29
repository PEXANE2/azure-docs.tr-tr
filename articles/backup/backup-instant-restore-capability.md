---
title: Azure Anında Geri Yükleme Özelliği
description: VM yedekleme yığını, Kaynak Yöneticisi dağıtım modeli için Azure Anında Geri Yükleme Özelliği ve SSS'leri
ms.reviewer: sogup
ms.topic: conceptual
ms.date: 04/23/2019
ms.openlocfilehash: 7cf437c6b20ea6b688e8e93e401cf71ef0260888
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76705437"
---
# <a name="get-improved-backup-and-restore-performance-with-azure-backup-instant-restore-capability"></a>Azure Yedekleme Anında Geri Yükleme özelliğiyle gelişmiş yedekleme ve performans geri yükleme

> [!NOTE]
> Kullanıcılardan gelen geri bildirimlere dayanarak, Azure Yığını işleviyle ilgili karışıklığı azaltmak için **VM yedekleme yığını V2'yi** **Anında Geri Yükleme** olarak yeniden adlandırıyoruz.
> Azure yedekleme kullanıcılarının tümü artık **Anında Geri Yükleme'ye**yükseltildi.

Anında Geri Yükleme için yeni model aşağıdaki özellik geliştirmeleri sağlar:

* Veri aktarımının tamamlanmasını beklemeden, kurtarma için kullanılabilen yedekleme işinin bir parçası olarak alınan anlık görüntüleri kullanma yeteneği. Geri yüklemeyi tetiklemeden önce anlık görüntünün kasaya kopyalanması için bekleme süresini azaltır.
* Anlık görüntüleri varsayılan olarak iki gün boyunca yerel olarak saklayarak yedekleme ve geri yükleme sürelerini azaltır. Bu varsayılan anlık görüntü tutma değeri 1 ila 5 gün arasındaki herhangi bir değer için yapılandırılabilir.
* 32 TB'a kadar disk boyutlarını destekler. Disklerin yeniden boyutlandırılması Azure Yedekleme tarafından önerilmez.
* Standart HDD diskler ve Premium SSD disklerle birlikte Standart SSD diskleri destekler.
* Geri yüklenirken yönetilmeyen bir VM orijinal depolama hesapları (disk başına) kullanma yeteneği. Bu yetenek, VM'de depolama hesapları arasında dağıtılan diskler olsa bile vardır. Çok çeşitli VM yapılandırmaları için geri yükleme işlemlerini hızlandırAr.
* Anında Geri Yükleme ile premium depolama alanı kullanan VM'lerin yedeklemesi için, **yalnızca** ilk yedekleme için gerekli olan toplam ayrılmış depolama alanının *%50* boş alanını ayırmanızı öneririz. %50 boş alan, ilk yedekleme tamamlandıktan sonra yedeklemeler için bir gereklilik değildir.

## <a name="whats-new-in-this-feature"></a>Bu özellikteki yenilikler

Şu anda, yedekleme işi iki aşamadan oluşur:

1. VM anlık görüntüsü alıyorum.
2. Azure Kurtarma Hizmetleri kasasına Bir VM anlık görüntüsü aktarma.

Kurtarma noktası, yalnızca aşama 1 ve 2 tamamlandıktan sonra oluşturulur. Bu yükseltmenin bir parçası olarak, anlık görüntü tamamlanır tamamlanmaz bir kurtarma noktası oluşturulur ve anlık görüntü türündeki bu kurtarma noktası aynı geri yükleme akışını kullanarak geri yükleme gerçekleştirmek için kullanılabilir. Azure portalındaki bu kurtarma noktasını kurtarma noktası türü olarak "anlık görüntü" kullanarak tanımlayabilirsiniz ve anlık görüntü kasaya aktarıldıktan sonra kurtarma noktası türü "anlık görüntü ve kasa" olarak değişir.

![VM yedekleme yığını Kaynak Yöneticisi dağıtım modelinde yedekleme işi-- depolama ve kasa](./media/backup-azure-vms/instant-rp-flow.png)

Varsayılan olarak, anlık görüntüler iki gün boyunca saklanır. Bu özellik, geri yükleme sürelerini azaltarak bu anlık görüntülerden geri yükleme sağlar. Verileri kasadan dönüştürmek ve kopyalamak için gereken süreyi azaltır.

## <a name="feature-considerations"></a>Özellik hususları

* Anlık görüntüler, kurtarma noktası oluşturmayı artırmak ve geri yükleme işlemlerini hızlandırmak için disklerle birlikte depolanır. Sonuç olarak, bu dönemde çekilen anlık görüntülere karşılık gelen depolama maliyetlerini görürsünüz.
* Artımlı anlık görüntüler sayfa lekeleri olarak depolanır. Yönetilmeyen diskleri kullanan tüm kullanıcılar, yerel depolama hesaplarında depolanan anlık görüntüler için ücretlendirilir. Yönetilen VM yedeklemeleri tarafından kullanılan geri yükleme noktası koleksiyonları temel depolama düzeyinde blob anlık görüntüleri kullandığından, yönetilen diskler için blob anlık görüntü fiyatlandırmasına karşılık gelen maliyetleri görürsünüz ve bunlar artımlıdır.
* Premium depolama hesapları için, anlık kurtarma noktaları için çekilen anlık görüntüler, ayrılan alanın 10-TB sınırına doğru sayılır.
* Geri yükleme gereksinimlerine göre anlık görüntü tutma yapılandırma küçümünü nşa Gereksinime bağlı olarak, anlık görüntü tutmayı aşağıda açıklandığı gibi yedekleme ilkesi bıçağında en az bir gün olarak ayarlayabilirsiniz. Bu, sık sık geri yükleme yapmazsanız anlık görüntü tutma için maliyet tasarrufu yardımcı olacaktır.
* Bu bir tek yönlü yükseltme, bir kez Anında geri yükleme yükseltilmiş, geri gidemez.

>[!NOTE]
>Bu anında geri yükleme yükseltmesi ile, tüm müşterilerin anlık görüntü saklama süresi **(yeni ve mevcut her ikisi de dahil)** iki günlük varsayılan değerolarak ayarlanır. Ancak, gereksiniminize göre süreyi 1 ila 5 gün arasında herhangi bir değere ayarlayabilirsiniz.

## <a name="cost-impact"></a>Maliyet etkisi

Artımlı anlık görüntüler, anında kurtarma için kullanılan VM'nin depolama hesabında depolanır. Artımlı anlık görüntü, anlık görüntü nün kapladığınız alanın, anlık görüntü oluşturulduktan sonra yazılan sayfaların işgal ettiği alana eşit olduğu anlamına gelir. Faturalandırma hala anlık görüntü tarafından işgal GB başına kullanılan alan için, ve GB başına fiyat [fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/managed-disks/)belirtilen aynıdır. Yönetilmeyen diskler kullanan VM'ler için anlık görüntüler her diskin VHD dosyasının menüsünde görülebilir. Yönetilen diskler için anlık görüntüler, belirlenmiş bir kaynak grubunda geri yükleme noktası toplama kaynağında depolanır ve anlık görüntülerin kendileri doğrudan görünmez.

>[!NOTE]
> Anlık görüntü tutma, haftalık ilkeler için 5 güne sabitlenir.

## <a name="configure-snapshot-retention"></a>Anlık görüntü tutma yapılandırma

### <a name="using-azure-portal"></a>Azure portalını kullanma

Azure portalında, **Anında Geri Yükleme** bölümünün altındaki **VM Yedekleme İlkesi** bıçağına eklenen bir alan görebilirsiniz. Belirli yedekleme ilkesiyle ilişkili tüm VM'ler için Anlık Görüntü Bekletme süresini **VM Yedekleme İlkesi** bıçağından değiştirebilirsiniz.

![Anında Geri Yükleme Özelliği](./media/backup-azure-vms/instant-restore-capability.png)

### <a name="using-powershell"></a>PowerShell’i kullanma

>[!NOTE]
> Az PowerShell sürüm 1.6.0'dan itibaren PowerShell'i kullanarak politikadaki anlık görüntü tutma süresini güncelleyebilirsiniz

```powershell
$bkpPol = Get-AzureRmRecoveryServicesBackupProtectionPolicy -WorkloadType "AzureVM"
$bkpPol.SnapshotRetentionInDays=5
Set-AzureRmRecoveryServicesBackupProtectionPolicy -policy $bkpPol
```

Her ilke için varsayılan anlık görüntü tutma iki gün olarak ayarlanır. Kullanıcı değeri en az 1 ve en fazla beş gün olarak değiştirebilir. Haftalık ilkeler için anlık görüntü tutma beş güne sabitlenir.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="what-are-the-cost-implications-of-instant-restore"></a>Anında geri yüklemenin maliyet etkileri nelerdir?

Anlık görüntüler, kurtarma noktası oluşturmayı hızlandırmak ve işlemleri geri yüklemek için disklerle birlikte depolanır. Sonuç olarak, VM yedekleme ilkesinin bir parçası olarak seçilen anlık görüntü tutma alanına karşılık gelen depolama maliyetlerini görürsünüz.

### <a name="in-premium-storage-accounts-do-the-snapshots-taken-for-instant-recovery-point-occupy-the-10-tb-snapshot-limit"></a>Premium Depolama hesaplarında, anında kurtarma noktası için çekilen anlık görüntüler 10-TB anlık görüntü sınırını kaplar mı?

Evet, premium depolama hesapları için anlık kurtarma noktası için çekilen anlık görüntüler, ayrılan anlık görüntü alanının 10 TB'sini kaplar.

### <a name="how-does-the-snapshot-retention-work-during-the-five-day-period"></a>Beş günlük süre boyunca anlık görüntü tutma nasıl çalışır?

Her gün yeni bir anlık görüntü alınır, sonra beş ayrı artımlı anlık görüntü vardır. Anlık görüntünün boyutu, çoğu durumda %2-7 civarında olan veri karmaşası bağlıdır.

### <a name="is-an-instant-restore-snapshot-an-incremental-snapshot-or-full-snapshot"></a>Anlık geri yükleme anlık görüntüsü artımlı bir anlık görüntü mü yoksa tam anlık görüntü mü?

Anında geri yükleme özelliğinin bir parçası olarak alınan anlık görüntüler, artımlı anlık görüntülerdir.

### <a name="how-can-i-calculate-the-approximate-cost-increase-due-to-instant-restore-feature"></a>Anında geri yükleme özelliği nedeniyle yaklaşık maliyet artışını nasıl hesaplayabilirim?

VM'nin karmaşaya bağlı. Sabit bir durumda, maliyet artışı = Anlık görüntü tutma süresi GB başına VM depolama maliyeti başına günlük churn olduğunu varsayabiliriz.

### <a name="if-the-recovery-type-for-a-restore-point-is-snapshot-and-vault-and-i-perform-a-restore-operation-which-recovery-type-will-be-used"></a>Bir geri yükleme noktası için kurtarma türü "Anlık görüntü ve tonoz" ise ve ben bir geri yükleme işlemi gerçekleştirmek, hangi kurtarma türü kullanılacak?

Kurtarma türü "anlık görüntü ve kasa" ise, geri yükleme otomatik olarak kasadan yapılan geri yükleme ile karşılaştırıldığında çok daha hızlı olacak yerel anlık görüntü, yapılır.

### <a name="what-happens-if-i-select-retention-period-of-restore-point-tier-2-less-than-the-snapshot-tier1-retention-period"></a>Anlık görüntü (Tier1) bekletme döneminden daha az geri yükleme noktası (Tier 2) bekletme süresini seçersem ne olur?

Anlık görüntü (Tier1) silinmedikçe, yeni model geri yükleme noktasının (Tier2) silinmesine izin vermez. Anlık bekletme döneminden daha büyük geri yükleme noktası (Tier2) bekletme süresini zamanlamanızı öneririz.

### <a name="why-is-my-snapshot-existing-even-after-the-set-retention-period-in-backup-policy"></a>Yedekleme ilkesindeki ayarlı bekletme döneminden sonra bile anlık görüntüm neden var?

Kurtarma noktası anlık görüntü varsa ve bu mevcut en son RP ise, bir sonraki başarılı yedekleme olana kadar tutulur. Bu, bugün en az bir en son RP'nin vm'deki bir sorun nedeniyle tüm yedeklemelerin başarısız olması durumunda her zaman hazır bulunmayı zorunlu kılan tasarlanmış "çöp toplama" (GC) politikasına göre dir. Normal senaryolarda, RP'ler son kullanma tarihinden en fazla 24 saat sonra temizlenir.

### <a name="i-dont-need-instant-restore-functionality-can-it-be-disabled"></a>Anında Geri Yükleme işlevine ihtiyacım yok. Devre dışı tutulabilir mi?

Anında geri yükleme özelliği herkes için etkinleştirilir ve devre dışı tutulamaz. Anlık görüntü tutma yı en aza indirebilirsiniz.

>[!NOTE]
> **Azure Yedekleme artık seçici disk yedeklemesini destekler ve Azure Sanal Makine yedekleme çözümlerini kullanarak geri yüklemeyi destekler.**
>
>Bugün Azure Yedekleme, Sanal Makine yedekleme çözümlerini kullanarak VM'deki tüm diskleri (İşletim Sistemi ve verileri) birlikte yedeklemeyi destekler. Diski dışlama işleviyle, VM'deki birçok veri diskinden bir veya birkaçını yedekleme seçeneğine sahip olursunuz. Bu, yedekleme ve geri yükleme gereksinimleriniz için verimli ve uygun maliyetli bir çözüm sağlar. Her kurtarma noktası yedekleme işlemine dahil edilen disklerin verilerini içerir ve bu da geri yükleme işlemi sırasında verilen kurtarma noktasından geri yüklenen disklerin bir alt kümesini geri yüklemenize olanak tanır. Bu, hem anlık görüntüden hem de kasadan geri yüklemek için geçerlidir.
>
>**Önizleme için kaydolmak için, bize yazınAskAzureBackupTeam@microsoft.com**
