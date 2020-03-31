---
title: Azure HPC Önbelleğini yönetme ve güncelleştirme
description: Azure portalını kullanarak Azure HPC Önbelleğini yönetme ve güncelleştirme
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 1/29/2020
ms.author: rohogue
ms.openlocfilehash: da260074fc69fac9e98d3698bb2d40fdf80d7118
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77252051"
---
# <a name="manage-your-cache-from-the-azure-portal"></a>Önbelleğinizi Azure portalından yönetme

Azure portalındaki önbelleğe genel bakış sayfası, önbelleğinizin proje ayrıntılarını, önbellek durumunu ve temel istatistiklerini gösterir. Ayrıca önbelleği durdurmak veya başlatmak, önbelleği silmek, verileri uzun süreli depolamaya yıkamak ve yazılımı güncelleştirmek için denetimlere sahiptir.

Genel bakış sayfasını açmak için Azure portalındaki önbellek kaynağınızı seçin. Örneğin, Tüm **kaynaklar** sayfasını yükleyin ve önbellek adını tıklatın.

![Azure HPC Önbellek örneğinin Genel Bakış sayfasının ekran görüntüsü](media/hpc-cache-overview.png)

Sayfanın üst kısmındaki düğmeler önbelleği yönetmenize yardımcı olabilir:

* **Başlat** ve [**Durdur**](#stop-the-cache) - Önbellek işlemini askıya aldı
* [**Flush**](#flush-cached-data) - Değiştirilen verileri depolama hedeflerine yazar
* [**Yükseltme**](#upgrade-cache-software) - Önbellek yazılımını güncelleştirir
* **Yenile** - Genel bakış sayfasını yeniden yükler
* [**Sil**](#delete-the-cache) - Önbelleği kalıcı olarak yok eder

Aşağıdaki bu seçenekler hakkında daha fazla bilgi edinin.

## <a name="stop-the-cache"></a>Önbelleği durdur

Etkin olmayan bir dönemde maliyetleri azaltmak için önbelleği durdurabilirsiniz. Önbellek durdurulurken çalışma süresi için ücretlendirilmezsiniz, ancak önbelleğin ayrılan disk depolama alanı için ücretlendirilirsiniz. (Ayrıntılar için [fiyatlandırma](https://aka.ms/hpc-cache-pricing) sayfasına bakın.)

Durdurulan önbellek istemci isteklerine yanıt vermez. Önbelleği durdurmadan önce istemcilerin montajını kesmelisiniz.

**Durdur** düğmesi etkin bir önbelleği askıya aldı. Önbelleğin durumu **Sağlıklı** veya **Bozulmuş**olduğunda **Durdur** düğmesi kullanılabilir.

![Stop vurgulanan üst düğmelerin ekran görüntüsü ve durdurma eylemini açıklayan ve 'devam etmek istiyor musunuz?' diye soran bir açılır mesaj Evet (varsayılan) ve Hayır düğmeleri ile](media/stop-cache.png)

Önbelleği durdurmak için Evet'i tıklattıktan sonra, önbellek içeriğini otomatik olarak depolama hedeflerine doğru alar. Bu işlem biraz zaman alabilir, ancak veri tutarlılığı sağlar. Son olarak, önbellek durumu **Durduruldu'da**değişir.

Durdurulan önbelleği yeniden etkinleştirmek için **Başlat** düğmesini tıklatın. Onay gerekmez.

![Start vurgulanan üst düğmelerin ekran görüntüsü](media/start-cache.png)

## <a name="flush-cached-data"></a>Önbelleğe alınan verileri temizle

Genel bakış sayfasındaki **Flush** düğmesi, önbelleğe alınan tüm değiştirilen verileri arka uç depolama hedeflerine hemen yazmasını söyler. Önbellek düzenli olarak depolama hedeflerine veri kaydeder, bu nedenle arka uç depolama sisteminin güncel olduğundan emin olmak istemiyorsanız bunu el ile yapmanız gerekmez. Örneğin, bir depolama anlık görüntüsü çekmeden veya veri kümesi boyutunu denetlemeden önce **Flush'ı** kullanabilirsiniz.

> [!NOTE]
> Temizleme işlemi sırasında önbellek istemci isteklerini suadresinden karşılar. Önbellek erişimi askıya alınır ve işlem bittikten sonra devam eder.

![Flush vurgulanan üst düğmeleri ekran görüntüsü ve floş eylem açıklayan ve 'devam etmek istiyor musunuz?' soran bir pop-up mesajı Evet (varsayılan) ve Hayır düğmeleri ile](media/hpc-cache-flush.png)

Önbellek temizleme işlemini başlattığınızda, önbellek istemci isteklerini kabul etmeyi durdurur ve genel bakış sayfasındaki önbellek durumu **Flushing'e**dönüşür.

Önbellekteki veriler uygun depolama hedeflerine kaydedilir. Ne kadar veri temizlenmesi gerektiğine bağlı olarak, işlem birkaç dakika veya bir saatten fazla sürebilir.

Tüm veriler depolama hedeflerine kaydedildikten sonra, önbellek otomatik olarak istemci isteklerini yeniden almaya başlar. Önbellek durumu **Sağlıklı'ya**döndürür.

## <a name="upgrade-cache-software"></a>Önbellek yazılımLarını yükseltme

Yeni bir yazılım sürümü varsa, **Yükseltme** düğmesi etkin hale gelir. Ayrıca, sayfanın üst kısmında yazılımı güncelleştirmeyle ilgili bir ileti de görmeniz gerekir.

![Yükseltme düğmesi etkin leştirilmiş düğmelerin üst satırının ekran görüntüsü](media/hpc-cache-upgrade-button.png)

İstemci erişimi bir yazılım yükseltmesi sırasında kesintiye uğramaz, ancak önbellek performansı yavaşlar. Yazılımı yoğun olmayan kullanım saatlerinde veya planlanan bakım döneminde yükseltmeyi planlayın.

Yazılım güncelleştirmesi birkaç saat sürebilir. Daha yüksek iş elde lerle yapılandırılan önbelleklerin yükseltilemi, daha küçük tepe çıkış değerlerine sahip önbelleklere göre daha uzun sürer.

Bir yazılım yükseltmesi kullanılabilir olduğunda, bunu el ile uygulamak için bir haftanız veya daha fazla nız olacaktır. Bitiş tarihi yükseltme iletisinde listelenir. Bu süre içinde yükseltme yapmazsanız, Azure güncelleştirmeyi otomatik olarak önbelleğinize uygular. Otomatik yükseltmenin zamanlaması yapılandırılamaz. Önbellek performansı etkisi yle ilgili endişeleriniz varsa, süre dolmadan önce yazılımı kendiniz yükseltmeniz gerekir.

Önbelleğiniz bitiş tarihi geçtiğinde durdurulursa, önbellek yazılımı bir sonraki başlatıldığında otomatik olarak yükseltir. (Güncelleştirme hemen başlatılamayabilir, ancak ilk saat içinde başlar.)

Yazılım güncelleştirmesine başlamak için **Yükseltme** düğmesini tıklatın. Önbellek durumu, işlem tamamlanana kadar **Yükseltme** olarak değişir.

## <a name="delete-the-cache"></a>Önbelleği silme

**Sil** düğmesi önbelleği yok eder. Bir önbelleği sildiğinizde, tüm kaynakları yok edilir ve artık hesap ücretlerine tabi değildir.

Önbelleği sildiğinizde depolama hedefi olarak kullanılan arka uç depolama birimleri etkilenmez. Bunları daha sonra gelecekteki bir önbelleğe ekleyebilir veya ayrı ayrı devre dışı bırakabilirsiniz.

> [!NOTE]
> Azure HPC Önbelleği, önbelleği silmeden önce değiştirilen verileri otomatik olarak önbellekten arka uç depolama sistemlerine yazmaz.
>
> Önbellekteki tüm verilerin uzun süreli depolama alanına yazıldığından emin olmak için, önbelleği silmeden önce [durdurun.](#stop-the-cache) Sil düğmesini tıklatmadan önce **Durduruldu** durumunu gösterdiğinden emin olun.
<!--... written to long-term storage, follow this procedure:
>
> 1. [Remove](hpc-cache-edit-storage.md#remove-a-storage-target) each storage target from the Azure HPC Cache by using the delete button on the Storage targets page. The system automatically writes any changed data from the cache to the back-end storage system before removing the target.
> 1. Wait for the storage target to be completely removed. The process can take an hour or longer if there is a lot of data to write from the cache. When it is done, a portal notification says that the delete operation was successful, and the storage target disappears from the list.
> 1. After all affected storage targets have been deleted, it is safe to delete the cache.
>
> Alternatively, you can use the [flush](#flush-cached-data) option to save cached data, but there is a small risk of losing work if a client writes a change to the cache after the flush completes but before the cache instance is destroyed.-->

## <a name="cache-metrics-and-monitoring"></a>Önbellek ölçümleri ve izleme

Genel bakış sayfası, bazı temel önbellek istatistiklerinin grafiklerini gösterir - önbellek çıktısı, saniyebaşına işlemler ve gecikme.

![örnek bir önbellek için yukarıda belirtilen istatistikleri gösteren üç satır grafiğiekran görüntüsü](media/hpc-cache-overview-stats.png)

Bu grafikler, Azure'un yerleşik izleme ve analiz araçlarının bir parçasıdır. Portal kenar çubuğundaki **İzleme** başlığıaltındaki sayfalarda ek araçlar ve uyarılar mevcuttur. [Azure İzleme belgelerinin](../azure-monitor/insights/monitor-azure-resource.md#monitoring-in-the-azure-portal)portal bölümündedaha fazla bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

<!-- * Learn more about metrics and statistics for hpc cache -->
* [Azure ölçümleri ve istatistik araçları](../azure-monitor/index.yml) hakkında daha fazla bilgi edinin
* [Azure HPC Önbelleği ile ilgili yardım](hpc-cache-support-ticket.md) alın
