---
title: Redis için Azure Önbelleği nasıl uygulanır?
description: Redis için Azure Önbelleği güncelleştirmelerini yeniden başlatma ve zamanlama güncelleştirmeleri gibi yönetim görevlerini nasıl gerçekleştireceğinizi öğrenin
author: yegu-ms
ms.service: cache
ms.topic: conceptual
ms.date: 07/05/2017
ms.author: yegu
ms.openlocfilehash: 69686cad20bc4ce70bff2a92a216c9430522c301
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79278850"
---
# <a name="how-to-administer-azure-cache-for-redis"></a>Redis için Azure Önbelleği nasıl uygulanır?
Bu konu, Redis örnekleri için Azure Önbelleği'niz için [güncelleştirmeleri yeniden başlatma](#reboot) ve [zamanlama](#schedule-updates) gibi yönetim görevlerinin nasıl gerçekleştirileceğini açıklar.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="reboot"></a>Yeniden başlatma
Yeniden Başlatma bıçağı önbelleğinizden bir veya daha fazla düğümü yeniden **başlatmanızı** sağlar. Bu yeniden başlatma özelliği, önbellek düğümünde bir hata varsa uygulamanızı esneklik açısından sınamamanızı sağlar.

![Yeniden başlatma](./media/cache-administration/redis-cache-administration-reboot.png)

Yeniden başlatmak için düğümleri seçin ve **Yeniden Başlat'ı**tıklatın.

![Yeniden başlatma](./media/cache-administration/redis-cache-reboot.png)

Kümeleme etkinleştirilmiş bir premium önbelleğiniz varsa, önbelleğin hangi kırıklarının yeniden başlatılacağını seçebilirsiniz.

![Yeniden başlatma](./media/cache-administration/redis-cache-reboot-cluster.png)

Önbelleğinizin bir veya daha fazla düğümünü yeniden başlatmak için, istenen düğümleri seçin ve **Yeniden Başlat'ı**tıklatın. Kümeleme etkin leştirilmiş bir premium önbelleğiniz varsa, yeniden başlatmak için istenen parçaları seçin ve ardından **Yeniden Başlat'ı**tıklatın. Birkaç dakika sonra, seçili düğümler yeniden başlatılır ve birkaç dakika sonra tekrar çevrimiçi duruma döner.

İstemci uygulamaları üzerindeki etkisi, yeniden başlatdığınız düğümlere bağlı olarak değişir.

* **Ana -** Ana düğüm yeniden başlatıldığında, Redis için Azure Önbelleği yineleme düğümünde başarısız olur ve ana olarak tanıtılır. Bu hata sırasında, bağlantıların önbellekte başarısız olabileceği kısa bir aralık olabilir.
* **Slave** - Köle düğümü yeniden başlatıldığında, önbellek istemcileri için genellikle hiçbir etkisi yoktur.
* **Hem ana hem de köle** - Her iki önbellek düğümü yeniden başlatıldığında, önbellekte tüm veriler kaybolur ve birincil düğüm çevrimiçi olana kadar önbelleğe bağlantılar başarısız olmaz. [Veri kalıcılığını](cache-how-to-premium-persistence.md)yapılandırmışsanız, önbellek yeniden çevrimiçi olduğunda en son yedekleme geri yüklenir, ancak en son yedekleme kaybolduktan sonra oluşan önbellek yazar.
* **Kümeleme etkinleştirilmiş bir premium önbelleğin düğümleri** - Kümeleme etkinleştirilmiş bir premium önbelleğin bir veya daha fazla düğümlerini yeniden başlattığınızda, seçili düğümlerin davranışı kümelenmiş olmayan bir önbelleğin karşılık gelen düğümveya düğümlerini yeniden başlattığınızdaki yle aynıdır.

## <a name="reboot-faq"></a>Yeniden Başlatma SSS
* [Uygulamamı test etmek için hangi düğümü yeniden başlatmalıyım?](#which-node-should-i-reboot-to-test-my-application)
* [İstemci bağlantılarını temizlemek için önbelleği yeniden başlatabilir miyim?](#can-i-reboot-the-cache-to-clear-client-connections)
* [Yeniden başlatma yaparsam önbelleğimdeki verileri kaybeder miyim?](#will-i-lose-data-from-my-cache-if-i-do-a-reboot)
* [PowerShell, CLI veya diğer yönetim araçlarını kullanarak önbelleğimi yeniden başlatabilir miyim?](#can-i-reboot-my-cache-using-powershell-cli-or-other-management-tools)

### <a name="which-node-should-i-reboot-to-test-my-application"></a>Uygulamamı test etmek için hangi düğümü yeniden başlatmalıyım?
Uygulamanızın esnekliğini önbelleğinizin birincil düğümünün başarısızlığına karşı sınamak için **Ana** düğümü yeniden başlatın. İkincil düğümün başarısızlığına karşı uygulamanızın dayanıklılığını test etmek için **Slave** düğümünün yeniden başlatılır. Uygulamanızın esnekliğini önbelleğin toplam hatasına karşı test etmek için **her iki** düğümü de yeniden başlatın.

### <a name="can-i-reboot-the-cache-to-clear-client-connections"></a>İstemci bağlantılarını temizlemek için önbelleği yeniden başlatabilir miyim?
Evet, önbelleği yeniden başlatırsanız tüm istemci bağlantıları temizlenir. Yeniden başlatma, istemci uygulamasındaki bir mantık hatası veya hata nedeniyle tüm istemci bağlantılarının kullanıldığı durumlarda yararlı olabilir. Her fiyatlandırma katmanı, çeşitli boyutlar için farklı [istemci bağlantı sınırlarına](cache-configure.md#default-redis-server-configuration) sahiptir ve bu sınırlara ulaşıldıktan sonra artık istemci bağlantısı kabul edilmez. Önbelleği yeniden başlatmak, tüm istemci bağlantılarını temizlemek için bir yol sağlar.

> [!IMPORTANT]
> İstemci bağlantılarını temizlemek için önbelleğinizi yeniden başlatırsanız, StackExchange.Redis Redis Redis düğümü yeniden çevrimiçi olduğunda otomatik olarak yeniden bağlanır. Temel sorun çözülmezse, istemci bağlantıları kullanılmaya devam edebilir.
> 
> 

### <a name="will-i-lose-data-from-my-cache-if-i-do-a-reboot"></a>Yeniden başlatma yaparsam önbelleğimdeki verileri kaybeder miyim?
Hem **Ana** hem de **Slave** düğümlerini yeniden yeniden başlatırsanız, önbellekteki tüm veriler (veya kümeleme etkinleştirilmiş bir premium önbellek kullanıyorsanız bu parçadaki) kaybolabilir, ancak bu da garanti edilmez. [Veri kalıcılığını](cache-how-to-premium-persistence.md)yapılandırmışsanız, önbellek yeniden çevrimiçi olduğunda en son yedekleme geri yüklenir, ancak yedekleme yapıldıktan sonra oluşan önbellek kaybolur.

Düğümlerden yalnızca birini yeniden yeniden başlatTıracak olursak, veriler genellikle kaybolmaz, ancak yine de kaybolabilir. Örneğin, ana düğüm yeniden başlatılırsa ve önbellek yazısı devam ediyorsa, önbellek yazılmasındaki veriler kaybolur. Veri kaybı için başka bir senaryo, bir düğümü yeniden başlatmanız ve diğer düğümün aynı anda bir hata nedeniyle inmesi olacaktır. Veri kaybının olası nedenleri hakkında daha fazla bilgi için [bkz.](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md)

### <a name="can-i-reboot-my-cache-using-powershell-cli-or-other-management-tools"></a>PowerShell, CLI veya diğer yönetim araçlarını kullanarak önbelleğimi yeniden başlatabilir miyim?
Evet, PowerShell yönergeleri [için bkz. Redis için bir Azure Önbelleği yeniden başlatın.](cache-how-to-manage-redis-cache-powershell.md#to-reboot-an-azure-cache-for-redis)

## <a name="schedule-updates"></a>Güncelleştirmeleri zamanlama
**Zamanlama güncelleştirmeleri** bıçağı, önbellek örneğiniz için bir bakım penceresi belirlemenize olanak tanır. Bakım penceresi belirtildiğinde, bu pencere sırasında redis sunucu güncelleştirmeleri yapılır. 

> [!NOTE] 
> Bakım penceresi yalnızca Redis sunucu güncelleştirmeleri için geçerlidir ve önbelleği barındıran VM'lerin işletim sistemindeki Azure güncelleştirmeleri veya güncelleştirmeleri için geçerli değildir.
>

![Güncelleştirmeleri zamanlama](./media/cache-administration/redis-schedule-updates.png)

Bakım penceresi belirtmek için, istenilen günleri kontrol edin ve her gün için bakım penceresi başlangıç saatini belirtin ve **Tamam'ı**tıklatın. Bakım penceresi süresinin UTC'de olduğunu unutmayın. 

Varsayılan ve güncelleştirmeler için en az bakım penceresi beş saattir. Bu değer Azure portalından yapılandırılamaz, ancak `MaintenanceWindow` [Yeni-AzRedisCacheScheduleEntry](/powershell/module/az.rediscache/new-azrediscachescheduleentry) cmdlet parametresini kullanarak PowerShell'de yapılandırabilirsiniz. Daha fazla bilgi için powershell, CLI veya diğer yönetim araçlarını kullanarak zamanlanmış güncelleştirmeleri yönetebilir miyim?

## <a name="schedule-updates-faq"></a>Güncelleştirmeleri Zamanlama SSS
* [Zamanlama güncelleştirmeleri özelliğini kullanmazsam güncelleştirmeler ne zaman oluşur?](#when-do-updates-occur-if-i-dont-use-the-schedule-updates-feature)
* [Zamanlanan bakım penceresinde ne tür güncelleştirmeler yapılır?](#what-type-of-updates-are-made-during-the-scheduled-maintenance-window)
* [PowerShell, CLI veya diğer yönetim araçlarını kullanarak zamanlanmış güncelleştirmeleri yönetebilir miyim?](#can-i-managed-scheduled-updates-using-powershell-cli-or-other-management-tools)

### <a name="when-do-updates-occur-if-i-dont-use-the-schedule-updates-feature"></a>Zamanlama güncelleştirmeleri özelliğini kullanmazsam güncelleştirmeler ne zaman oluşur?
Bir bakım penceresi belirtmezseniz, güncelleştirmeler herhangi bir zamanda yapılabilir.

### <a name="what-type-of-updates-are-made-during-the-scheduled-maintenance-window"></a>Zamanlanan bakım penceresinde ne tür güncelleştirmeler yapılır?
Yalnızca Redis sunucu güncelleştirmeleri zamanlanan bakım penceresi sırasında yapılır. Bakım penceresi Azure güncelleştirmeleri veya VM işletim sistemindeki güncelleştirmeler için geçerli değildir.

### <a name="can-i-managed-scheduled-updates-using-powershell-cli-or-other-management-tools"></a>PowerShell, CLI veya diğer yönetim araçlarını kullanarak zamanlanmış güncelleştirmeleri yönetebilir miyim?
Evet, zamanlanmış güncelleştirmelerinizi aşağıdaki PowerShell cmdlets'i kullanarak yönetebilirsiniz:

* [Get-AzRedisCachePatchSchedule](/powershell/module/az.rediscache/get-azrediscachepatchschedule)
* [Yeni-AzRedisCachePatchSchedule](/powershell/module/az.rediscache/new-azrediscachepatchschedule)
* [Yeni-AzRedisÖnbellekProgramGirişi](/powershell/module/az.rediscache/new-azrediscachescheduleentry)
* [Remove-AzRedisCachePatchSchedule](/powershell/module/az.rediscache/remove-azrediscachepatchschedule)

## <a name="next-steps"></a>Sonraki adımlar
* Redis premium katman özellikleri için daha fazla [Azure Önbelleği'ni](cache-premium-tier-intro.md) keşfedin.

