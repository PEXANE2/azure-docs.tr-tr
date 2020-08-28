---
title: Azure disk depolama için rezervasyon indirimlerini anlama
description: Azure ayrılmış disk indiriminin Azure premium SSD yönetilen disklerinize nasıl uygulandığını öğrenin.
author: roygara
ms.author: rogarana
ms.service: cost-management-billing
ms.subservice: reservations
ms.topic: conceptual
ms.date: 01/30/2020
ms.openlocfilehash: cd89c3ddc3c58de02f1104109ce7f243c4d1e6fd
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88682459"
---
# <a name="understand-how-your-reservation-discount-is-applied-to-azure-disk-storage"></a>Rezervasyon indiriminizin Azure Disk Depolama’ya nasıl uygulandığını anlama

Azure diskleriniz için ayrılmış kapasite satın aldığınızda rezervasyon indirimi, rezervasyonunuzun dönemiyle eşleşen disk kaynaklarına otomatik olarak uygulanır. Rezervasyon indirimi yalnızca disk SKU’larına uygulanır. Disk anlık görüntüleri kullandıkça öde fiyatlarıyla ücretlendirilir.

Azure Disk rezervasyonu hakkında daha fazla bilgi için bkz. [Azure Disk rezervasyonu ile tasarruf etme](../../virtual-machines/disks-reserved-capacity.md). Azure disk rezervasyonu fiyatları hakkında daha fazla bilgi için bkz. [Azure Yönetilen Disk fiyatlandırması](https://azure.microsoft.com/pricing/details/managed-disks/).

## <a name="how-the-reservation-discount-is-applied"></a>Rezervasyon indiriminin uygulanması

Azure disk rezervasyonu indirimini kullanmazsanız hakkınızı kaybedersiniz. Yönetilen disk kaynaklarına saatlik olarak uygulanır. Belirli bir saat için, rezervasyon koşullarını karşılayan yönetilen disk kaynaklarınız yoksa bu saate yönelik rezervasyon miktarını kaybedersiniz. Kullanılmayan saatler devredilemez.

Bir kaynağı sildiğinizde rezervasyon indirimi, belirtilen kapsamdaki başka bir eşleşen kaynağa otomatik olarak uygulanır. Eşleşen kaynak bulunmazsa ayrılan saatler kaybedilir.

## <a name="discount-examples"></a>İndirim örnekleri

Aşağıdaki örneklerde, Azure disk rezervasyonu indiriminin dağıtımınıza bağlı olarak nasıl uygulandığı gösterilmektedir.

Bir yıllığına ABD Batı 2 bölgesinde 100 tane P30 diski satın alıp ayırdığınızı varsayalım. Her disk yaklaşık 1 TiB depolama alanı sunar. Bu örnek rezervasyonun maliyetinin 140.100 ABD doları olduğunu düşünelim. Tutarın tamamını peşin ödeyebilir veya sonraki 12 ay boyunca 11.675 ABD doları tutarında sabit taksitler halinde ödeme yapabilirsiniz.

Aşağıdaki senaryolarda, ayrılmış kapasitenizi daha az, daha fazla veya katmanlı olarak kullandığınızda karşılaşacağınız durumlar açıklanır. Bu örneklerde, aylık rezervasyon planına kaydolduğunuzu varsayacağız.

### <a name="underusing-your-capacity"></a>Kapasitenizi az kullanma

Rezervasyon döneminde 100 adet ayrılmış Azure premium katı hal sürücüsü (SSD) P30 diskinizden yalnızca 99 tanesini bir saatliğine dağıttığınızı varsayalım. Dağıtılmayan P30 diski bu saat boyunca uygulanmaz. Aynı zamanda devredilemez.

### <a name="overusing-your-capacity"></a>Kapasitenizi fazla kullanma

Rezervasyon döneminde belirli bir saat boyunca 101 tane premium SSD P30 diski kullandığınızı varsayalım. Rezervasyon indirimi yalnızca 100 tane P30 diske uygulanır. Geri kalan P30 diskine bu saat için kullandıkça öde ücretleri uygulanır. Bir sonraki saatte yapılan kullanım 100 P30 diskine düşerse kullanımın tamamı rezervasyon tarafından karşılanır.

### <a name="tiering-your-capacity"></a>Kapasitenizi katmanlama

Rezervasyon döneminde belirli bir saat içinde toplam 200 tane P30 premium SSD kullanmak istediğinizi varsayalım. Ayrıca, ilk 30 dakika boyunca yalnızca 100 tanesini kullandığınızı varsayalım. Rezervasyonu 100 P30 disk için yaptığınızdan bu süre boyunca yaptığınız kullanımlar kapsam dahilinde olur. Daha sonra ilk 100 diski kullanmayı bırakırsanız (kullandığınız disk sayısı sıfıra inerse) ve kalan 30 dakika için 100 disk daha kullanmaya başlarsanız bu kullanım da rezervasyon kapsamına girer.

![Kapasiteye yönelik az kullanım, fazla kullanım ve katmanlama örnekleri](media/understand-disk-reservations/reserved-disks-example-scenarios.png)

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bizimle iletişim kurun

Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Disk Rezervasyonu ile maliyetleri azaltın](../../virtual-machines/disks-reserved-capacity.md)
- [Azure Ayrılmış Sanal Makine Örnekleri nedir?](save-compute-costs-reservations.md)
