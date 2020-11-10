---
title: Kurtarma noktalarını yönet
description: Azure Backup hizmetinin sanal makineler için kurtarma noktalarını nasıl yönettiğini öğrenin
ms.topic: conceptual
ms.date: 11/08/2020
ms.openlocfilehash: 256df693aba0f799c24bcba6defe846e5c37ccaa
ms.sourcegitcommit: 0dcafc8436a0fe3ba12cb82384d6b69c9a6b9536
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2020
ms.locfileid: "94428810"
---
# <a name="manage-recovery-points"></a>Kurtarma noktalarını yönet

Bu makalede, sanal makinelerde bekletme nasıl çalıştığı açıklanır. Yedeklemeler gerçekleştiğinde, hangi geri yükleme işlemlerinin gerçekleştirilebileceği kurtarma noktaları oluşturulur.

Sanal makineler için ilk yedekleme tam bir yedekleme ve sonraki yedeklemeler artımlı yedeklemelerdir.

## <a name="recovery-points-and-retention"></a>Kurtarma noktaları ve bekletme

### <a name="scheduled-initial-and-incremental-backup"></a>Zamanlanan ilk ve artımlı yedekleme

Dört bloktan oluşan bir veri diski ile sanal makine *v1* 'nin basitleştirilmiş bir örneğini ele alalım: blok 1, blok 2, blok 3 ve blok 4. Her blok 16 KB boyutlenemez.

![Dört bloğuyla sanal makine](./media/manage-recovery-points/four-blocks.png)

**1. adım-Ilk yedekleme:** İlk yedekleme tam bir yedeklemedir. Sonraki artımlı yedeklemelerin uygulandığı bir taban çizgisi görevi görür. Kaynak VM 'de blok 1 ve blok 2 ' ye yazılan verilerin olduğunu varsayalım. Aynı veriler, kurtarma hizmetleri Kasası depolamada D1 ve D2 olarak çoğaltılır.

![İlk yedekleme çoğaltıldı](./media/manage-recovery-points/initial-backup.png)

**2. adım-artımlı yedekleme 1:** VM 'nin blok 3 ' e eklenen yeni verileri göz önünde bulundurun. Aynı veriler sonraki artımlı yedeklemede çoğaltılır ve yalnızca değiştirilen blok D3 olarak depolanır.  Her adım sırasında, blok 1 KB değişse bile, 16 KB 'lık bloğun tamamı kurtarma noktasına yüklenir.

![İlk artımlı yedekleme](./media/manage-recovery-points/first-incremental-backup.png)

**3. adım-artımlı yedekleme 2:**  Şimdi, blok 3 ve blok 2 ' de kaynak VM üzerinde veri değişiklikleri olduğunu göz önünde bulundurun. Bu değişiklikler, sonraki artımlı yedeklemede D3 ' ve D2 ' olarak çoğaltılır.

![İkinci artımlı yedekleme](./media/manage-recovery-points/second-incremental-backup.png)

### <a name="on-demand-backup"></a>İsteğe bağlı yedekleme

Üzerinde koruma ayarladıktan sonra her zaman bir VM 'nin isteğe bağlı yedeklemesini çalıştırmayı tercih edebilirsiniz.

- İsteğe bağlı yedekleme, ilk zamanlanan ilk yedeklemeden önce tetikleniyorsa tam yedekleme olur.
- İlk yedekleme tamamlanmıştır ve isteğe bağlı bir yedekleme tetikleniyorsa, bu, artımlı bir yedektir.
- İsteğe bağlı yedekleme için oluşturulan kurtarma noktalarının saklama süresi, yedeklemeyi tetiklemeniz durumunda belirttiğiniz bekletme değeridir.

### <a name="storage-cost"></a>Depolama maliyeti

İlk yedekleme için oluşturulan **Kurtarma noktası** , verilerin bulunduğu tüm blokları içerir. Sonraki artımlı kurtarma noktaları yalnızca değiştirilen verileri olan bloklarında oluşur. Depolama maliyetleri, tüm kurtarma noktalarında yayılan tüm blokların toplamına karşılık gelir.

Her adımdan sonra depolama maliyetini anlamak için yukarıdaki örneği kullanalım:

|Adım  |Yedekleme türü  |Bloklar değişti  |Depolama türü |
|------|---------|---------|---------|
|1     |     İlk yedekleme    | Blok 1, blok 2        |    Kurtarma noktası 1 ' e (D1 + D2) karşılık gelen     |
|2     |  Artımlı yedekleme 1       |  Blok 3       |   Kurtarma noktası 1 (D1 + D2) + kurtarma noktası 2 (D3) öğesine karşılık gelen      |
|3     |    Artımlı yedekleme 2     |    Blok 2, blok 3     |   Kurtarma noktası 1 (D1 + D2) + kurtarma noktası 2 (D3) + kurtarma noktası 3 (D2 ' + D3 ') ile ilgili      |

### <a name="recovery-point-expiration"></a>Kurtarma noktası süre sonu

Her kurtarma noktasının, yedekleme ilkesinde belirtildiği gibi bir saklama süresi vardır. Temizleme, düzenli aralıklarla gerçekleşir ve bu süre dolduğunda tüm kurtarma noktaları temizlenir.

Kurtarma noktasının süresi dolduğunda, silinir ya da birleştirilir.

### <a name="case-1-initial-recovery-point-expires"></a>Durum 1: Ilk kurtarma noktası süresi doluyor

İlk kurtarma noktası sona erdiğinde, sonraki artımlı kurtarma noktasıyla birleştirir. Artımlı kurtarma noktasında üzerine yazılan tüm veri blokları silinir ve REST birleştirilir. Artımlı yedekleme, ilk tam yedekleme olur. Bir örnek ile gözden geçirelim:

- İlk yedekleme sırasında oluşturulan *Kurtarma noktası 1* , sanal makinenin tam yedeklemesini içerir.
- *Kurtarma noktası 1* süresi dolduğunda, *Kurtarma noktası 2* bir sonraki tam yedekleme olur.
- Bir blok D1, kurtarma noktası *2 ' de* , blok 2 ' deki verilerin üzerine yazıldığından, *Kurtarma noktası 2* ile birleştirilir ve D2 silinir. Bu değişiklik D2 ' i engelle olarak yakalanır.
- Bir sonraki yedeklemeden önce üzerinde yapılan herhangi bir değişiklik olana kadar D1 blok, ardışık kurtarma noktalarında olduğu gibi tutulur.

![İlk durum](./media/manage-recovery-points/first-case.png)

### <a name="case-2-in-between-incremental-recovery-point-expires"></a>Durum 2: Içinde artımlı kurtarma noktası süre sonu

- Kurtarma noktası *2* ' yi *Kurtarma noktası 1* ' den önce dolarsa, *Kurtarma noktası 2* ' deki veriler bir sonraki kullanılabilir kurtarma noktasıyla birleştirilir: *Kurtarma noktası 3*. Bu nedenle Block D3, *Kurtarma noktası 3* ile birleştirilir.
- *Kurtarma noktası 1* , D1 ve D2 bloğundaki tam yedekleme olmaya devam etmektedir.

![İkinci durum](./media/manage-recovery-points/second-case.png)

### <a name="case-3-on-demand-recovery-point-expires"></a>Durum 3: isteğe bağlı kurtarma noktası süre sonu

Bu örnekte, bir zamanlama (günlük yedekleme) ilkesi *n* gün bekletme süresiyle çalışacak şekilde zamanlanır.  İsteğe bağlı bir yedekleme, bir sonraki zamanlanmış yedeklemeden önceki dördüncü günde tetikleniyorsa ve Bekletme dönemi 10 gün olarak belirtildiğinde, hala artımlı bir yedekleme olur. Kurtarma noktası *3* sonrasında ve kurtarma *noktası 4* ' ten önce bir kurtarma noktası ( *isteğe bağlı RP1* ) oluşturulur.  14. günün sonunda, isteğe bağlı kurtarma noktası ( *isteğe bağlı RP1* ) sona erer ve bir sonraki kullanılabilir kurtarma noktasıyla birleştirilir. Sunucuda hala mevcut olan veri blokları birleştirilir, ancak değiştirilen (üzerine yazılan veya silinen) veri blokları, zaman aşımına uğradı kurtarma noktasından silinir.

![Üçüncü durum](./media/manage-recovery-points/third-case.png)

### <a name="impact-of-policy-change-on-recovery-points"></a>Kurtarma noktalarında ilke değişikliğinin etkisi

Bir ilke değiştirildiğinde, yeni ve var olan kurtarma noktalarına her ikisi de uygulanır. Daha fazla bilgi için bkz. [kurtarma noktalarında ilke değişikliği etkisi](backup-architecture.md#impact-of-policy-change-on-recovery-points).

### <a name="impact-of-stop-protection-on-recovery-points"></a>Kurtarma noktalarında koruma durdurma etkisi

Bir VM 'yi korumayı durdurmanın iki yolu vardır:

- **Korumayı durdurun ve yedekleme verilerini silin.** Bu seçenek, gelecekteki tüm yedekleme işlerinin VM 'nizi korumasını durdurur ve tüm kurtarma noktalarını siler. [Geçici silme](backup-azure-security-feature-cloud.md) etkinse, silinen veriler 14 gün boyunca korunur. Geçici olarak silinen durumdaki öğeler için ücretler tahakkuk etmemektedir. Verilerin 14 günlük süre içinde silinmesi geri alınabilir. Geçici silme etkinleştirilmemişse, veriler hemen temizlenir ve VM 'yi geri yükleyemez veya **yedeklemeyi** geri yükle seçeneğini kullanamazsınız.
- **Korumayı durdurun ve yedekleme verilerini koruyun.** Bu seçenek, gelecekteki tüm yedekleme işlerinin VM 'nizi korumasını durduracak. Ancak, Azure Backup hizmeti yedeklenen kurtarma noktalarını süresiz olarak korur. Kurtarma noktalarını kasada tutmak için ödeme yapmanız gerekir (Ayrıntılar için [Azure Backup fiyatlandırmasına](https://azure.microsoft.com/pricing/details/backup/) bakın). Gerekirse VM 'yi geri yükleyebileceksiniz. VM korumasını sürdürmeye karar verirseniz, **yedeklemeyi** geri edin seçeneğini kullanabilirsiniz. Yedeklemeyi sürdürmeden sonra, saklama kuralları sona erme noktalarına uygulanır. Ayrıca,  **yedekleme verilerini sil** seçeneğini kullanarak yedeklenen verileri silebilirsiniz.

## <a name="impact-of-deleting-a-vm-without-stop-protection"></a>Korumayı durdurmadan bir sanal makineyi silmenin etkisi

Bir VM 'yi durdurma koruması olmadan silmek, kurtarma noktalarını etkiler ve istenmeyen bir senaryodur. Sanal makine silinmeden önce ideal yedeklemeler durdurulmalıdır. Kaynak mevcut olmadığından, zamanlanmış yedeklemeler [VMNotFoundV2 hatasıyla](backup-azure-vms-troubleshoot.md#320001-resourcenotfound---could-not-perform-the-operation-as-vm-no-longer-exists--400094-bcmv2vmnotfound---the-virtual-machine-doesnt-exist--an-azure-virtual-machine-wasnt-found)başarısız olur. Kurtarma noktaları, bekletme ilkesine göre düzenli aralıklarla temizlenir, ancak sanal makinenin son kopyası süresiz olarak kalır ve buna göre faturalandırılırsınız. Senaryonuza bağlı olarak, aşağıdaki iki seçeneğe sahipsiniz:

- **Seçenek 1:** Kurtarma noktalarından herhangi birini kullanarak VM 'yi geri yükleyin. Silinen VM 'yi kurtarmak isterseniz aynı adı ve aynı kaynak grubunu kullanarak geri yükleyin. Geri yüklenen VM 'yi aynı kasaya koruyorsanız, var olan kurtarma noktaları otomatik olarak eklenir.
- **Seçenek 2:** Kurtarma Hizmetleri kasasına gidin ve verileri silme ile korumayı durdurun.

### <a name="impact-of-expired-recovery-points-for-items-in-soft-deleted-state"></a>Geçici olarak silinen durumdaki öğeler için süre dolma kurtarma noktalarının etkileri

Kurtarma Hizmetleri Kasası için [geçici silme](backup-azure-security-feature-cloud.md) etkinse, zaman aşımına uğradı kurtarma noktası geçici olarak silinmiş durumda kalır ve temizlenmez. Kurtarma noktası geçici olarak silinmiş durumda olduğunda ücret alınmaz.

### <a name="impact-of-churn-on-backup-performance"></a>Yedekleme performansı üzerindeki karmaşıklığın etkisi

Bir VM 'nin toplam depolamanın 8 TB olduğunu ve karmaşıklığın %5 olduğunu varsayalım. Bundan sonra karşılık gelen artımlı yedekleme depolaması, 0,4 TB olan 8 TB 'lık %5 olur. Daha yüksek dalgalanma, sonraki artımlı yedeklemeler için arka uç depolamaya karşılık gelir. Dalgalanma, yedekleme performansını etkiler. Karmaşıklığın yüksek olması, yedekleme işleminin ne kadar yavaş ve arka uç depolamanın tüketimine daha fazla.

Karmaşıklığın yedekleme performansını nasıl etkilediğini anlamak için şu senaryoya bakın:

|Sanal makineler  |VM1  |VM2  |VM3  |
|---------|---------|---------|---------|
|Veri diski sayısı    | 4 (A1, A2, A3, A4)        | 4 (B1, B2, B3, B4)        |  4 (C1, C2, C3, C4)       |
|Her diskin boyutu   |      4 TB   | 4 TB        |  4 TB       |
|Yedekleme verileri karmaşıklığı    |   A1-4 TB      | B1-1 TB; B2-1 TB <br> B3-1 TB; B4-1 TB  |   C1-2 TB; C4-2 TB      |

Yedekleme performansı Order VM2>VM3>VM1 şeklinde olacaktır. Bunun nedeni, yaygın olarak kullanılan verilerin çeşitli disklere yayılmaktadır. Disklerin yedeklemesi paralel olarak olduğundan, VM2 en iyi performansı gösterecektir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Backup mimarisi ve bileşenler](backup-architecture.md)
