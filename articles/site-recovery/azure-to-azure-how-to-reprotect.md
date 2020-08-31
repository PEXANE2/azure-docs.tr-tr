---
title: Azure VM 'lerini Azure Site Recovery ile birincil bölgeye yeniden koruma | Microsoft Docs
description: Azure VM 'lerinin yük devretme sonrasında, ikincil ve birincil bölge olan Azure Site Recovery kullanarak nasıl yeniden korunacağını açıklar.
services: site-recovery
author: Rajeswari-Mamilla
manager: gaggupta
ms.service: site-recovery
ms.topic: article
ms.date: 11/27/2018
ms.author: ramamill
ms.openlocfilehash: c3e4b1c97c5fcc86a863313c2d6f54d93f26390e
ms.sourcegitcommit: 420c30c760caf5742ba2e71f18cfd7649d1ead8a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89055050"
---
# <a name="reprotect-failed-over-azure-vms-to-the-primary-region"></a>Azure sanal makinelerinden birincil bölgeye yük devretme yeniden koruması başarısız oldu

Azure VM 'lerinden [Azure Site Recovery](site-recovery-overview.md)kullanarak bir bölgeden diğerine [Yük devretmek](site-recovery-failover.md) için VM 'ler, **korunmayan** bir durumda ikincil bölgede önyüklenir. VM 'Leri birincil bölgeye yeniden devretmek istiyorsanız aşağıdaki görevleri yapın:

1. İkincil bölgedeki VM 'Leri, birincil bölgeye çoğaltılmaya başlayacak şekilde yeniden koruyun.
1. Yeniden koruma tamamlandıktan ve VM 'Ler çoğaltıldıktan sonra ikinciden birincil bölgeye yük devretmek için yük devretme yapabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

- Birincil sunucudan ikincil bölgeye sanal makine yük devretmesi uygulanmalıdır.
- Birincil hedef site kullanılabilir olmalıdır ve bu bölgedeki kaynaklara erişebiliyor veya kaynak oluşturmanız gerekir.

## <a name="reprotect-a-vm"></a>VM 'yi yeniden koruma

1. **Kasa**  >  **çoğaltılan öğeler**' de, yük devredilen VM 'ye sağ tıklayın ve **yeniden koru**' yı seçin. Yeniden koruma yönü ikincilden birinciye göstermelidir.

   ![Yeniden koruma](./media/site-recovery-how-to-reprotect-azure-to-azure/reprotect.png)

1. Kaynak grubu, ağ, depolama ve kullanılabilirlik kümelerini gözden geçirin. Daha sonra, **Tamam**'a tıklayın. Yeni olarak işaretlenmiş kaynaklar varsa, bunlar yeniden koruma sürecinin bir parçası olarak oluşturulur.
1. Yeniden koruma işi hedef siteye en son verileri ekler. İş bittikten sonra değişim çoğaltması gerçekleşir. Ardından, birincil siteye yük devretmek için geri dönebilirsiniz. Özelleştirme seçeneğini kullanarak, yeniden koruma sırasında kullanmak istediğiniz depolama hesabını veya ağı seçebilirsiniz.

   ![Özelleştirme seçeneği](./media/site-recovery-how-to-reprotect-azure-to-azure/customize.png)

### <a name="customize-reprotect-settings"></a>Yeniden koruma ayarlarını özelleştirme

Yeniden koruma sırasında hedef VM 'nin aşağıdaki özelliklerini özelleştirebilirsiniz.

![Özelleştirme](./media/site-recovery-how-to-reprotect-azure-to-azure/customizeblade.png)

|Özellik |Notlar  |
|---------|---------|
|Hedef kaynak grubu | VM 'nin oluşturulduğu hedef kaynak grubunu değiştirin. Yeniden korumanın parçası olarak, hedef sanal makine silinir. Yük devretmeden sonra VM oluşturmak için yeni bir kaynak grubu seçebilirsiniz. |
|Hedef sanal ağ | Hedef ağ, yeniden koruma işi sırasında değiştirilemez. Ağı değiştirmek için Ağ eşlemesini yeniden yapın. |
|Hedef depolama (Ikincil VM yönetilen diskleri kullanmıyor) | Yük devretmeden sonra VM 'nin kullandığı depolama hesabını değiştirebilirsiniz. |
|Yönetilen çoğaltma diskleri (Ikincil VM yönetilen diskleri kullanır) | Site Recovery, birincil bölgede, ikincil VM 'nin yönetilen disklerini yansıtmak için çoğaltma yönetilen diskler oluşturur. |
|Önbellek depolama | Çoğaltma sırasında kullanılacak bir önbellek depolama hesabı belirtebilirsiniz. Varsayılan olarak, mevcut değilse yeni bir önbellek depolama hesabı oluşturulur. |
|Kullanılabilirlik kümesi | İkincil bölgedeki VM bir kullanılabilirlik kümesinin parçasıysa, birincil bölgedeki hedef VM için bir kullanılabilirlik kümesi seçebilirsiniz. Varsayılan olarak, Site Recovery mevcut kullanılabilirlik kümesini birincil bölgede bulmaya çalışır ve onu kullanır. Özelleştirme sırasında yeni bir kullanılabilirlik kümesi belirtebilirsiniz. |

### <a name="what-happens-during-reprotection"></a>Yeniden koruma sırasında ne olur?

Varsayılan olarak, aşağıdakiler gerçekleşir:

1. Yük devredilen VM 'nin çalıştığı bölgede bir önbellek depolama hesabı oluşturulur.
1. Hedef depolama hesabı (birincil bölgedeki orijinal depolama hesabı) yoksa, yeni bir tane oluşturulur. Atanan depolama hesabı adı, ikincil VM tarafından kullanılan ve soneki olan depolama hesabının adıdır `asr` .
1. VM 'niz yönetilen diskler kullanıyorsa, ikincil VM 'nin disklerinden çoğaltılan verileri depolamak için birincil bölgede çoğaltma yönetilen diskler oluşturulur.
1. Hedef kullanılabilirlik kümesi yoksa, gerekirse yeniden koruma işinin bir parçası olarak yeni bir tane oluşturulur. Yeniden koruma ayarlarını özelleştirdiyseniz, seçilen küme kullanılır.

Yeniden koruma işi tetikliyorsanız ve hedef VM varsa, aşağıdakiler gerçekleşir:

1. Çalışıyorsa, hedef tarafı VM kapalıdır.
1. VM yönetilen diskleri kullanıyorsa, orijinal diskin bir kopyası bir sonek ile oluşturulur `-ASRReplica` . Özgün diskler silinir. `-ASRReplica`Kopyalar, çoğaltma için kullanılır.
1. VM, yönetilmeyen diskler kullanıyorsa, hedef sanal makinenin veri diskleri ayrılır ve çoğaltma için kullanılır. İşletim sistemi diskinin bir kopyası oluşturulur ve VM 'ye eklenir. Özgün işletim sistemi diski ayrılır ve çoğaltma için kullanılır.
1. Yalnızca kaynak disk ve hedef disk arasındaki değişiklikler eşitlenir. Farklılıklar, her iki diski karşılaştırarak ve sonra aktarılarak hesaplanır. Yeniden korumanın tamamlanacağı tahmini süreyi bulmak için aşağıdaki onay işareti yapın.
1. Eşitleme tamamlandıktan sonra, Delta çoğaltma başlar ve çoğaltma ilkesi ile birlikte bir kurtarma noktası oluşturulur.

Yeniden koruma işi tetikliyorsanız ve hedef VM ve diskler yoksa, aşağıdakiler gerçekleşir:

1. VM yönetilen diskleri kullanıyorsa, çoğaltma diskleri sonek ile oluşturulur `-ASRReplica` . `-ASRReplica`Kopyalar, çoğaltma için kullanılır.
1. VM, yönetilmeyen diskler kullanıyorsa, çoğaltma diskleri hedef depolama hesabında oluşturulur.
1. Tüm diskler, yük devredilen bölgeden yeni hedef bölgeye kopyalanır.
1. Eşitleme tamamlandıktan sonra, Delta çoğaltma başlar ve çoğaltma ilkesi ile birlikte bir kurtarma noktası oluşturulur.

#### <a name="estimated-time-to-do-the-reprotection"></a>Yeniden koruma için tahmini süre

Çoğu durumda Azure Site Recovery, tüm verileri kaynak bölgeye çoğaltmaz.
Aşağıdaki koşullar, ne kadar veri çoğaltılacağı belirlenir:

1. Kaynak VM verileri silinmiş, bozuk veya bir nedenden dolayı bir kaynak grubu değişikliği/silme gibi bir nedenden dolayı, yeniden koruma sırasında, kullanılacak kaynak bölgede kullanılabilir veri olmadığından, tüm ilk çoğaltma işlemi gerçekleşir.
1. Kaynak VM verileri erişilebilir ise, yalnızca farklılıklar, diskler karşılaştırılarak ve sonra aktarıldıktan sonra hesaplanır. Tahmini süreyi almak için aşağıdaki tabloyu kontrol edin.

|Örnek durum | Yeniden korumak için geçen süre |
|---|---|
|Kaynak bölgesinin 1 TB standart diski olan 1 VM 'si vardır.<br/>Yalnızca 127 GB veri kullanılır ve diskin geri kalanı boştur.<br/>Disk türü 60 MBps aktarım hızı ile standarttır.<br/>Yük devretmeden sonra veri değişikliği yok.| Yaklaşık süre: 60-90 dakika.<br/> Yeniden koruma sırasında Site Recovery tüm verilerin sağlama toplamını dolduracaktır. Bu, 45Mb/sn ' de çalışır, bu nedenle, geçecek toplam süre 127 GB/45 MBps, yaklaşık 45 dakika olur.<br/>Site Recovery otomatik olarak ölçeklendirmek için bazı ek yük süresi gerekir, yaklaşık 20-30 dakika. |
|Kaynak bölgesinin 1 TB standart diski olan 1 VM 'si vardır.<br/>Yalnızca 127 GB veri kullanılır ve diskin geri kalanı boştur.<br/>Disk türü 60 MBps aktarım hızı ile standarttır.<br/>Yük devretmeden sonra 45 GB veri değişikliği.| Yaklaşık süre: 2,5-3 saat.<br/> Yeniden koruma sırasında Site Recovery tüm verilerin sağlama toplamını dolduracaktır. Bu, 45Mb/sn ' de çalışır, bu nedenle, geçecek toplam süre 127 GB/45 MBps, yaklaşık 45 dakika olur.<br/>Aktarım hızı, üretilen işleme yaklaşık %16 oranında veya 9.6 'ya MB/sn 'dir. Bu nedenle, 45 GB/9.6 'ya MBps ve yaklaşık 80 dakika olan 45 GB değişikliklerini uygulamak için aktarım süresi.<br/>Site Recovery otomatik olarak ölçeklendirmek için bazı ek yük süresi gerekir, yaklaşık 20-30 dakika. |
|Kaynak bölgesinin 1 TB standart diski olan 1 VM 'si vardır.<br/>Yalnızca 20 GB veri kullanılır ve diskin geri kalanı boştur.<br/>Disk türü 60 MBps aktarım hızı ile standarttır.<br/>Yük devretme işleminden hemen sonra diskteki ilk veriler 15 GB idi. Yük devretmeden sonra 5 GB veri değişikliği vardı. Bu nedenle toplam doldurulmuş veri sayısı 20 GB 'dir.| Yaklaşık saat: 1-1,5 saat.<br/>Diskte doldurulmuş veriler disk boyutunun %10 ' dan az olduğundan, bir ilk çoğaltma işlemi yaptık.<br/> Aktarım hızı, üretilen işleme yaklaşık %16 oranında veya 9.6 'ya MB/sn 'dir. Bu nedenle, 20 GB/9.6 'ya MBps, yaklaşık 36 dakika olan 20 GB değişiklikleri uygulamak için aktarım süresi.<br/>Site Recovery otomatik olarak ölçeklendirmek için bazı ek yük süresi gerekir, yaklaşık 20-30 dakika. |
|Kaynak bölgesinin 1 TB Premium diski olan 1 VM 'si vardır.<br/>Yalnızca 127 GB veri kullanılır ve diskin geri kalanı boştur.<br/>Disk türü 200 MBps aktarım hızı ile Premium ' dur.<br/>Yük devretmeden sonra veri değişikliği yok.| Yaklaşık saat: 2 saat.<br/>Yeniden koruma sırasında Site Recovery tüm verilerin sağlama toplamını dolduracaktır. Bu, 25 MB/sn hızında (disk aktarım hızı %16 ' a kadar) çalışır; bu nedenle, geçecek toplam süre 127 GB/25 MBps, yaklaşık 87 dakika olur.<br/>Site Recovery otomatik olarak ölçeklendirmek için bazı ek yük süresi gerekir, yaklaşık 20-30 dakika. |
|Kaynak bölgesinin 1 TB Premium diski olan 1 VM 'si vardır.<br/>Yalnızca 127 GB veri kullanılır ve diskin geri kalanı boştur.<br/>Disk türü 200 MBps aktarım hızı ile Premium ' dur.<br/>Yük devretmeden sonra 45 GB veri değişikliği.| Yaklaşık süre: 2,5-3 saat.<br/>Yeniden koruma sırasında Site Recovery tüm verilerin sağlama toplamını dolduracaktır. Bu, 25 MB/sn hızında (disk aktarım hızı %16 ' a kadar) çalışır; bu nedenle, geçecek toplam süre 127 GB/25 MBps, yaklaşık 87 dakika olur.</br>Aktarım hızı, üretilen işleme yaklaşık %16 veya 32MBps. Bu nedenle, 45 GB/32 MBps ve yaklaşık 24 dakika olan 45 GB değişikliklerini uygulamak için aktarım süresi.<br/>Site Recovery otomatik olarak ölçeklendirmek için bazı ek yük süresi gerekir, yaklaşık 20-30 dakika. |
|Kaynak bölgesinin 1 TB Premium diski olan 1 VM 'si vardır.<br/>Yalnızca 20 GB veri kullanılır ve diskin geri kalanı boştur.<br/>Disk türü 200 MBps aktarım hızı ile Premium ' dur.<br/>Yük devretme işleminden hemen sonra diskteki ilk veriler 15 GB idi. Yük devretmeden sonra 5 GB veri değişikliği vardı. Bu nedenle toplam doldurulmuş veri sayısı 20 GB 'dir| Yaklaşık süre: 30-45 dakika.<br/>Diskte doldurulmuş veriler disk boyutunun %10 ' dan az olduğundan, bir ilk çoğaltma işlemi yaptık.<br/>Aktarım hızı, üretilen işleme yaklaşık %16 veya 32MBps. Bu nedenle, 20 GB/32 MBps ve yaklaşık 11 dakika olan 20 GB değişiklikleri uygulamak için aktarım süresi.<br/>Site Recovery otomatik olarak ölçeklendirmek için bazı ek yük süresi gerekir, yaklaşık 20-30 dakika |

VM birincil bölgeye geri çağrıldıktan sonra yeniden korunuyorsa (yani, VM birincil bölgeden DR bölgesine yeniden korunuyorsa), hedef VM ve ilişkili NIC 'ler silinir.

VM, DR bölgesinden birincil bölgeye yeniden korunuyorsa, Erstwhile Birincil VM ve ilişkili NIC 'leri silmedik.

## <a name="next-steps"></a>Sonraki adımlar

VM korunduktan sonra bir yük devretme işlemi başlatabilirsiniz. Yük devretme, İkincil bölgedeki sanal makineyi kapatır ve bu işlem sırasında kısa kapalı kalma süresiyle birincil bölgede VM 'yi oluşturur ve önyükler. Bu işlem için uygun bir zaman seçmenizi ve birincil siteye tam yük devretme başlatmadan önce yük devretme testi çalıştırmanızı öneririz. Azure Site Recovery yük devretme hakkında [daha fazla bilgi edinin](site-recovery-failover.md) .
