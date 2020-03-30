---
title: include dosyası
description: include dosyası
services: data-factory
author: linda33wj
ms.service: data-factory
ms.topic: include
ms.date: 01/08/2020
ms.author: jingwang
ms.custom: include file
ms.openlocfilehash: 217e48ab21439f03e0b52c894c9aace5b51b1502
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79086180"
---
Azure Veri Fabrikası, müşteri aboneliklerinin birbirlerinin iş yüklerinden korunduğundan emin olmak için aşağıdaki varsayılan sınırlara sahip çok kiracılı bir hizmettir. Aboneliğiniz için sınırları maksimuma çıkarmak için desteğe başvurun.

### <a name="version-2"></a>Sürüm 2

| Kaynak | Varsayılan limit | Üst sınır |
| -------- | ------------- | ------------- |
| Azure aboneliğindeki veri fabrikaları | 800 | [Desteğe başvurun.](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Veri fabrikası içindeki ardışık hatlar, veri kümeleri, tetikleyiciler, bağlantılı hizmetler ve tümleştirme çalışma saatleri gibi toplam varlık sayısı | 5.000 | [Desteğe başvurun.](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Azure-SSIS Tümleştirme Çalışma Saatleri için tek bir abonelik altında toplam CPU çekirdekleri | 256 | [Desteğe başvurun.](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Fabrikadaki tüm boru hatları arasında paylaşılan veri fabrikası başına eşzamanlı boru hattı çalışır | 10,000  | [Desteğe başvurun.](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Eşzamanlı Dış [etkinlik, Azure Tümleştirme Çalışma Süresi bölgesi](../articles/data-factory/concepts-integration-runtime.md#integration-runtime-location) başına abonelik başına çalışır<br><small>Dış etkinlikler tümleştirme çalışma zamanında yönetilir, ancak Databricks, depolanan yordam, HDInsights, Web ve diğerleri dahil olmak üzere bağlantılı hizmetlerde yürütülür.</small> | 3000 | [Desteğe başvurun.](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Eşzamanlı Pipeline [etkinliği, Azure Tümleştirme Çalışma Süresi bölgesi](../articles/data-factory/concepts-integration-runtime.md#integration-runtime-location) başına abonelik başına çalışır <br><small>Ardışık düzen etkinlikleri, Arama, GetMetadata ve Delete dahil olmak üzere tümleştirme çalışma zamanında yürütülür.</small>| 1000 | [Desteğe başvurun.](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| [Azure Tümleştirme Çalışma Süresi bölgesi](../articles/data-factory/concepts-integration-runtime.md#integration-runtime-location) başına abonelik başına eşzamanlı yazma işlemleri<br><small>Test bağlantısı, göz atma klasör listesi ve tablo listesi, önizleme verileri de dahil olmak üzere. | 200 | [Desteğe başvurun.](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Eşzamanlı Veri Tümleştirme Birimleri<sup>1</sup> Abonelik başına azure [tümleştirme çalışma süresi bölgesi](../articles/data-factory/concepts-integration-runtime.md#integration-runtime-location) başına tüketim| Bölge grup 1<sup>2</sup>: 6000<br>Bölge grup 2<sup>2</sup>: 3000<br>Bölge grup 3<sup>2</sup>: 1500 | [Desteğe başvurun.](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Konteynerler için iç faaliyetleri içeren boru hattı başına maksimum faaliyetleri | 40 | 40 |
| Tek bir kendi kendine barındırılan tümleştirme çalışma süresine karşı oluşturulabilecek maksimum bağlantılı tümleştirme çalışma süreleri sayısı | 100 | [Desteğe başvurun.](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Boru hattı başına maksimum parametreler | 50 | 50 |
| ForEach öğeleri | 100.000 | 100.000 |
| ForEach paralellik | 20 | 50 |
| Ardışık hat başına en fazla sıralı çalışır | 100 | 100 |
| İfade başına karakterler | 8,192 | 8,192 |
| Minimum yuvarlanan pencere tetikleme aralığı | 15 dk | 15 dk |
| Boru hattı etkinliği için maksimum zaman aşımı | 7 gün | 7 gün |
| Boru hattı nesneleri için nesne başına bayt<sup>3</sup> | 200 KB | 200 KB |
| Veri kümesi ve bağlantılı hizmet nesneleri için nesne başına bayt<sup>3</sup> | 100 KB | 2.000 KB |
| Veri Tümleştirme Birimleri<sup>1</sup> kopya etkinliği çalıştır | 256 | [Desteğe başvurun.](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| API aramaları yazma | 1.200/saat<br/><br/> Bu sınır, Azure Veri Fabrikası tarafından değil, Azure Kaynak Yöneticisi tarafından uygulanır. | [Desteğe başvurun.](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| API çağrılarını okuma | 12.500/saat<br/><br/> Bu sınır, Azure Veri Fabrikası tarafından değil, Azure Kaynak Yöneticisi tarafından uygulanır. | [Desteğe başvurun.](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Sorguları dakika başına izleme | 1000 | [Desteğe başvurun.](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Varlık CRUD işlemleri dakikada | 50 | [Desteğe başvurun.](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Veri akışı hata ayıklama oturumunun maksimum zamanı | 8 saat | 8 saat |
| Fabrika başına eşzamanlı veri akışı sayısı | 50 | [Desteğe başvurun.](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Fabrika başına kullanıcı başına eşzamanlı veri akışı hata ayıklama oturumları sayısı | 3 | 3 |
| Veri Akışı Azure IR TTL sınırı | 4 saat | [Desteğe başvurun.](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |

<sup>1</sup> Veri tümleştirme birimi (DIU), Buluttan Buluta Kopyalama işleminde kullanılır ve [Veri tümleştirme birimlerinden (sürüm 2)](../articles/data-factory/copy-activity-performance.md#data-integration-units)daha fazla bilgi edinir. Faturalandırma hakkında daha fazla bilgi için [Azure Veri Fabrikası fiyatlandırması'na](https://azure.microsoft.com/pricing/details/data-factory/)bakın.

<sup>2</sup> [Azure Tümleştirme Çalışma Süresi,](../articles/data-factory/concepts-integration-runtime.md#azure-integration-runtime) veri uyumluluğu, verimliliği ve azaltılmış ağ çıkış maliyetlerini sağlamak için [genel olarak kullanılabilir.](https://azure.microsoft.com/global-infrastructure/services/) 

| Bölge grubu | Bölgeler | 
| -------- | ------ |
| Bölge grubu 1 | Orta ABD, Doğu ABD, Doğu US2, Kuzey Avrupa, Batı Avrupa, Batı ABD, Batı ABD 2 |
| Bölge grubu 2 | Avustralya Doğu, Avustralya Güneydoğu, Brezilya Güney, Orta Hindistan, Japonya Doğu, Kuzeyorta ABD, Güney orta ABD, Güneydoğu Asya, Batı Orta ABD |
| Bölge grubu 3 | Kanada Orta, Doğu Asya, Fransa Merkez, Kore Merkez, İngiltere Güney |

<sup>3</sup> Ardışık hatlar, veri kümesi ve bağlantılı hizmet nesneleri iş yükünüzün mantıksal bir gruplandırmayı temsil eder. Bu nesneler için sınırlar, Azure Veri Fabrikası ile taşıyabileceğiniz ve işleebileceğiniz veri miktarıyla ilgili değildir. Veri Fabrikası, petabaytlara kadar veriyi işlemek için ölçeklendirecek şekilde tasarlanmıştır.

### <a name="version-1"></a>Sürüm 1

| **Kaynak** | **Varsayılan limit** | **Maksimum limit** |
| --- | --- | --- |
| Veri fabrikası içindeki boru hatları |2,500 |[Desteğe başvurun.](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Veri fabrikası içindeki veri kümeleri |5.000 |[Desteğe başvurun.](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Veri kümesi başına eşzamanlı dilimler |10 |10 |
| Boru hattı nesneleri için nesne başına bayt<sup>1</sup> |200 KB |200 KB |
| Veri kümesi ve bağlantılı hizmet nesneleri için nesne başına bayt<sup>1</sup> |100 KB |2.000 KB |
| Abonelik<sup>2</sup> içindeki Azure HDInsight isteğe bağlı küme çekirdekleri |60 |[Desteğe başvurun.](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Kopyalama etkinliği başına bulut veri hareketi birimleri çalıştırın<sup>3</sup> |32 |[Desteğe başvurun.](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Boru hattı etkinlik çalışır için yeniden sayım |1000 |MaxInt (32 bit) |

<sup>1</sup> Ardışık hatlar, veri kümesi ve bağlantılı hizmet nesneleri iş yükünüzün mantıksal bir gruplandırmayı temsil eder. Bu nesneler için sınırlar, Azure Veri Fabrikası ile taşıyabileceğiniz ve işleebileceğiniz veri miktarıyla ilgili değildir. Veri Fabrikası, petabaytlara kadar veriyi işlemek için ölçeklendirecek şekilde tasarlanmıştır.

<sup>2</sup> İsteğe bağlı HDInsight çekirdekleri, veri fabrikasını içeren abonelikten ayrılır. Sonuç olarak, önceki sınır, isteğe bağlı HDInsight çekirdekleri için Veri Fabrikası tarafından uygulanan çekirdek sınırıdır. Azure aboneliğinizle ilişkili temel sınırdan farklıdır.

<sup>3</sup> Sürüm 1 için bulut veri hareketi birimi (DMU), buluttan buluta kopyalama işleminde kullanılır ve [Bulut veri hareketi birimlerinden (sürüm 1)](../articles/data-factory/v1/data-factory-copy-activity-performance.md#cloud-data-movement-units)daha fazla bilgi edinir. Faturalandırma hakkında daha fazla bilgi için [Azure Veri Fabrikası fiyatlandırması'na](https://azure.microsoft.com/pricing/details/data-factory/)bakın.

| **Kaynak** | **Varsayılan alt sınır** | **Minimum limit** |
| --- | --- | --- |
| Zamanlama aralığı |15 dakika |15 dakika |
| Yeniden deneme denemeleri arasındaki aralık |1 saniye |1 saniye |
| Zaman dışarı değerini yeniden deneyin |1 saniye |1 saniye |

#### <a name="web-service-call-limits"></a>Web hizmeti arama limitleri
Azure Kaynak Yöneticisi'nin API çağrıları için sınırları vardır. ApI çağrılarını [Azure Kaynak Yöneticisi API sınırları](../articles/azure-resource-manager/management/azure-subscription-service-limits.md#resource-group-limits)içinde bir oranda yapabilirsiniz.
