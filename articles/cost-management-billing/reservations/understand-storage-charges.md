---
title: Rezervasyon indiriminin Azure Depolama'ya nasıl uygulandığını anlama | Microsoft Docs
description: Azure Depolama ayrılmış kapasitesi indiriminin blok blobu ve Azure Data Lake Storage 2. Nesil kaynaklarına nasıl uygulandığını öğrenin.
author: tamram
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 11/05/2019
ms.author: tamram
ms.openlocfilehash: 40fba61b173979fb9362de73a87d6192aac9941d
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "75995318"
---
# <a name="understand-how-the-reservation-discount-is-applied-to-azure-storage"></a>Rezervasyon indiriminin Azure Depolama'ya nasıl uygulandığını anlama

Azure Depolama ayrılmış kapasitesi satın aldıktan sonra rezervasyon indirimi rezervasyon dönemiyle eşleşen blok blobu ve Azure Data Lake Storage 2. Nesil kaynaklarına otomatik olarak uygulanır. Rezervasyon indirimi yalnızca depolama kapasitesine uygulanır. Bant genişliği ve istek oranı, kullandıkça öde fiyatları üzerinden ücretlendirilir.

Azure Depolama ayrılmış kapasitesi hakkında daha fazla bilgi için bkz. [Ayrılmış kapasite ile Blob depolama maliyetlerini iyileştirme](../../storage/blobs/storage-blob-reserved-capacity.md).

Azure Depolama ayrılmış kapasitesi fiyatları hakkında daha fazla bilgi için bkz. [Blok blobu fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/blobs/) ve [Azure Data Lake Storage 2. Nesil fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/data-lake/).

## <a name="how-the-reservation-discount-is-applied"></a>Rezervasyon indiriminin uygulanması

Azure Depolama ayrılmış kapasitesi indirimi, blok blobu ve Azure Data Lake Storage 2. Nesil kaynaklarına saatlik olarak uygulanır.

Azure Depolama ayrılmış kapasitesi indirimi "kullanılmadığı takdirde hakkı kaybedilen" bir indirimdir. Belirli bir saatte rezervasyon şartlarını karşılayan blok blobu veya Azure Data Lake Storage 2. Nesil kaynağınız yoksa o saat için rezervasyon indirimi uygulanmaz. Kullanılmayan ayrılmış saatleri devredemezsiniz.

Bir kaynağı sildiğinizde rezervasyon indirimi, belirtilen kapsamdaki başka bir eşleşen kaynağa otomatik olarak uygulanır. Belirtilen kapsamda eşleşen kaynak bulunamazsa, ayrılan saatler kaybedilir.

## <a name="discount-examples"></a>İndirim örnekleri

Aşağıdaki örneklerde, dağıtımlara bağlı olarak Azure Depolama ayrılmış kapasite indiriminin nasıl uygulandığı gösterilmektedir.

1 yıllık bir dönem için ABD Batı 2 bölgesinde 100 TB ayrılmış kapasite satın aldığınızı düşünelim. Rezervasyonunuz, sık erişim katmanındaki yerel olarak yedekli depolamayı (LRS) kapsıyor.

Bu örnek rezervasyonun maliyetinin 18.540 ABD doları olduğunu düşünelim. Tutarın tamamını peşin ödeyebilir veya sonraki 12 ay boyunca aylık 1.545 ABD doları taksitler halinde ödeme yapabilirsiniz.

Bu örneklerde aylık rezervasyon planına kaydolduğunuzu kabul edelim. Aşağıdaki senaryolarda, ayrılmış kapasitenizi az veya fazla kullandığınızda karşılaşacağınız durumlar anlatılmıştır.

### <a name="underusing-your-capacity"></a>Kapasitenizi az kullanma

Rezervasyon döneminde belirli bir saat içinde 100 TB ayrılmış kapasitenizin yalnızca 80 TB kadarını kullandığınızı düşünün. Kalan 20 TB o saate uygulanmaz ve devredilmez.

### <a name="overusing-your-capacity"></a>Kapasitenizi fazla kullanma

Rezervasyon döneminde belirli bir saat içinde 100 TB ayrılmış kapasitenizin 101 TB kadarını kullandığınızı düşünün. Rezervasyon indirimi 100 TB veriye uygulanır ve kalan 1 TB veri, o saat için geçerli olan kullandıkça öde fiyatları üzerinden ücretlendirilir. Bir sonraki saatte yapılan kullanım 100 TB olursa tamamı rezervasyon tarafından karşılanır.

## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bize ulaşın

Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

- [Ayrılmış kapasite ile Blob depolama maliyetlerini iyileştirme](../../storage/blobs/storage-blob-reserved-capacity.md)
- [Azure Rezervasyonlar nedir?](save-compute-costs-reservations.md)
