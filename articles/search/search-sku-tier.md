---
title: Fiyatlandırma katmanı seçin
titleSuffix: Azure Cognitive Search
description: "Azure Bilişsel Arama için fiyatlandırma katmanları (veya SKU 'Lar) hakkında bilgi edinin. Şu katmanlarda bir arama hizmeti sağlanabilir: ücretsiz, temel ve standart. Standart, çeşitli kaynak yapılandırmalarında ve kapasite düzeylerinde kullanılabilir."
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 01/15/2021
ms.custom: contperf-fy21q2
ms.openlocfilehash: 38ddfc2d3940bb9267edd6c5c683918c1fb5dc58
ms.sourcegitcommit: 25d1d5eb0329c14367621924e1da19af0a99acf1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2021
ms.locfileid: "98251714"
---
# <a name="choose-a-pricing-tier-for-azure-cognitive-search"></a>Azure Bilişsel Arama için bir fiyatlandırma katmanı seçin

[Bir arama hizmeti oluşturduğunuzda](search-create-service-portal.md), hizmetin kullanım ömrü boyunca düzeltilen bir Fiyatlandırma Katmanı (veya SKU) seçersiniz. Tahmini aylık maliyetler portalda **fiyatlandırma katmanı Seç** sayfasında gösterilir. Bunun yerine PowerShell veya Azure CLı aracılığıyla bir hizmet oluşturuyorsanız, katman parametresi aracılığıyla belirtilir **`-Sku`** .

Seçtiğiniz katman şunları belirler:

+ Hizmette oluşturabileceğiniz en fazla dizin ve diğer nesne sayısı
+ Bölümlerin boyutu ve hızı (fiziksel depolama)
+ Faturalanabilir ücret sabit bir aylık maliyet, ancak kapasite eklerseniz de artımlı maliyet

Birkaç örnekte, seçtiğiniz katman [Premium özelliklerinin](#premium-features)kullanılabilirliğini belirler.

## <a name="tier-descriptions"></a>Katman açıklamaları

Katmanlar **ücretsiz**, **temel**, **Standart** ve depolama için **iyileştirilmiş**. Standart ve depolama için Iyileştirilmiş çeşitli yapılandırmalarda ve kapasitelerde kullanılabilir. Azure portal aşağıdaki ekran görüntüsünde, kullanılabilir katmanlar, eksi fiyatlandırma (portalda ve [fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/search/)bulabilirsiniz) gösterilmektedir. 

:::image type="content" source="media/search-sku-tier/tiers.png" alt-text="Fiyatlandırma Katmanı grafiği" border="true":::

**Ücretsiz** , öğreticileri ve kod örneklerini çalıştırma gibi daha küçük projeler için sınırlı bir arama hizmeti oluşturur. Dahili olarak, sistem kaynakları birden çok abone arasında paylaşılır. Ücretsiz bir hizmeti ölçeklendiremez veya önemli iş yüklerini çalıştıramazsınız.

**Temel** ve **Standart** , en yaygın olarak kullanılan faturalanabilir katmanlardır, ancak iş yüklerinin ölçeklendirilmesine daha fazla esneklik sağladığından **Standart** olarak varsayılan olarak kullanılır. Denetiminizin altında adanmış kaynaklar sayesinde, daha büyük projeler dağıtabilir, performansı iyileştirir ve kapasiteyi artırabilirsiniz.

Bazı katmanlar belirli iş türleri için tasarlanmıştır. Örneğin, **Standart 3 yüksek yoğunluklu (S3 HD)** , S3 için bir *barındırma modudur* ; burada temeldeki donanım çok sayıda daha küçük dizinler için optimize edilmiştir ve çok kiracılı senaryolar için tasarlanmıştır. S3 HD, S3 ile aynı birim başına ücretine sahiptir, ancak donanım çok sayıda daha küçük dizin üzerinde hızlı dosya okuma için en iyi duruma getirilmiştir.

**Depolama Için iyileştirilmiş** katmanlar, standart katmanlardan TB başına daha büyük depolama kapasitesi sunar. Birincil zorunluluğunu getirir, belirli uygulama gereksinimleriniz için doğrulamanız gereken daha yüksek sorgu gecikmedir. Bu katmanın performans konuları hakkında daha fazla bilgi için bkz. [performans ve iyileştirme konuları](search-performance-optimization.md).

[Fiyatlandırma sayfasındaki](https://azure.microsoft.com/pricing/details/search/)çeşitli katmanlar hakkında daha fazla bilgi edinmek için bkz. [Azure bilişsel arama 'deki hizmet sınırları](search-limits-quotas-capacity.md) ve hizmet sağlanırken Portal sayfası.

<a name="premium-features"></a>

## <a name="feature-availability-by-tier"></a>Katmana göre özellik kullanılabilirliği

Birçok özellik, ücretsiz katman dahil olmak üzere tüm katmanlarda kullanılabilir. Birkaç durumda, seçtiğiniz katman bir özelliği uygulama yeteneğinizi etkiler. Aşağıdaki tabloda hizmet katmanıyla ilgili özellik kısıtlamaları açıklanmaktadır.

| Öne çıkan özelliği | Sınırlamalar |
|---------|-------------|
| [Dizinleyiciler](search-indexer-overview.md) | Dizin oluşturucular S3 HD üzerinde kullanılamaz.  |
| [Yapay zeka zenginleştirme](search-security-manage-encryption-keys.md) | Ücretsiz katmanda çalışır, ancak önerilmez. |
| [Giden (Dizin Oluşturucu) erişimi için yönetilen veya güvenilen kimlikler](search-howto-managed-identities-data-sources.md) | Ücretsiz katmanda kullanılamaz.|
| [Müşteri tarafından yönetilen şifreleme anahtarları](search-security-manage-encryption-keys.md) | Ücretsiz katmanda kullanılamaz. |
| [IP güvenlik duvarı erişimi](service-configure-firewall.md) | Ücretsiz katmanda kullanılamaz. |
| [Özel uç nokta (Azure özel bağlantısı ile tümleştirme)](service-create-private-endpoint.md) | Bir arama hizmetine gelen bağlantılar için ücretsiz katmanda kullanılamaz. Diğer Azure kaynaklarına yönelik Dizin oluşturucular tarafından giden bağlantılar için ücretsiz veya S3 HD 'de kullanılamaz. Becerileri kullanan Dizin oluşturucular için, ücretsiz, temel, S1 veya S3 HD ' de kullanılamaz.|

Yeterli kapasiteye sahip değilseniz, Kaynak yoğunluklu Özellikler iyi çalışmayabilir. Örneğin, [AI zenginleştirme](cognitive-search-concept-intro.md) , veri kümesi küçük olmadığı sürece ücretsiz bir hizmette zaman aşımına uğrar uzun süreli yetenekler içerir.

## <a name="upper-limits"></a>Üst sınırlar

Katmanlar hizmetin kendisinin en fazla depolama alanını, oluşturabileceğiniz en fazla dizin, Dizin Oluşturucu, veri kaynağı, becerileri ve eş anlamlı haritalar sayısını da tespit edebilir. Tüm limitlerin tamamını tümüyle kesmek için bkz. [Azure bilişsel arama hizmet limitleri](search-limits-quotas-capacity.md). 

## <a name="partition-size-and-speed"></a>Bölüm boyutu ve hızı

Katman fiyatlandırması, temel için 2 GB ve depolama için Iyileştirilmiş (L2) katmanları için 2 TB 'a kadar değişen bölüm başına depolama hakkındaki ayrıntıları içerir. İşlemler, gecikme süresi ve aktarım hızı gibi diğer donanım özellikleri yayımlanmamıştır, ancak belirli çözüm mimarileri için tasarlanan katmanlar, bu senaryoları destekleme özelliklerine sahip donanımlar üzerine kurulmuştur.

## <a name="billing-rates"></a>Fatura ücretleri

Katmanlar, daha pahalı donanımlar üzerinde çalışan katmanlara yönelik daha yüksek ücretler ve daha pahalı özellikler sağlayan farklı fatura fiyatları vardır. Fatura ücreti, Azure Bilişsel Arama her bir hizmet katmanı için [Azure fiyatlandırma sayfalarında](https://azure.microsoft.com/pricing/details/search/) gördüğünüz şeydir.

Bir hizmet oluşturduktan sonra faturalandırma ücreti, hizmetin saat içinde çalıştırılması için *sabit bir maliyet* ve daha fazla kapasite eklemeyi seçerseniz *artımlı bir maliyet* haline gelir.

Arama Hizmetleri, *bölüm* kaynakları (depolama için) ve *çoğaltmalar* (sorgu altyapısının örnekleri) biçiminde bilgi işlem kaynaklarına ayrılır. Başlangıçta, her biri ile bir hizmet oluşturulur ve faturalandırma ücreti her iki kaynağın de dahil değildir. Ancak kapasiteyi ölçeklendirirseniz, maliyetler faturalandırılabilir fiyat artışlarıyla artar veya azaltılabilir.

Aşağıdaki örnek, bir gösterim sağlar. Ayda $100 olan kuramsal faturalandırma oranını varsayın. Arama hizmetini bir bölümün ve bir çoğaltmanın ilk kapasitesine tutarsanız $100, ayın sonunda ödemeyi beklediğiniz şeydir. Ancak, yüksek kullanılabilirlik elde etmek için iki çoğaltma eklerseniz aylık fatura, ilk çoğaltma bölümü çifti için $300 ($100) ve ardından iki çoğaltma için $200 olarak artar.

Bu fiyatlandırma modeli, bir arama hizmeti tarafından kullanılan numara *arama birimlerine* (su) faturalama ücreti uygulama kavramını temel alır. Tüm hizmetler başlangıçta bir SU üzerinden sağlanır, ancak daha büyük iş yüklerini işlemek için bölümler veya çoğaltmalar ekleyerek SUs 'yi artırabilirsiniz. Daha fazla bilgi için bkz. [Arama hizmeti maliyetlerini tahmin etme](search-sku-manage-costs.md).

## <a name="next-steps"></a>Sonraki adımlar

Fiyatlandırma katmanını seçmek için en iyi yol, en az maliyetli bir katmanla başlamadır ve daha sonra deneyimle ve test, hizmeti tutma veya daha yüksek bir katmanda yeni bir tane oluşturma kararınızı bilgilendirir. Sonraki adımlar için, bir katmanda, yapmak istediğiniz test düzeyini barındırabilecek bir arama hizmeti oluşturmanızı ve ardından maliyet ve kapasite tahmini hakkında öneriler için aşağıdaki kılavuzu incelemenizi öneririz.

+ [Arama hizmeti oluşturma](search-create-service-portal.md)
+ [Maliyetleri tahmin etme](search-sku-manage-costs.md)
+ [Tahmin kapasitesi](search-sku-manage-costs.md)