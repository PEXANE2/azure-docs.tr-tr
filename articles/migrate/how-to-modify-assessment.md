---
title: Azure Geçir Sunucu Değerlendirmesi için değerlendirmeleri özelleştirin | Microsoft Dokümanlar
description: Azure Geçiş Sunucusu Değerlendirmesi ile oluşturulan değerlendirmelerin nasıl özelleştirilebildiğini açıklar
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 07/15/2019
ms.author: raynew
ms.openlocfilehash: 2cfac978b0a5af20e9e2fa1e32a7361488f20fbe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68234280"
---
# <a name="customize-an-assessment"></a>Bir değerlendirmeyi özelleştirme

Bu makalede, Azure Geçir Sunucusu Değerlendirmesi tarafından oluşturulan değerlendirmelerin nasıl özelleştirilen bir şekilde özelleştirilen açıklanmaktadır.

[Azure Geçiş,](migrate-services-overview.md) şirket içi uygulamalarınızın ve iş yüklerinizin ve özel/genel bulut VM'lerinizin Azure'a keşfini, değerlendirmesini ve geçişini izlemek için merkezi bir hub sağlar. Hub, değerlendirme ve geçiş için Azure Geçiş araçlarının yanı sıra üçüncü taraf bağımsız yazılım satıcısı (ISV) teklifleri sağlar.

Azure'a geçişe hazırlık amacıyla şirket içi VMware VM'ler ve Hyper-V V'ler için değerlendirmeler oluşturmak için Azure Geçir Sunucu Değerlendirmesi aracını kullanabilirsiniz.

## <a name="about-assessments"></a>Değerlendirmeler hakkında

Azure Geçir Sunucu Değerlendirmesi'ni kullanarak çalıştırabileceğiniz iki tür değerlendirme vardır.

**Değerlendirme** | **Şey** | **Veri**
--- | --- | ---
**Performansa dayalı** | Toplanan performans verilerine dayalı değerlendirmeler | **Önerilen VM boyutu**: CPU ve bellek kullanım verilerine dayanır.<br/><br/> **Önerilen disk türü (standart veya premium yönetilen disk)**: IOPS'ye ve şirket içi disklerin üretim emresine dayanır.
**Şirket içinde olduğu gibi** | Şirket içi boyutlandırmaya dayalı değerlendirmeler. | **Önerilen VM boyutu**: Şirket içi VM boyutuna göre<br/><br> **Önerilen disk türü**: Değerlendirme için seçtiğiniz depolama türü ayarına göre.


## <a name="how-is-an-assessment-done"></a>Değerlendirme nasıl yapılır?

Azure Geçir Sunucu Değerlendirmesi'nde yapılan bir değerlendirmenin üç aşaması vardır. Değerlendirme bir uygunluk analizi ile başlar, ardından boyutlandırma ve son olarak, bir aylık maliyet tahmini. Bir makine yalnızca bir öncekinden geçerse daha sonraki bir aşamaya geçer. Örneğin, bir makine Azure uygunluk denetiminde başarısız olursa, Azure için uygun olmadığı işaretlenir ve boyutlandırma ve maliyetlendirme yapılmaz. [Daha fazla bilgi edinin.](https://docs.microsoft.com/azure/migrate/concepts-assessment-calculation)

## <a name="whats-in-an-assessment"></a>Bir değerlendirme neleri içerir?

**Özellik** | **Şey**
--- | ---
**Hedef konum** | Geçişi yapmak istediğiniz Azure konumu.<br/> Sunucu Değerlendirmesi şu anda bu hedef bölgeleri destekler: Avustralya Doğu, Avustralya Güneydoğu, Brezilya Güney, Kanada Orta, Kanada Doğu, Orta Hindistan, Orta ABD, Çin Doğu, Çin Kuzey, Doğu Asya, Doğu ABD, Doğu US2, Almanya Orta, Almanya Kuzeydoğu, Japonya Doğu, Japonya Batı, Kore Merkez, Kore Güney, Kuzey Orta ABD, Kuzey Avrupa, Güney Orta ABD, Güneydoğu Asya, Güney Hindistan, İngiltere Güney, İngiltere Batı, ABD Gov Arizona, ABD Gov Texas, ABD Gov Virginia, Batı Orta ABD, Batı Avrupa, Batı Hindistan, Batı ABD ve Batı US2.
**Depolama türü** | Bu özelliği, Azure'da taşımak istediğiniz disk türünü belirtmek için kullanabilirsiniz.<br/><br/> Şirket içi boyutlandırma için hedef depolama türünü Premium yönetilen diskler, Standart SSD tarafından yönetilen diskler veya Standart HDD tarafından yönetilen diskler olarak belirtebilirsiniz. Performans tabanlı boyutlandırma için hedef disk türünü Otomatik, Premium yönetilen diskler, Standart HDD tarafından yönetilen diskler veya Standart SSD tarafından yönetilen diskler olarak belirtebilirsiniz.<br/><br/> Depolama türünü otomatik olarak belirttiğiniz zaman, disk önerisi disklerin performans verilerine (IOPS ve iş bölümü) göre yapılır. Depolama türünü premium/standart olarak belirtirseniz, değerlendirme, seçilen depolama türü içinde bir disk SKU önerir. %99,9'luk tek bir VM SLA elde etmek istiyorsanız, depolama türünü Premium yönetilen diskler olarak belirtmek isteyebilirsiniz. Bu, değerlendirmedeki tüm disklerin Premium yönetilen diskler olarak tavsiye edilmesini sağlar. Azure
**Ayrılmış Örnekler (RI)** | Bu özellik, Azure'da [Ayrılmış Örnekler](https://azure.microsoft.com/pricing/reserved-vm-instances/) varsa, değerlendirmedeki maliyet tahminlerinin RI indirimleri dikkate alınarak yapıldığını belirtmenize yardımcı olur. Rezerve edilen örnekler şu anda yalnızca Azure Geçiş'te Yalnızca Sizden Önce Öde teklifi için desteklenir.
**Boyutlandırma ölçütü** | Azure için sağ boyutlu VM'lerde kullanılacak ölçüt. *Performans tabanlı* boyutlandırma yapabilir veya performans geçmişini düşünmeden VM'leri şirket *içi olarak*boyutlandırabilirsiniz.
**Performans geçmişi** | Makinelerin performans verilerinin değerlendirilmesi nde göz önünde bulundurulması gereken süre. Bu özellik yalnızca boyutlandırma ölçütü *performansa dayalı*olduğunda uygulanabilir.
**Yüzdebirlik kullanımı** | Doğru boyutlandırma için dikkate alınacak performans örnek kümesinin yüzdebirlik değeri. Bu özellik yalnızca boyutlandırma *performansa dayalı*olduğunda uygulanabilir.
**VM serisi** |     Uygun boyutlandırma için değerlendirmek istediğiniz bir VM serisini belirtebilirsiniz. Örneğin, Azure'da A serisi VM'lere geçiş yapmayı planlamadığınız bir üretim ortamınız varsa, A serisini listeden veya seriden hariç tutabilirsiniz ve sağ boyutlandırma yalnızca seçili serilerde yapılır.
**Konfor katsayısı** | Azure Geçir Sunucu Değerlendirmesi, değerlendirme sırasında bir arabellek (konfor faktörü) olarak kabul eder. Bu tampon, VM’lerin makine kullanım verilerinin (CPU, bellek, disk ve ağ) üzerine uygulanır. Konfor katsayısı; sezona özgü kullanım, kısa performans geçmişi ve gelecek kullanımlarda oluşabilecek artışlar gibi konuları hesaba katar.<br/><br/> Örneğin, %20 kullanıma sahip 10 çekirdekli bir VM normalde 2 çekirdekli VM ile sonuçlanır. Ancak, 2.0x konfor katsayısı ile sonuç 4 çekirdekli VM olur.
**Teklif** | Kaydolduğunuz [Azure teklifi](https://azure.microsoft.com/support/legal/offer-details/). Azure Geçişi, buna göre bir maliyet tahmini oluşturur.
**Para birimi** | Fatura para birimi.
**İndirim (%)** | Azure teklifinin yanı sıra aldığınız, aboneliğe özgü indirim.<br/> Varsayılan ayar, %0’dır.
**VM çalışma süresi** | VM'leriniz Azure'da 24x7 çalışmayacaksa, çalıştıkları süreyi (aylık gün sayısı ve günlük saat sayısı) belirtebilirsiniz ve maliyet tahminleri buna göre yapılır.<br/> Varsayılan değer ayda 31 gün ve günde 24 saattir.
**Azure Hibrit Avantajı** | Yazılım güvenceniz olup olmadığını ve [Azure Karma Avantajı](https://azure.microsoft.com/pricing/hybrid-use-benefit/)için uygun olup olmadığını belirtin. Evet olarak ayarlanırsa, Windows sanal makineleri için Windows olmayan Azure fiyatları dikkate alınır. Varsayılan değer Evet’tir.


## <a name="edit-assessment-properties"></a>Değerlendirme özelliklerini edin

Değerlendirme yi oluşturduktan sonra değerlendirme özelliklerini yeniden değerlendirmek için aşağıdakileri yapın:

1. Azure Geçir projesinde **Sunucular'ı**tıklatın.
2. **Azure Geçir: Sunucu Değerlendirmesi'nde**değerlendirmeler sayısına tıklayın.
3. **Değerlendirmede,** **özellikleri >** ilgili değerlendirmeyi tıklatın.
5. Değerlendirme özelliklerini yukarıdaki tabloya göre özelleştirin.
6. Değerlendirmeyi güncellemek için **Kaydet'i** tıklatın.


Değerlendirme oluştururken değerlendirme özelliklerini de edinebilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

Değerlendirmelerin nasıl hesaplandığı hakkında [daha fazla bilgi](concepts-assessment-calculation.md) edinin.
