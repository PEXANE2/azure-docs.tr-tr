---
title: Redis için Azure Önbelleği için coğrafi çoğaltma nasıl ayarlanır | Microsoft Dokümanlar
description: Redis örnekleri için Azure Önbelleğinizi coğrafi bölgelerde nasıl kopyalayacağınızı öğrenin.
author: yegu-ms
ms.service: cache
ms.topic: conceptual
ms.date: 03/06/2019
ms.author: yegu
ms.openlocfilehash: ce50c665fa79c361f638fda4ec373d5215c407f8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74129422"
---
# <a name="how-to-set-up-geo-replication-for-azure-cache-for-redis"></a>Redis için Azure Önbelleği için coğrafi çoğaltma nasıl ayarlanır?

Coğrafi çoğaltma, Redis örnekleri için iki Premium katmanAzure Önbelleği bağlamak için bir mekanizma sağlar. Bir önbellek birincil bağlı önbellek olarak, diğeri ise ikincil bağlı önbellek olarak seçilir. İkincil bağlantılı önbellek salt okunur olur ve birincil önbelleğe yazılan veriler ikincil bağlantılı önbelleğe kopyalanır. Bu işlev, Azure bölgelerinde bir önbelleği çoğaltmak için kullanılabilir. Bu makalede, Redis örnekleri için Premium katmanı Azure Önbelleğiniz için coğrafi çoğaltma yapılandırma kılavuzu verilmektedir.

## <a name="geo-replication-prerequisites"></a>Coğrafi çoğaltma ön koşulları

İki önbellek arasında coğrafi çoğaltmayı yapılandırmak için aşağıdaki ön koşullar yerine getirilmelidir:

- Her iki önbellek de [Premium katman](cache-premium-tier-intro.md) önbellekleridir.
- Her iki önbellek de aynı Azure aboneliğindedir.
- İkincil bağlantılı önbellek, birincil bağlı önbellekten önce aynı önbellek boyutu veya daha büyük bir önbellek boyutudur.
- Her iki önbellek de oluşturulur ve çalışan bir durumda.

Bazı özellikler coğrafi çoğaltma ile desteklenmez:

- Kalıcılık coğrafi çoğaltma ile desteklenmez.
- Kümeleme, her iki önbelleğe de kümelenme etkinve aynı sayıda parçaya sahipse desteklenir.
- Aynı VNET'teki önbellekler desteklenir.
- Farklı VNET'lerde önbellekler uyarılarla desteklenir. Bkz. Daha fazla bilgi için [bir VNET'teki önbelleklerimle coğrafi çoğaltma yı kullanabilir miyim?](#can-i-use-geo-replication-with-my-caches-in-a-vnet)

Coğrafi çoğaltma yapılandırıldıktan sonra, bağlı önbellek çiftiniz için aşağıdaki kısıtlamalar geçerlidir:

- İkincil bağlantılı önbellek salt okunur; ondan okuyabilirsiniz, ancak ona herhangi bir veri yazamaz. 
- Bağlantı eklenmeden önce ikincil bağlı önbellekte olan tüm veriler kaldırılır. Ancak, coğrafi çoğaltma daha sonra kaldırılırsa, çoğaltılan veriler ikincil bağlı önbellekte kalır.
- Önbellekler [scale](cache-how-to-scale.md) bağlıyken önbelleği de ölçeklendiremezsiniz.
- Önbellek kümeleme etkinse, [parça sayısını değiştiremezsiniz.](cache-how-to-premium-clustering.md)
- Her iki önbellekte de kalıcılığı etkinleştiremezsiniz.
- Her iki önbellekten [dışa aktarabilirsiniz.](cache-how-to-import-export-data.md#export)
- İkincil bağlantılı [Import](cache-how-to-import-export-data.md#import) önbelleğe aktaramazsınız.
- Önbellekleri açana kadar bağlantılı önbelleği veya bunları içeren kaynak grubunu silemezsiniz. Daha fazla bilgi için bkz. [Bağlı önbelleğimi silmeye çalıştığımda işlem neden başarısız oldu?](#why-did-the-operation-fail-when-i-tried-to-delete-my-linked-cache)
- Önbellekler farklı bölgelerdeyse, ağ çıkış maliyetleri bölgeler arasında taşınan veriler için geçerlidir. Daha fazla bilgi için bkz: [Verilerimi Azure bölgelerinde çoğaltmak ne kadara mal olur?](#how-much-does-it-cost-to-replicate-my-data-across-azure-regions)
- Birincil ve ikincil bağlı önbellek arasında otomatik hata yapmaz. İstemci uygulamasının nasıl başarısız olduğu hakkında daha fazla bilgi ve bilgi için [bkz.](#how-does-failing-over-to-the-secondary-linked-cache-work)

## <a name="add-a-geo-replication-link"></a>Coğrafi çoğaltma bağlantısı ekleme

1. Coğrafi çoğaltma için iki önbelleği birbirine bağlamak için, birincil bağlantılı önbellek olmasını istediğiniz önbelleğin Kaynak menüsünden **Coğrafi çoğaltma** düğmesini tıklatın. Ardından, **Geo-çoğaltma** bıçağından **önbellek çoğaltma bağlantısını ekle'yi** tıklatın.

    ![Bağlantı ekle](./media/cache-how-to-geo-replication/cache-geo-location-menu.png)

2. **Uyumlu önbellekler** listesinden amaçlanan ikincil önbelleğin adını tıklatın. İkincil önbelleğiniz listede görüntülenmiyorsa, ikincil önbelleğin [Coğrafi çoğaltma ön koşullarının](#geo-replication-prerequisites) karşılandığını doğrulayın. Önbellekleri bölgeye göre filtrelemek için, yalnızca **Uyumlu önbellekler** listesindeki önbellekleri görüntülemek için haritadaki bölgeyi tıklatın.

    ![Coğrafi çoğaltma uyumlu önbellekler](./media/cache-how-to-geo-replication/cache-geo-location-select-link.png)
    
    Ayrıca bağlam menüsünü kullanarak bağlantı işlemini başlatabilir veya ikincil önbellekle ilgili ayrıntıları görüntüleyebilirsiniz.

    ![Coğrafi çoğaltma bağlam menüsü](./media/cache-how-to-geo-replication/cache-geo-location-select-link-context-menu.png)

3. İki önbelleğe birbirine bağlamak ve çoğaltma işlemini başlatmak için **Bağlantı'yı** tıklatın.

    ![Bağlantı önbellekleri](./media/cache-how-to-geo-replication/cache-geo-location-confirm-link.png)

4. **Coğrafi çoğaltma** bıçağında çoğaltma işleminin ilerlemesini görüntüleyebilirsiniz.

    ![Bağlantı durumu](./media/cache-how-to-geo-replication/cache-geo-location-linking.png)

    Ayrıca, hem birincil hem de ikincil önbellekler için **Genel Bakış** bıçağındaki bağlantı durumunu da görüntüleyebilirsiniz.

    ![Önbellek durumu](./media/cache-how-to-geo-replication/cache-geo-location-link-status.png)

    Çoğaltma işlemi tamamlandıktan **sonra, Bağlantı durumu** **Başarılı**olarak değişir.

    ![Önbellek durumu](./media/cache-how-to-geo-replication/cache-geo-location-link-successful.png)

    Birincil bağlı önbellek bağlama işlemi sırasında kullanılabilir kalır. Bağlantı işlemi tamamlanana kadar ikincil bağlantılı önbellek kullanılamaz.

## <a name="remove-a-geo-replication-link"></a>Coğrafi çoğaltma bağlantısını kaldırma

1. İki önbellek arasındaki bağlantıyı kaldırmak ve coğrafi çoğaltmayı durdurmak **için, Coğrafi çoğaltma** bıçağından **Bağlantıyı Kaldır önbelleklerini** tıklatın.
    
    ![Bağlantıyı açma önbellekleri](./media/cache-how-to-geo-replication/cache-geo-location-unlink.png)

    Bağlantı açma işlemi tamamlandığında, ikincil önbellek hem okuma hem de yazma için kullanılabilir.

>[!NOTE]
>Coğrafi çoğaltma bağlantısı kaldırıldığında, birincil bağlı önbellekten çoğaltılan veriler ikincil önbellekte kalır.
>
>

## <a name="geo-replication-faq"></a>Coğrafi çoğaltma SSS

- [Standart veya Temel katman önbelleğiyle coğrafi çoğaltma kullanabilir miyim?](#can-i-use-geo-replication-with-a-standard-or-basic-tier-cache)
- [Önbelleğim bağlama veya bağlama işlemi sırasında kullanılabilir mi?](#is-my-cache-available-for-use-during-the-linking-or-unlinking-process)
- [İkiden fazla önbellekbağlantı miyim?](#can-i-link-more-than-two-caches-together)
- [Farklı Azure aboneliklerinden iki önbellek bağlayabilir miyim?](#can-i-link-two-caches-from-different-azure-subscriptions)
- [Farklı boyutlarda iki önbellek bağlayabilir miyim?](#can-i-link-two-caches-with-different-sizes)
- [Kümeleme etkinken coğrafi çoğaltma kullanabilir miyim?](#can-i-use-geo-replication-with-clustering-enabled)
- [Bir VNET'te önbelleklerimle coğrafi çoğaltma kullanabilir miyim?](#can-i-use-geo-replication-with-my-caches-in-a-vnet)
- [Redis coğrafi çoğaltma için çoğaltma zamanlaması nedir?](#what-is-the-replication-schedule-for-redis-geo-replication)
- [Coğrafi çoğaltma çoğaltma ne kadar sürer?](#how-long-does-geo-replication-replication-take)
- [Çoğaltma kurtarma noktası garanti mi?](#is-the-replication-recovery-point-guaranteed)
- [Coğrafi çoğaltmayı yönetmek için PowerShell veya Azure CLI kullanabilir miyim?](#can-i-use-powershell-or-azure-cli-to-manage-geo-replication)
- [Verilerimi Azure bölgelerinde çoğaltmak ne kadara mal olur?](#how-much-does-it-cost-to-replicate-my-data-across-azure-regions)
- [Bağlı önbelleğimi silmeye çalıştığımda işlem neden başarısız oldu?](#why-did-the-operation-fail-when-i-tried-to-delete-my-linked-cache)
- [İkincil bağlantılı önbelleğim için hangi bölgeyi kullanmalıyım?](#what-region-should-i-use-for-my-secondary-linked-cache)
- [İkincil bağlantılı önbellek üzerinde başarısız nasıl çalışır?](#how-does-failing-over-to-the-secondary-linked-cache-work)

### <a name="can-i-use-geo-replication-with-a-standard-or-basic-tier-cache"></a>Standart veya Temel katman önbelleğiyle coğrafi çoğaltma kullanabilir miyim?

Hayır, coğrafi çoğaltma yalnızca Premium katman önbellekleri için kullanılabilir.

### <a name="is-my-cache-available-for-use-during-the-linking-or-unlinking-process"></a>Önbelleğim bağlama veya bağlama işlemi sırasında kullanılabilir mi?

- Bağlama sırasında, bağlama işlemi tamamlandığında birincil bağlı önbellek kullanılabilir kalır.
- Bağlantı yaparken, bağlama işlemi tamamlanana kadar ikincil bağlantılı önbellek kullanılamaz.
- Bağlantılarını kaldırma işlemi tamamlarken her iki önbellek de kullanılabilir kalır.

### <a name="can-i-link-more-than-two-caches-together"></a>İkiden fazla önbellekbağlantı miyim?

Hayır, sadece iki önbellek birbirine bağlayabilirsiniz.

### <a name="can-i-link-two-caches-from-different-azure-subscriptions"></a>Farklı Azure aboneliklerinden iki önbellek bağlayabilir miyim?

Hayır, her iki önbellek de aynı Azure aboneliğinde olmalıdır.

### <a name="can-i-link-two-caches-with-different-sizes"></a>Farklı boyutlarda iki önbellek bağlayabilir miyim?

Evet, ikincil bağlı önbellek birincil bağlı önbellekten daha büyük olduğu sürece.

### <a name="can-i-use-geo-replication-with-clustering-enabled"></a>Kümeleme etkinken coğrafi çoğaltma kullanabilir miyim?

Evet, her iki önbelleğe de aynı sayıda parça olduğu sürece.

### <a name="can-i-use-geo-replication-with-my-caches-in-a-vnet"></a>Bir VNET'te önbelleklerimle coğrafi çoğaltma kullanabilir miyim?

Evet, VNETs önbelleklerin coğrafi çoğaltma uyarılar ile desteklenir:

- Aynı VNET'teki önbellekler arasındaki coğrafi çoğaltma desteklenir.
- Farklı VNET'lerde önbellekler arasındaki coğrafi çoğaltma da desteklenir.
  - VNET'ler aynı bölgedeyse, bunları [VNET eşleme](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) veya [VPN Ağ Geçidi VNET-to-VNET bağlantısını](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways#V2V)kullanarak bağlayabilirsiniz.
  - VNET'ler farklı bölgelerdeyse, Temel iç yük dengeleyicileri ile ilgili bir kısıtlama nedeniyle VNET eşlemesini kullanarak coğrafi çoğaltma desteklenmez. VNET eşleme kısıtlamaları hakkında daha fazla bilgi için [Sanal Ağ - Peering - Gereksinimler ve kısıtlamalar](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering#requirements-and-constraints)alabilirsiniz. Önerilen çözüm vpn ağ geçidi VNET-to-VNET bağlantısı kullanmaktır.

[Bu Azure şablonu](https://azure.microsoft.com/resources/templates/201-redis-vnet-geo-replication/)kullanarak, vpn ağ geçidi VNET'den VNET bağlantısına bağlı bir VNET'e coğrafi olarak çoğaltılan iki önbellek dağıtabilirsiniz.

### <a name="what-is-the-replication-schedule-for-redis-geo-replication"></a>Redis coğrafi çoğaltma için çoğaltma zamanlaması nedir?

Çoğaltma sürekli ve eşzamanlıdır ve belirli bir zamanlamada gerçekleşmez. Birincil yapılan tüm yazmaanında ve eşzamanlı olarak ikincil çoğaltılır.

### <a name="how-long-does-geo-replication-replication-take"></a>Coğrafi çoğaltma çoğaltma ne kadar sürer?

Çoğaltma artımlı, eşzamanlı ve süreklidir ve alınan süre bölgeler arası gecikme süresinden çok farklı değildir. Belirli koşullar altında, ikincil önbellek birincil verileri tam bir eşitlemek yapmak için gerekli olabilir. Bu durumda çoğaltma süresi gibi etkenlerin sayısına bağlıdır: birincil önbellek, kullanılabilir ağ bant genişliği ve bölgeler arası gecikme. Tam 53 GB coğrafi olarak çoğaltılmış bir çiftin çoğaltma süresinin 5 ila 10 dakika arasında olabileceğini bulduk.

### <a name="is-the-replication-recovery-point-guaranteed"></a>Çoğaltma kurtarma noktası garanti mi?

Coğrafi olarak çoğaltılan bir modda önbellekler için kalıcılık devre dışı bırakılır. Coğrafi olarak çoğaltılan bir çift, müşteri tarafından başlatılan başarısızlık gibi bağlantısızsa, ikincil bağlantılı önbellek senkronize verilerini o zamana kadar tutar. Bu gibi durumlarda hiçbir kurtarma noktası garanti değildir.

Kurtarma noktası elde etmek için, her iki önbellekten [dışa aktarın.](cache-how-to-import-export-data.md#export) Daha sonra birincil bağlı önbelleğe [aktarabilirsiniz.](cache-how-to-import-export-data.md#import)

### <a name="can-i-use-powershell-or-azure-cli-to-manage-geo-replication"></a>Coğrafi çoğaltmayı yönetmek için PowerShell veya Azure CLI kullanabilir miyim?

Evet, coğrafi çoğaltma Azure portalı, PowerShell veya Azure CLI kullanılarak yönetilebilir. Daha fazla bilgi için [PowerShell dokümanlarına](https://docs.microsoft.com/powershell/module/az.rediscache/?view=azps-1.4.0#redis_cache) veya [Azure CLI dokümanlarına](https://docs.microsoft.com/cli/azure/redis/server-link?view=azure-cli-latest)bakın.

### <a name="how-much-does-it-cost-to-replicate-my-data-across-azure-regions"></a>Verilerimi Azure bölgelerinde çoğaltmak ne kadara mal olur?

Coğrafi çoğaltma kullanılırken, birincil bağlı önbellekten elde edilen veriler ikincil bağlantılı önbelleğe kopyalanır. Bağlantılı iki önbellek aynı bölgedeyse, veri aktarımı için herhangi bir ücret alınmaz. Bağlı iki önbellek farklı bölgelerdeyse, veri aktarım ücreti her iki bölgede de hareket eden verilerin ağ çıkış maliyetidir. Daha fazla bilgi için [Bkz. Bant Genişliği Fiyatlandırma Ayrıntıları.](https://azure.microsoft.com/pricing/details/bandwidth/)

### <a name="why-did-the-operation-fail-when-i-tried-to-delete-my-linked-cache"></a>Bağlı önbelleğimi silmeye çalıştığımda işlem neden başarısız oldu?

Coğrafi çoğaltılan önbellekler ve kaynak grupları, siz coğrafi çoğaltma bağlantısını kaldırana kadar bağlanırken silinemez. Bağlı önbelleklerden birini veya her ikisini içeren kaynak grubunu silmeye çalışırsanız, kaynak grubundaki diğer kaynaklar `deleting` silinir, ancak kaynak grubu durumda kalır `running` ve kaynak grubundaki bağlı önbellekler durumda kalır. Kaynak grubunu ve içindeki bağlantılı önbellekleri tamamen silmek için, [bir coğrafi çoğaltma bağlantısını kaldır'da](#remove-a-geo-replication-link)açıklandığı gibi önbelleklerin bağlantısını kaldırın.

### <a name="what-region-should-i-use-for-my-secondary-linked-cache"></a>İkincil bağlantılı önbelleğim için hangi bölgeyi kullanmalıyım?

Genel olarak, önbelleğinizin, kendisine erişen uygulamayla aynı Azure bölgesinde bulunması önerilir. Ayrı birincil ve geri dönüş bölgelerine sahip uygulamalar için, birincil ve ikincil önbelleklerinizin aynı bölgelerde bulunması önerilir. Eşlenen bölgeler hakkında daha fazla bilgi için En [İyi Uygulamalar – Azure Eşleştirilmiş bölgelere](../best-practices-availability-paired-regions.md)bakın.

### <a name="how-does-failing-over-to-the-secondary-linked-cache-work"></a>İkincil bağlantılı önbellek üzerinde başarısız nasıl çalışır?

Azure bölgelerinde otomatik hata, coğrafi olarak çoğaltılan önbellekler için desteklenmez. Olağanüstü durum kurtarma senaryosunda, müşteriler tüm uygulama yığınını yedekleme bölgelerinde eşgüdümlü bir şekilde gündeme getirmelidir. Tek tek uygulama bileşenlerinin yedeklemelerine ne zaman geçeceklerine kendi başlarına karar vermelerine izin vermek performansı olumsuz etkileyebilir. Redis'in en önemli avantajlarından biri çok düşük gecikmeli bir mağaza olmasıdır. Müşterinin ana uygulaması önbelleğinden farklı bir bölgedeyse, eklenen gidiş-dönüş süresi performans üzerinde belirgin bir etkiye sahip olacaktır. Bu nedenle, geçici kullanılabilirlik sorunları nedeniyle otomatik olarak başarısız önlemek.

Müşteri tarafından başlatılan bir başarısızlık başlatmak için, önce önbelleklerin bağlantısını aç. Ardından Redis istemcinizi değiştirip (eski bağlantılı) ikincil önbelleğin bağlantı bitiş noktasını kullanın. İki önbellek bağlantısı zedilenolduğunda, ikincil önbellek yeniden normal bir okuma yazma önbelleği haline gelir ve doğrudan Redis istemcilerinden gelen istekleri kabul eder.

## <a name="next-steps"></a>Sonraki adımlar

[Redis Premium katmanı için Azure Önbelleği](cache-premium-tier-intro.md)hakkında daha fazla bilgi edinin.
