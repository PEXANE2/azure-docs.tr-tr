---
title: Azure HPC önbelleğini yönetme ve güncelleştirme
description: Azure portal kullanarak Azure HPC önbelleğini yönetme ve güncelleştirme
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 1/29/2020
ms.author: rohogue
ms.openlocfilehash: da260074fc69fac9e98d3698bb2d40fdf80d7118
ms.sourcegitcommit: 79cbd20a86cd6f516acc3912d973aef7bf8c66e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2020
ms.locfileid: "77252051"
---
# <a name="manage-your-cache-from-the-azure-portal"></a>Azure portal önbelleğinizi yönetin

Azure portal önbellekte Genel Bakış sayfasında, önbelleğiniz için proje ayrıntıları, önbellek durumu ve temel istatistikler gösterilmektedir. Ayrıca, önbelleği durdurmak veya başlatmak, önbelleği silmek, uzun süreli depolamaya verileri temizlemek ve yazılımı güncelleştirmek için denetimler de vardır.

Genel Bakış sayfasını açmak için Azure portal ön belleği kaynağını seçin. Örneğin, **tüm kaynaklar** sayfasını yükleyin ve önbellek adına tıklayın.

![Azure HPC önbellek örneğinin genel bakış sayfasının ekran görüntüsü](media/hpc-cache-overview.png)

Sayfanın üst kısmındaki düğmeler, önbelleği yönetmenize yardımcı olabilir:

* **Başlatma** ve [**durdurma**](#stop-the-cache) -önbellek işlemini askıya al
* [**Flush**](#flush-cached-data) -değiştirilen verileri depolama hedeflerine yazar
* [**Yükseltme**](#upgrade-cache-software) -önbellek yazılımını güncelleştirir
* **Yenile** -genel bakış sayfasını yeniden yükler
* [**Sil**](#delete-the-cache) -önbelleği kalıcı olarak yok eder

Aşağıdaki seçenekler hakkında daha fazla bilgi edinin.

## <a name="stop-the-cache"></a>Önbelleği durdur

Etkin olmayan bir süre boyunca maliyetleri azaltmak için önbelleğin durdurulmasını sağlayabilirsiniz. Önbellek durdurulduğunda çalışma süresi ücretsizdir, ancak önbelleğin ayrılan disk depolaması için ücretlendirilirsiniz. (Ayrıntılar için [fiyatlandırma](https://aka.ms/hpc-cache-pricing) sayfasına bakın.)

Durdurulmuş bir önbellek, istemci isteklerine yanıt vermez. Önbelleği durdurmadan önce istemcilerin bağlantısını çıkarmanız gerekir.

**Durdur** düğmesi etkin bir önbelleği askıya alır. Önbelleğin durumu **sağlıklı** veya **düşürülmüş**olduğunda **Durdur** düğmesi kullanılabilir.

![Durdur ile üst düğmelerin ve Durdur eylemini açıklayan ve ' devam etmek istiyor musunuz? ' adlı bir açılan iletinin ekran görüntüsü Evet (varsayılan) ve düğme yok](media/stop-cache.png)

Önbelleği durdurmayı onaylamak için Evet ' e tıkladıktan sonra önbellek, içeriğini depolama hedeflerine otomatik olarak temizler. Bu işlem biraz zaman alabilir, ancak veri tutarlılığını sağlar. Son olarak, önbellek durumu **durduruldu**olarak değişir.

Durdurulmuş bir önbelleği yeniden etkinleştirmek için **Başlat** düğmesine tıklayın. Onay gerekmez.

![vurgulanacak üst düğmelerin ekran görüntüsü](media/start-cache.png)

## <a name="flush-cached-data"></a>Önbelleğe alınmış verileri temizleme

Genel Bakış sayfasındaki **Temizleme** düğmesi önbelleğin önbellekte depolanan tüm değiştirilen verileri arka uç depolama hedeflerine hemen yazmasını söyler. Önbellek verileri düzenli aralıklarla depolama hedeflerine kaydeder, bu nedenle arka uç depolama sisteminin güncel olduğundan emin olmak istemediğiniz takdirde bunu el ile yapmak gerekli değildir. Örneğin, bir depolama anlık görüntüsü almadan veya veri kümesi boyutunu kontrol etmeden önce **temizlemeyi** kullanabilirsiniz.

> [!NOTE]
> Temizleme işlemi sırasında önbellek, istemci isteklerine yönelik hizmeti veremez. Önbellek erişimi askıya alınır ve işlem bittikten sonra devam eder.

![Flush vurgulanmış olan üst düğmelerin ve Temizleme eylemini açıklayan ve ' devam etmek istiyor musunuz? ' adlı bir açılan iletinin ekran görüntüsü Evet (varsayılan) ve düğme yok](media/hpc-cache-flush.png)

Önbellek temizleme işlemini başlattığınızda, önbellek istemci isteklerini kabul etmeyi durduruyor ve Genel Bakış sayfasındaki önbellek durumu **reçeteye göre**değişir.

Önbellekteki veriler uygun depolama hedeflerine kaydedilir. Ne kadar veri temizlenmesi gerektiğine bağlı olarak, işlem birkaç dakika sürebilir veya bir saat boyunca zaman alabilir.

Tüm veriler depolama hedeflerine kaydedildikten sonra, önbellek otomatik olarak istemci istekleri almaya başlar. Önbellek durumu **sağlıklı**olarak döner.

## <a name="upgrade-cache-software"></a>Yükseltme önbelleği yazılımı

Yeni bir yazılım sürümü varsa, **Yükselt** düğmesi etkin hale gelir. Ayrıca, sayfanın en üstünde yazılım güncelleştirme hakkında bir ileti de görmeniz gerekir.

![Yükseltme düğmesi etkin olan düğmelerin en üstteki satır görüntüsü](media/hpc-cache-upgrade-button.png)

İstemci erişimi, yazılım yükseltmesi sırasında kesintiye uğramaz, ancak önbellek performansı yavaşlar. Yoğun olmayan kullanım saatlerinde veya planlanan bir bakım döneminde yazılım yükseltmeyi planlayın.

Yazılım güncelleştirmesi birkaç saat sürebilir. Daha yüksek aktarım hızı ile yapılandırılan önbellekler, daha az yoğun üretilen iş değeri olan önbellekler daha uzun sürer

Bir yazılım yükseltmesi kullanılabilir olduğunda, bir hafta veya el ile uygulamanız gerekir. Son Tarih yükseltme iletisinde listelenir. Bu süre boyunca yükseltmezseniz Azure, güncelleştirmeyi otomatik olarak önbelleğe uygular. Otomatik yükseltmenin zamanlaması yapılandırılamaz. Önbellek performansı etkisi konusunda endişeleriniz varsa, zaman döneminin süresi dolmadan önce yazılımı kendiniz yükseltmeniz gerekir.

Bitiş tarihi geçtiğinde önbelleğiniz durdurulmuşsa, önbellek bir sonraki başlatılışında otomatik olarak yazılım yükseltir. (Güncelleştirme hemen başlatılamayabilir, ancak ilk saat içinde başlatılacaktır.)

Yazılım güncelleştirmesini başlatmak için **Yükselt** düğmesine tıklayın. İşlem tamamlanana kadar önbellek durumu **yükseltme** olarak değişir.

## <a name="delete-the-cache"></a>Önbelleği silme

**Sil** düğmesi önbelleği yok eder. Bir önbelleği sildiğinizde, tüm kaynakları yok edilir ve artık hesap ücretlerine tabi değildir.

Depolama hedefleri olarak kullanılan arka uç depolama birimleri, önbelleği sildiğinizde etkilenmez. Daha sonra gelecekteki bir önbelleğe ekleyebilir veya onları ayrı olarak yetkisini alabilirsiniz.

> [!NOTE]
> Azure HPC önbelleği, önbellek silinmeden önce, önbellekteki verileri otomatik olarak arka uç depolama sistemlerine yazar.
>
> Önbellekteki tüm verilerin uzun süreli depolamaya yazıldığından emin olmak için, silmeden önce [önbelleği durdurun](#stop-the-cache) . Sil düğmesine tıklamadan önce **durdurulan** durumu gösteriyor olduğundan emin olun.
<!--... written to long-term storage, follow this procedure:
>
> 1. [Remove](hpc-cache-edit-storage.md#remove-a-storage-target) each storage target from the Azure HPC Cache by using the delete button on the Storage targets page. The system automatically writes any changed data from the cache to the back-end storage system before removing the target.
> 1. Wait for the storage target to be completely removed. The process can take an hour or longer if there is a lot of data to write from the cache. When it is done, a portal notification says that the delete operation was successful, and the storage target disappears from the list.
> 1. After all affected storage targets have been deleted, it is safe to delete the cache.
>
> Alternatively, you can use the [flush](#flush-cached-data) option to save cached data, but there is a small risk of losing work if a client writes a change to the cache after the flush completes but before the cache instance is destroyed.-->

## <a name="cache-metrics-and-monitoring"></a>Önbellek ölçümleri ve izleme

Genel Bakış sayfasında bazı temel önbellek istatistikleri için grafikler, önbellek aktarım hızı, saniye başına işlem ve gecikme gösterilmektedir.

![Örnek önbellek için yukarıda bahsedilen istatistikleri gösteren üç satırlık grafiklerin ekran görüntüsü](media/hpc-cache-overview-stats.png)

Bu grafikler, Azure 'un yerleşik izleme ve analiz araçlarının bir parçasıdır. Ek araçlar ve uyarılar, Portal kenar çubuğu 'ndaki **izleme** başlığı altındaki sayfalardan kullanılabilir. [Azure izleme belgelerinin](../azure-monitor/insights/monitor-azure-resource.md#monitoring-in-the-azure-portal)Portal bölümünde daha fazla bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

<!-- * Learn more about metrics and statistics for hpc cache -->
* [Azure ölçümleri ve istatistik araçları](../azure-monitor/index.yml) hakkında daha fazla bilgi edinin
* [Azure HPC önbelleğiniz ile ilgili yardım](hpc-cache-support-ticket.md) alın
