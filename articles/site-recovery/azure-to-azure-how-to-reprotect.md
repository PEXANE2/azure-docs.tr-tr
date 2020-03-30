---
title: Azure Site Kurtarma ile Azure VM'lerini birincil bölgeye yeniden koruma | Microsoft Dokümanlar
description: Azure Site Kurtarma'yı kullanarak ikincil ve birincil bölge olan Azure VM'lerinin başarısız olduktan sonra nasıl yeniden korunuruz unu açıklar.
services: site-recovery
author: rajani-janaki-ram
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 11/27/2018
ms.author: rajanaki
ms.openlocfilehash: 73747b8331054cdc3bfa1f4073ccf2cdb62ab326
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80283251"
---
# <a name="reprotect-failed-over-azure-vms-to-the-primary-region"></a>Azure VM'ler üzerinde birincil bölgeye karşı başarısız olan Yeniden koruma

[Azure Site Kurtarma'yı](site-recovery-overview.md)kullanarak bir bölgeden diğerine Azure VM'leri [üzerinde başarısız](site-recovery-failover.md) olduğunuzda, VM'ler **korumasız** bir durumda ikincil bölgede önyükleme sağlar. VM'leri birincil bölgeye geri döndüremeistiyorsanız, aşağıdaki görevleri yapın:

1. İkinci bölgedeki VM'leri yeniden koruyun, böylece birincil bölgeye çoğalmaya başlarlar.
1. Yeniden koruma tamamlandıktan ve VM'ler çoğaldıktan sonra, ikincil bölgeden birincil bölgeye kadar başarısız olabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

- VM başarısız olması birincil bölgeden ikincil bölgeye doğru yapılmalıdır.
- Birincil hedef site kullanılabilir olmalıdır ve bu bölgede kaynaklara erişebilmeli veya kaynak oluşturabilmelisin.

## <a name="reprotect-a-vm"></a>VM'yi yeniden koruma

1. **Vault** > **Çoğaltılan öğelerde,** VM üzerinden başarısız olana sağ tıklayın ve Yeniden **Koruma'yı**seçin. Yeniden koruma yönü ikincilden birincile doğru gösterilmelidir.

   ![Yeniden Koruma](./media/site-recovery-how-to-reprotect-azure-to-azure/reprotect.png)

1. Kaynak grubunu, ağı, depolamayı ve kullanılabilirlik kümelerini gözden geçirin. Ardından **Tamam**'a tıklayın. Yeni olarak işaretlenmiş herhangi bir kaynak varsa, bunlar yeniden koruma işleminin bir parçası olarak oluşturulur.
1. Yeniden koruma işi, hedef alanı en son verilerle tohumlar. İş bittikten sonra delta çoğaltma gerçekleşir. Daha sonra, birincil siteye geri başarısız olabilir. Yeniden koruma sırasında özelleştirme seçeneğini kullanarak depolama hesabını veya kullanmak istediğiniz ağı seçebilirsiniz.

   ![Özelleştir seçeneğini](./media/site-recovery-how-to-reprotect-azure-to-azure/customize.png)

### <a name="customize-reprotect-settings"></a>Yeniden koruma ayarlarını özelleştirin

Yeniden koruma sırasında hedef VM'nin aşağıdaki özelliklerini özelleştirebilirsiniz.

![Özelleştirme](./media/site-recovery-how-to-reprotect-azure-to-azure/customizeblade.png)

|Özellik |Notlar  |
|---------|---------|
|Hedef kaynak grubu | VM'nin oluşturulduğu hedef kaynak grubunu değiştirin. Yeniden korumanın bir parçası olarak, hedef VM silinir. Başarısız olduktan sonra VM'yi oluşturmak için altında yeni bir kaynak grubu seçebilirsiniz. |
|Hedef sanal ağ | Yeniden koruma işi sırasında hedef ağ değiştirilemez. Ağı değiştirmek için ağ eşlemiyeniden yapın. |
|Hedef depolama (İkincil VM yönetilen diskler kullanmaz) | Başarısız olduktan sonra VM'nin kullandığı depolama hesabını değiştirebilirsiniz. |
|Çoğaltma yönetilen diskler (İkincil VM yönetilen diskleri kullanır) | Site Kurtarma, ikincil VM'nin yönetilen disklerini yansıtmak için birincil bölgede yineleme yönetilen diskler oluşturur. |
|Önbellek depolama | Çoğaltma sırasında kullanılacak bir önbellek depolama hesabı belirtebilirsiniz. Varsayılan olarak, yoksa yeni bir önbellek depolama hesabı oluşturulur. |
|Kullanılabilirlik kümesi | İkinci bölgedeki VM kullanılabilirlik kümesinin bir parçasıysa, birincil bölgedeki hedef VM için bir kullanılabilirlik kümesi seçebilirsiniz. Varsayılan olarak, Site Kurtarma birincil bölgede ayarlanan varolan kullanılabilirliği bulmaya çalışır ve onu kullanır. Özelleştirme sırasında yeni bir kullanılabilirlik kümesi belirtebilirsiniz. |

### <a name="what-happens-during-reprotection"></a>Yeniden koruma sırasında ne olur?

Varsayılan olarak, aşağıdakioluşur:

1. VM üzerinde başarısız olan bölgede bir önbellek depolama hesabı oluşturulur.
1. Hedef depolama hesabı (birincil bölgedeki özgün depolama hesabı) yoksa, yeni bir hesap oluşturulur. Atanan depolama hesabı adı, ikincil VM tarafından kullanılan depolama hesabının `asr`adıdır.
1. VM'niz yönetilen diskleri kullanıyorsa, ikincil VM disklerinden çoğaltılan verileri depolamak için birincil bölgede yineleme yönetilen diskler oluşturulur.
1. Hedef kullanılabilirlik kümesi yoksa, gerekirse yeniden koruma işinin bir parçası olarak yeni bir tane oluşturulur. Yeniden koruma ayarlarını özelleştirmişseniz, seçili küme kullanılır.

Bir yeniden koruma işini tetiklediğinizde ve hedef VM olduğunda aşağıdakiler oluşur:

1. Hedef tarafı VM çalışıyorsa kapatılır.
1. VM yönetilen diskler kullanıyorsa, özgün diskin bir `-ASRReplica` kopyası sonek ile oluşturulur. Özgün diskler silinir. Kopyalar `-ASRReplica` çoğaltma için kullanılır.
1. VM yönetilmeyen diskler kullanıyorsa, hedef VM'nin veri diskleri ayrılır ve çoğaltma için kullanılır. Os diskinin bir kopyası oluşturulur ve VM'ye eklenir. Özgün işletim sistemi diski ayrılmıştır ve çoğaltma için kullanılır.
1. Yalnızca kaynak disk ve hedef disk arasındaki değişiklikler eşitlenir. Diferansiyeller hem diskler karşılaştırılarak ve sonra aktarılarak hesaplanır. Yeniden korumayı tamamlamak için tahmini zamanı bulmak için aşağıya bakın.
1. Eşitleme tamamlandıktan sonra delta çoğaltma başlar ve çoğaltma ilkesine uygun bir kurtarma noktası oluşturulur.

Bir yeniden koruma işini tetiklediğinizde ve hedef VM ve diskler yoksa, aşağıdakiler oluşur:

1. VM yönetilen diskler kullanıyorsa, yineleme diskleri sonek ile `-ASRReplica` oluşturulur. Kopyalar `-ASRReplica` çoğaltma için kullanılır.
1. VM yönetilmeyen diskler kullanıyorsa, hedef depolama hesabında yineleme diskleri oluşturulur.
1. Tüm diskler başarısız bölgeden yeni hedef bölgeye kopyalanır.
1. Eşitleme tamamlandıktan sonra delta çoğaltma başlar ve çoğaltma ilkesine uygun bir kurtarma noktası oluşturulur.

#### <a name="estimated-time-to-do-the-reprotection"></a>Yeniden koruma yapmak için tahmini süre

Çoğu durumda, Azure Site Kurtarma kaynak bölgeye tam veri çoğaltmaz.
Aşağıdaki koşullar ne kadar veri çoğaltılacağını belirler:

1. Kaynak VM verileri kaynak grubu değişikliği/silme gibi bazı nedenlerden dolayı silinirse, bozulursa veya erişilemezse, kaynak bölgesinde kullanılacak veri bulunmadığından yeniden koruma sırasında tam bir ilk çoğaltma gerçekleşir.
1. Kaynak VM verilerine erişilebilirse, hem diskleri karşılaştırArak hem diskleri karşılaştırArak hem de aktarılarak yalnızca diferansiyeller hesaplanır. Tahmini zamanı almak için aşağıdaki tabloyu kontrol edin.

|Örnek durum | Yeniden korunmak için gereken süre |
|---|---|
|Kaynak bölgede 1 TB standart diskli 1 VM vardır.<br/>Yalnızca 127 GB veri kullanılır ve diskin geri kalanı boştur.<br/>Disk türü 60 MiB/S iş ile standarttır.<br/>Başarısız olduktan sonra veri değişikliği yok.| Yaklaşık süre: 45 dakika - 1,5 saat.<br/>Yeniden koruma sırasında, Site Kurtarma yaklaşık 45 dakika, 127 GB / 45 MBs alacak tüm verilerin checksum dolduracaktır.<br/>Site Kurtarma'nın yaklaşık 20-30 dakika otomatik ölçeklendirmesi için bazı ek su işleri süresi gereklidir.<br/>Çıkış suçlaması yok. |
|Kaynak bölgede 1 TB standart diskli 1 VM vardır.<br/>Yalnızca 127 GB veri kullanılır ve diskin geri kalanı boştur.<br/>Disk türü 60 MiB/S iş ile standarttır.<br/>45 GB veri başarısız olduktan sonra değişir.| Yaklaşık süre: 1 saat - 2 saat.<br/>Yeniden koruma sırasında, Site Kurtarma yaklaşık 45 dakika, 127 GB / 45 MBs alacak tüm verilerin checksum dolduracaktır.<br/>45 GB/ 45 MBps, yaklaşık 17 dakika 45 GB değişiklikleri uygulamak için transfer süresi.<br/>Çıkış ücretleri çekler için değil, 45 GB veri değişiklikleri için olacaktır. |

Birincil bölgeye geri döndükten sonra VM yeniden korunduğunda (örneğin, VM birincil bölgeden DR bölgesine yeniden korunuyorsa), hedef VM ve ilişkili NIC(ler) silinir.

VM, DR bölgesinden birincil bölgeye yeniden korunduğunda, eski birincil VM'yi ve ilişkili NIC(ler)i silmeyiz.

## <a name="next-steps"></a>Sonraki adımlar

VM korunduktan sonra, bir hata başlatabilirsiniz. Failover ikincil bölgedeki VM'yi kapatır ve bu işlem sırasında kısa bir kapalı kalma süresiyle birincil bölgede VM oluşturur ve önyüklemeye neden olabilir. Bu işlem için uygun bir zaman seçmenizi ve birincil siteye tam bir başarısızlık başlatmadan önce bir test başarısızlığını çalıştırmanızı öneririz. Azure Site Kurtarma başarısız ları hakkında [daha fazla bilgi edinin.](site-recovery-failover.md)
