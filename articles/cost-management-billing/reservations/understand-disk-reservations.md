---
title: Rezervasyon indiriminin Azure Disk Depolama'ya nasıl uygulandığını anlama
description: Azure ayrılmış disk indiriminin premium SSD yönetilen disklerinize nasıl uygulandığını öğrenin.
author: roygara
ms.service: cost-management-billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/30/2020
ms.author: rogarana
ms.openlocfilehash: 7f23aaebc20b562768fb04ae988e4aff1b62fb21
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2020
ms.locfileid: "76902130"
---
# <a name="understand-how-your-reservation-discount-is-applied-to-azure-disk-storage"></a>Rezervasyon indiriminizin Azure Disk Depolama'ya nasıl uygulandığını anlama

Azure disk ayrılmış kapasitesi satın aldıktan sonra rezervasyon indirimi, rezervasyonunuzun dönemiyle eşleşen disk kaynaklarına otomatik olarak uygulanır. Rezervasyon indirimi yalnızca disk kapasitesine uygulanır; disk anlık görüntüleri kullandıkça öde fiyatları üzerinden ücretlendirilir.

Azure Disk rezervasyonu hakkında daha fazla bilgi için bkz. [Azure Disk rezervasyonu ile tasarruf etme](../../virtual-machines/linux/disks-reserved-capacity.md).
Azure Disk rezervasyonu fiyatları hakkında daha fazla bilgi için bkz. [Azure Disk fiyatlandırması](https://azure.microsoft.com/pricing/details/managed-disks/).

## <a name="how-the-reservation-discount-is-applied"></a>Rezervasyon indiriminin uygulanması

Azure disk rezervasyonu indirimi, kullanılmadığı takdirde hakkı kaybedilen bir indirimdir ve yönetilen disk kaynaklarına saatlik olarak uygulanır. Belirli bir saatte rezervasyon şartlarını karşılayan yönetilen disk kaynağınız yoksa o saat için rezervasyon indirimi uygulanmaz ve kullanılmayan saatler devredilmez.

Bir kaynağı sildiğinizde rezervasyon indirimi, belirtilen kapsamdaki başka bir eşleşen kaynağa otomatik olarak uygulanır. Belirtilen kapsamda eşleşen kaynak bulunamazsa, ayrılan saatler kaybedilir.

## <a name="discount-examples"></a>İndirim örnekleri

Aşağıdaki örneklerde, dağıtımlarınıza bağlı olarak Azure Disk rezervasyonu indiriminin nasıl uygulandığı gösterilmektedir:

1 yıllık bir dönem için ABD Batı 2 bölgesinde 100 P30 disk boyutunda (disk başına yaklaşık 1 TiB) ayrılmış kapasite satın aldığınızı düşünelim. Bu örnek rezervasyonun maliyetinin 140.100 ABD doları olduğunu kabul edelim. Tutarın tamamını peşin ödeyebilir veya sonraki 12 ay boyunca aylık 11.675 ABD doları taksitler halinde ödeme yapabilirsiniz.
Bu örneklerde aylık rezervasyon planına kaydolduğunuzu kabul edelim. Aşağıdaki senaryolarda, ayrılmış kapasitenizi daha az, daha fazla veya katmanlı olarak kullandığınızda karşılaşacağınız durumlar anlatılmıştır.

### <a name="underusing-your-capacity"></a>Kapasitenizi az kullanma

Rezervasyon döneminde belirli bir saat içinde 100 P30 disk rezervasyonunuzun yalnızca 99 P30 premium SSD kadarını dağıttığınızı düşünelim. Kalan 1 P30 o saate uygulanmaz ve devredilmez.

### <a name="overusing-your-capacity"></a>Kapasitenizi fazla kullanma

Rezervasyon döneminde belirli bir saat içinde 101 P30 premium SSD kullandığınızı düşünün. Rezervasyon indirimi yalnızca 100 P30 diske uygulanır ve kalan 1 P30 disk, o saat için geçerli olan kullandıkça öde fiyatları üzerinden ücretlendirilir. Bir sonraki saatte yapılan kullanım 100 P30 diskine düşerse tamamı rezervasyon tarafından karşılanır.

### <a name="tiering-your-capacity"></a>Kapasitenizi katmanlama

Rezervasyon döneminde belirli bir saat içinde toplam 200 P30 premium SSD kullanmak istediğinizi düşünelim. İlk 30 dakika boyunca yalnızca 100 tanesini kullandınız. Rezervasyonu 100 P30 disk için yaptığınızdan bu süre boyunca yaptığınız kullanımlar kapsam dahilinde olur. Daha sonra ilk 100 diski kullanmayı bırakırsanız (kullandığınız disk sayısı sıfıra inerse) ve kalan 30 dakika için 100 disk daha kullanmaya başlarsanız bu kullanım da rezervasyon kapsamına girer.

![Kapasite için az kullanım, fazla kullanım ve katmanlama örneklerinin gösterimi](media/understand-disk-reservations/reserved-disks-example-scenarios.png)

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bizimle iletişim kurun

Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Disk Rezervasyonu ile maliyetleri azaltın (Linux)](../../virtual-machines/linux/disks-reserved-capacity.md)
- [Azure Disk Rezervasyonu ile maliyetleri azaltın (Windows)](../../virtual-machines/windows/disks-reserved-capacity.md)
- [Azure Ayrılmış Sanal Makine Örnekleri nedir?](save-compute-costs-reservations.md)