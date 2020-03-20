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
ms.sourcegitcommit: 20429bc76342f9d365b1ad9fb8acc390a671d61e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79086180"
---
Çok kiracılı bir hizmet olan Azure Data Factory, müşteri aboneliklerinin diğer aboneliklerin iş yüklerinden korunduğundan emin olmak için aşağıdaki varsayılan sınırları uygular. Aboneliğinize yönelik sınırları en üst düzeye çıkarmak için desteğe ulaşın.

### <a name="version-2"></a>Sürüm 2

| Kaynak | Varsayılan limit | Üst sınır |
| -------- | ------------- | ------------- |
| Bir Azure aboneliğindeki veri fabrikaları | 800 | [Desteğe ulaşın](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Bir veri fabrikasında bulunan işlem hatları, veri kümeleri, tetikleyiciler, bağlı hizmetler ve tümleştirme çalışma zamanları gibi varlıkların toplam sayısı | 5\.000 | [Desteğe ulaşın](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Bir abonelikte Azure-SSIS Integration Runtime’lar için ayrılan toplam CPU çekirdeği | 256 | [Desteğe ulaşın](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Fabrikadaki tüm işlem hatlarında paylaşılan veri fabrikası başına eşzamanlı işlem hattı çalıştırması | 10,000  | [Desteğe ulaşın](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Her [Azure Integration Runtime bölgesindeki](../articles/data-factory/concepts-integration-runtime.md#integration-runtime-location) abonelik başına eşzamanlı dış etkinlik çalıştırması<br><small>Tümleştirme çalışma zamanında yönetilen dış etkinlikler Databricks, saklı yordam, HDInsight, Web ve diğerleri de dahil olmak üzere bağlı hizmetlerde çalıştırılır.</small> | 3000 | [Desteğe ulaşın](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Her [Azure Integration Runtime bölgesindeki](../articles/data-factory/concepts-integration-runtime.md#integration-runtime-location) abonelik başına eşzamanlı işlem hattı etkinliği çalıştırması <br><small>Lookup, GetMetadata ve Delete de dahil olmak üzere tüm işlem hattı etkinlikleri tümleştirme çalışma zamanında çalıştırılır.</small>| 1000 | [Desteğe ulaşın](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| [Azure Integration Runtime bölgesi](../articles/data-factory/concepts-integration-runtime.md#integration-runtime-location) ve abonelik başına eşzamanlı yazma işlemi<br><small>Buna test bağlantısı, göz atma klasörü listesi, tablo listesi ve önizleme verileri de dahildir. | 200 | [Desteğe ulaşın](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| [Azure Integration Runtime bölgesi](../articles/data-factory/concepts-integration-runtime.md#integration-runtime-location) ve abonelik başına Eşzamanlı Veri Tümleştirme Birimi<sup>1</sup>| Bölge grubu 1<sup>2</sup>: 6000<br>Bölge grubu 2<sup>2</sup>: 3000<br>Bölge grubu 3<sup>2</sup>: 1500 | [Desteğe ulaşın](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Kapsayıcılara yönelik iç etkinlikleri de içeren işlem hattı başına etkinlik sayısı üst sınırı | 40 | 40 |
| Tek bir şirket içinde barındırılan tümleştirme çalışma zamanında oluşturulabilen bağlı tümleştirme çalışma zamanı sayısı üst sınırı | 100 | [Desteğe ulaşın](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| İşlem hattı başına en fazla parametre | 50 | 50 |
| ForEach öğeleri | 100.000 | 100.000 |
| ForEach paralelliği | 20 | 50 |
| İşlem hattı başına en fazla sıraya alınan çalıştırma sayısı | 100 | 100 |
| İfade başına karakter sayısı | 8,192 | 8,192 |
| Atlayan pencere tetikleyicisi aralığı süresi alt sınırı | 15 dakika | 15 dakika |
| İşlem hattı etkinliği çalıştırmaları için zaman aşımı süresi üst sınırı | 7 gün | 7 gün |
| İşlem hattı nesneleri için nesne başına bayt<sup>3</sup> | 200 KB | 200 KB |
| Veri kümesi ve bağlı hizmet nesnesi başına bayt<sup>3</sup> | 100 KB | 2\.000 KB |
| Kopyalama etkinliği başına<sup>1</sup>Veri Tümleştirme Birimi | 256 | [Desteğe ulaşın](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Yazma API’si çağrıları | 1\.200/sa<br/><br/> Bu sınırlama Azure Data Factory tarafından değil Azure Resource Manager tarafından uygulanır. | [Desteğe ulaşın](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Okuma API’si çağrıları | 12.500/sa<br/><br/> Bu sınırlama Azure Data Factory tarafından değil Azure Resource Manager tarafından uygulanır. | [Desteğe ulaşın](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Dakika başına izleme sorgusu sayısı | 1000 | [Desteğe ulaşın](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Dakika başına giriş CRUD işlemi sayısı | 50 | [Desteğe ulaşın](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Veri akışı hata ayıklama oturumu için süre üst sınırı | 8 saat | 8 saat |
| Fabrika başına eşzamanlı veri akışı sayısı | 50 | [Desteğe ulaşın](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Fabrika ve kullanıcı başına eşzamanlı veri akışı hata ayıklama oturumu sayısı | 3 | 3 |
| Veri Akışı Azure IR TTL sınırı | 4 saat | [Desteğe ulaşın](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |

<sup>1</sup> Buluttan buluta kopyalama işlemlerinde veri tümleştirme birimi (DIU) kullanılır. Daha fazla bilgi edinmek için bkz. [Veri tümleştirme üniteleri (sürüm 2)](../articles/data-factory/copy-activity-performance.md#data-integration-units). Faturalandırma hakkında bilgi edinmek için bkz. [Azure Data Factory fiyatlandırması](https://azure.microsoft.com/pricing/details/data-factory/).

<sup>2</sup> Veri uyumluluğu ve verimliliği sunan ve ağ çıkış maliyetlerini azaltan [Azure Integration Runtime](../articles/data-factory/concepts-integration-runtime.md#azure-integration-runtime) [genel kullanıma sunuldu](https://azure.microsoft.com/global-infrastructure/services/). 

| Bölge grubu | Bölgeler | 
| -------- | ------ |
| Bölge grubu 1 | Orta ABD, Doğu ABD, Doğu ABD2, Kuzey Avrupa, Batı Avrupa, Batı ABD, Batı ABD 2 |
| Bölge grubu 2 | Doğu Avustralya, Güneydoğu Avustralya, Güney Brezilya, Orta Hindistan, Japonya Doğu, Orta Kuzey ABD, Orta Güney ABD, Güneydoğu Asya, Orta Batı ABD |
| Bölge grubu 3 | Kanada Orta, Doğu Asya, Fransa Orta, Kore Orta, UK Güney |

<sup>3</sup> İşlem hattı, veri kümesi ve bağlı hizmet nesnesi iş yükünüzün mantıksal gruplandırmasını temsil eder. Bu nesnelere ilişkin sınırlar, Azure Data Factory ile taşıyabileceğiniz ve işleyebileceğiniz veri miktarıyla ilişkili değildir. Data Factory petabaytlarca veriyi işleyebilen ölçek ile tasarlanmıştır.

### <a name="version-1"></a>Sürüm 1

| **Kaynak** | **Varsayılan sınır** | **Üst sınır** |
| --- | --- | --- |
| Bir veri fabrikasında bulunan işlem hattı sayısı |2\.500 |[Desteğe ulaşın](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Bir veri fabrikasında bulunan veri kümesi sayısı |5\.000 |[Desteğe ulaşın](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Veri kümesi başına eşzamanlı dilim sayısı |10 |10 |
| İşlem hattı nesneleri için nesne başına bayt<sup>1</sup> |200 KB |200 KB |
| Veri kümesi ve bağlı hizmet nesnesi başına bayt<sup>1</sup> |100 KB |2\.000 KB |
| Bir abonelikte bulunan Azure HDInsight isteğe bağlı küme çekirdeği sayısı<sup>2</sup> |60 |[Desteğe ulaşın](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Kopyalama etkinliği çalıştırması başına bulut verisi hareketi birimi<sup>3</sup> |32 |[Desteğe ulaşın](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| İşlem hattı etkinliği çalıştırmaları için zaman aşımı sayısı |1000 |MaxInt (32 bit) |

<sup>1</sup> İşlem hattı, veri kümesi ve bağlı hizmet nesnesi iş yükünüzün mantıksal gruplandırmasını temsil eder. Bu nesnelere ilişkin sınırlar, Azure Data Factory ile taşıyabileceğiniz ve işleyebileceğiniz veri miktarıyla ilişkili değildir. Data Factory petabaytlarca veriyi işleyebilen ölçek ile tasarlanmıştır.

<sup>2</sup> İsteğe bağlı HDInsight çekirdekleri, veri fabrikasını içeren abonelikten tahsis edilir. Bunun sonucu olarak, önceki sınır Data Factory tarafından uygulanan isteğe bağlı HDInsight çekirdeklerine yönelik çekirdek sınırıdır. Azure aboneliğinizle ilişkili çekirdek sınırlandırmasından farklıdır.

<sup>3</sup> Sürüm 1’e yönelik bulut verisi taşıma birimi (DMU) bir buluttan buluta kopyalama işleminde kullanılır. Daha fazla bilgi edinmek için bkz. [Bulut verisi taşıma birimleri (sürüm 1)](../articles/data-factory/v1/data-factory-copy-activity-performance.md#cloud-data-movement-units). Faturalandırma hakkında bilgi edinmek için bkz. [Azure Data Factory fiyatlandırması](https://azure.microsoft.com/pricing/details/data-factory/).

| **Kaynak** | **Varsayılan alt sınır** | **Alt sınır** |
| --- | --- | --- |
| Zamanlama aralığı |15 dakika |15 dakika |
| Deneme girişimleri arasındaki aralık |1 saniye |1 saniye |
| Yeniden deneme zaman aşımı değeri |1 saniye |1 saniye |

#### <a name="web-service-call-limits"></a>Web hizmeti çağrı sınırları
Azure Resource Manager’ın API çağrılarına yönelik sınırları bulunur. API çağrılarını [Azure Resource Manager API sınırlarını](../articles/azure-resource-manager/management/azure-subscription-service-limits.md#resource-group-limits) aşmayan bir hızda yapabilirsiniz.
