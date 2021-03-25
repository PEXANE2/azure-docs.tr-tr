---
title: Rezervasyon iskontolarının Azure depolama hizmetlerine nasıl uygulandığını anlayın | Microsoft Docs
description: Ayrılmış kapasite iskontolarının Azure Blob depolama, Azure dosyaları ve Azure Data Lake Storage 2. kaynaklarına nasıl uygulandığı hakkında bilgi edinin.
author: tamram
ms.service: cost-management-billing
ms.subservice: reservations
ms.topic: conceptual
ms.date: 03/08/2021
ms.author: tamram
ms.openlocfilehash: f18097f0da658b3a485b79f5279e9ecc54a41ea0
ms.sourcegitcommit: a8ff4f9f69332eef9c75093fd56a9aae2fe65122
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2021
ms.locfileid: "105024039"
---
# <a name="understand-how-reservation-discounts-are-applied-to-azure-storage-services"></a>Azure depolama hizmetlerine rezervasyon iskontolarının nasıl uygulandığını anlayın 
Azure depolama hizmetleri kapasiteyi ayırarak depolama maliyetlerinden tasarruf etmenizi sağlar. Azure Blob depolama, Azure dosyaları ve Azure Data Lake depolama Gen 2, ayrılan örnekleri destekler. Ayrılmış kapasiteyi satın aldıktan sonra, rezervasyon indirimiyle eşleşen depolama kaynaklarına otomatik olarak rezervasyon iskontosu uygulanır. Rezervasyon indirimi yalnızca depolama kapasitesine uygulanır. Bant genişliği ve istek oranı, kullandıkça öde fiyatları üzerinden ücretlendirilir.

Azure Blob depolama ve Azure Data Lake depolama Gen 2 ayrılmış kapasitesi hakkında daha fazla bilgi için bkz. [ayrılmış kapasiteye sahip BLOB depolama için maliyetleri iyileştirme](../../storage/blobs/storage-blob-reserved-capacity.md). Azure dosyaları ayrılmış kapasitesi hakkında daha fazla bilgi için bkz. [ayrılmış kapasiteye sahip Azure dosyaları için maliyetleri iyileştirme](../../storage/files/files-reserve-capacity.md).

Azure Blob depolama rezervasyonu fiyatlandırması hakkında daha fazla bilgi için bkz. [Blok Blobu fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/blobs/) ve [Azure Data Lake Storage Gen 2 fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/data-lake/). Azure dosyaları depolama ayırma fiyatlandırması hakkında bilgi için bkz. [Azure dosyaları fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/files).

## <a name="how-the-reservation-discount-is-applied"></a>Rezervasyon indiriminin uygulanması
Ayrılmış kapasite indirimi, saat başına desteklenen depolama kaynaklarına uygulanır.

Ayrılmış kapasite indirimi bir "kullanım-veya-kaybedilme" indirimi olur. Belirli bir saat için rezervasyon şartlarını karşılayan herhangi bir blok blobunuz, Azure dosya paylaşımı veya Azure Data Lake Storage 2. kaynağınız yoksa, bu saat için bir rezervasyon miktarı kaybedersiniz. Kullanılmayan ayrılmış saatleri devredemezsiniz.

Bir kaynağı sildiğinizde rezervasyon indirimi, belirtilen kapsamdaki başka bir eşleşen kaynağa otomatik olarak uygulanır. Belirtilen kapsamda eşleşen kaynak bulunamazsa, ayrılan saatler kaybedilir.

## <a name="discount-examples"></a>İndirim örnekleri
Aşağıdaki örneklerde, dağıtıma bağlı olarak, ayrılmış kapasite iskontosunun nasıl uygulandığı gösterilmektedir.

1 yıllık bir dönem için ABD Batı 2 bölgesinde 100 TiB ayrılmış kapasitesi satın aldığınızı varsayalım. Ayırma işlemi, sık erişimli erişim katmanında yerel olarak yedekli depolama (LRS) blob depolaması içindir.

Bu örnek rezervasyonun maliyetinin 18.540 ABD doları olduğunu düşünelim. Tutarın tamamını peşin ödeyebilir veya sonraki 12 ay boyunca aylık 1.545 ABD doları taksitler halinde ödeme yapabilirsiniz.

Bu örneklerde aylık rezervasyon planına kaydolduğunuzu kabul edelim. Aşağıdaki senaryolarda, ayrılmış kapasitenizi az veya fazla kullandığınızda karşılaşacağınız durumlar anlatılmıştır.

### <a name="underusing-your-capacity"></a>Kapasitenizi az kullanma
Ayırma döneminde belirli bir saat içinde yalnızca 100 TiB ayrılmış kapasiteniz için yalnızca 80 TiB kullandığınızı varsayalım. Kalan 20 TiB bu saat için uygulanmaz ve üzerinde taşınmaz.

### <a name="overusing-your-capacity"></a>Kapasitenizi fazla kullanma
Ayırma döneminde belirli bir saat içinde 101 TiB depolama kapasitesi kullandığınızı varsayalım. Rezervasyon indirimi, verilerinizin 100 TiB ' si için geçerlidir ve kalan 1 TiB, bu saat için Kullandıkça Öde tarifesine göre ücretlendirilir. Kullanımınızın bir sonraki saati 100 TiB olarak değişirse, tüm kullanımlar rezervasyon tarafından ele alınmıştır.

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bizimle iletişim kurun
Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar
- [Ayrılmış kapasite ile Blob depolama maliyetlerini iyileştirme](../../storage/blobs/storage-blob-reserved-capacity.md)
- [Ayrılmış kapasiteye sahip Azure dosyaları için maliyetleri iyileştirin](../../storage/files/files-reserve-capacity.md)
- [Azure Ayrılmış Sanal Makine Örnekleri nedir?](save-compute-costs-reservations.md)
