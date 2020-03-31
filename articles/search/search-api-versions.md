---
title: .NET ve REST için API sürüm yönetimi
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama REST API'leri ve .NET SDK'daki istemci kitaplığı için sürüm ilkesi.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 178f56354120bf7a65c51f1c9cf54e34bd011d97
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79137297"
---
# <a name="api-versions-in-azure-cognitive-search"></a>Azure Bilişsel Arama'da API sürümleri

Azure Bilişsel Arama özellik güncelleştirmelerini düzenli olarak sunar. Bazen, ancak her zaman değil, bu güncelleştirmeler geriye dönük uyumluluğu korumak için API'nin yeni bir sürümünü gerektirir. Yeni bir sürüm yayımlamak, arama hizmeti güncelleştirmelerini kodunuza ne zaman ve nasıl entegre ettiğinizi denetlemenize olanak tanır.

Kural olarak, Azure Bilişsel Arama ekibi yeni sürümleri yalnızca gerektiğinde yayımlar, çünkü yeni bir API sürümünü kullanmak için kodunuzu yükseltmek için biraz çaba içerebilir. Yeni bir sürüm yalnızca API'nin bazı yönü geriye dönük uyumluluğu bozan bir şekilde değişmişse gereklidir. Bu tür değişiklikler, varolan özelliklerdeki düzeltmeler veya varolan API yüzey alanını değiştiren yeni özellikler nedeniyle gerçekleşebilir.

Aynı kural SDK güncelleştirmeleri için de geçerlidir. Azure Bilişsel Arama SDK' [sdk' si anlamsal sürüm](https://semver.org/) kurallarını izler, bu da sürümünün üç bölümden oluşur: büyük, minör ve yapı numarası (örneğin, 1.1.0). SDK'nın yeni bir ana sürümü yalnızca geriye dönük uyumluluğu bozan değişiklikler için yayımlanır. Kırılmayan özellik güncelleştirmeleri küçük sürümü artırır ve hata düzeltmeleri yalnızca yapı sürümünü artırır.

> [!NOTE]
> Azure Bilişsel Arama hizmeti örneğiniz, en sonuncusu da dahil olmak üzere birkaç REST API sürümlerini destekler. Bir sürümü artık en son sürüm olmadığında kullanmaya devam edebilirsiniz, ancak en yeni sürümü kullanmak için kodunuzu geçirmenizi öneririz. REST API'sini kullanırken, api sürüm parametresi aracılığıyla her istekteki API sürümünü belirtmeniz gerekir. .NET SDK'yı kullanırken, kullandığınız SDK sürümü REST API'nin ilgili sürümünü belirler. Eski bir SDK kullanıyorsanız, hizmet daha yeni bir API sürümünü desteklemek üzere yükseltilse bile bu kodu hiçbir değişiklik olmadan çalıştırmaya devam edebilirsiniz.

## <a name="snapshot-of-current-versions"></a>Geçerli sürümlerin anlık görüntüsü
Aşağıda, Azure Bilişsel Arama'daki tüm programlama arabirimlerinin geçerli sürümlerinin anlık görüntüsü verilmiştir.


| Arabirimler | En son ana sürüm | Durum |
| --- | --- | --- |
| [.NET SDK](https://aka.ms/search-sdk) |9.0 |Genel Kullanıma Açık, Mayıs 2019'da yayınlandı |
| [.NET SDK Önizleme](https://aka.ms/search-sdk-preview) |8.0-önizleme |Önizleme, Nisan 2019 yayınlandı |
| [Hizmet REST API'si](https://docs.microsoft.com/rest/api/searchservice/) |2019-05-06 |Genel Kullanıma Sunulmuştur |
| [Servis REST API 2019-05-06-Önizleme](search-api-preview.md) |2019-05-06-Önizleme |Önizleme |
| [.NET Yönetim SDK'sı](https://aka.ms/search-mgmt-sdk) |3,0 |Genel Kullanıma Sunulmuştur |
| [Yönetim REST API'si](https://docs.microsoft.com/rest/api/searchmanagement/) |2020-03-13|Genel Kullanıma Sunulmuştur |

REST API'leri için, `api-version` her çağrı da dahil olmak üzere gereklidir. Kullanmak, `api-version` önizleme API'si gibi belirli bir sürümü hedeflemeyi kolaylaştırır. Aşağıdaki örnek, parametrenin nasıl belirtildiğini `api-version` göstermektedir:

    GET https://my-demo-app.search.windows.net/indexes/hotels?api-version=2019-05-06

> [!NOTE]
> Her isteğin `api-version`bir , tüm API istekleri için aynı sürümü kullanmanızı öneririz. Bu, özellikle yeni API sürümleri önceki sürümler tarafından tanınmayan öznitelikleri veya işlemleri tanıttığında geçerlidir. API sürümlerinin karıştırılması istenmeyen sonuçlardoğurabilir ve kaçınılmalıdır.
>
> Service REST API ve Management REST API birbirinden bağımsız olarak sürülür. Sürüm sayılarında herhangi bir benzerlik rastlantısaldır.

Genellikle kullanılabilir (veya GA) API'ler üretimde kullanılabilir ve Azure hizmet düzeyi sözleşmelerine tabidir. Önizleme sürümleri, her zaman BIR GA sürümüne geçirilmeyen deneysel özelliklere sahiptir. **Üretim uygulamalarında önizleme API'lerini kullanmaktan kaçınmanız önemle tavsiye edilir.**

## <a name="update-to-the-latest-version-of-the-rest-api-by-october-15-2020"></a>15 Ekim 2020'ye kadar REST API'nin en son sürümüne güncelleştirin
Azure Bilişsel Arama REST API'sinin aşağıdaki sürümleri 15 Ekim 2020 itibariyle kullanımdan kaldırılacak ve artık desteklenmeyeceksiniz: **2014-07-31-Önizleme**, **2014-10-20-Önizleme**, **2015-02-28 Önizleme**, ve **2015-02-28**. Ayrıca, Azure Bilişsel Arama .NET **SDK'nın 3.0.0-rc'den** büyük sürümleri de bu REST API sürümlerinden birini hedefledikleri için kullanımdan kaldırılacaktır. Bu tarihten sonra, amortismana kalanmış REST API veya SDK sürümlerinden herhangi birini kullanan uygulamalar artık çalışmaz ve yükseltilmelidir. Bu tür herhangi bir değişiklik olduğu gibi, biz 12 ay haber veriyoruz, böylece ayarlamak için yeterli zaman var.  Azure Bilişsel Arama'yı kullanmaya devam etmek için lütfen [REST API'yi](search-api-migration.md) hedefleyen varolan kodu REST [API sürümü 2019-05-06](https://docs.microsoft.com/rest/api/searchservice/) veya daha yeni veya .NET SDK'yı 15 Ekim 2020'ye kadar [sürüm 3.0](search-dotnet-sdk-migration.md) veya daha yeni sürümüne geçirin.  En son sürüme güncelleme yle ilgili sorularınız varsa, kodunuzu güncellemek için yeterli zamanınız olduğundan emin olmak için lütfen 15 Mayıs 2020'ye azuresearch_contact@microsoft.com kadar posta gönderin.

## <a name="about-preview-and-generally-available-versions"></a>Önizleme ve Genel Kullanılabilir sürümler hakkında
Azure Bilişsel Arama her zaman deneysel özellikleri önce REST API'si, ardından .NET SDK'nın ön sürüm sürümleri aracılığıyla önceden yayımlar.

Önizleme özellikleri, özellik tasarımı ve uygulaması hakkında geri bildirim toplamak amacıyla test ve deneme için kullanılabilir. Bu nedenle, önizleme özellikleri zaman içinde, muhtemelen geriye doğru uyumluluğu bozan şekillerde değişebilir. Bu, kararlı ve küçük geriye dönük düzeltmeler ve geliştirmeler dışında değişme olasılığı düşük olan GA sürümündeki özelliklerin aksinedir. Ayrıca, önizleme özellikleri her zaman bir GA sürümü içine yapmazlar.

Bu nedenlerden dolayı, önizleme sürümlerine bağımlı olan üretim kodu yazmanızı öneririz. Eski bir önizleme sürümü kullanıyorsanız, genel olarak kullanılabilir (GA) sürümüne geçiş yapmanızı öneririz.

.NET SDK için: Kod geçişi için kılavuz [yükseltme .NET SDK](search-dotnet-sdk-migration-version-9.md)bulunabilir.

Genel kullanılabilirlik, Azure Bilişsel Arama'nın artık hizmet düzeyi sözleşmesi (SLA) altında olduğu anlamına gelir. SLA, [Azure Bilişsel Arama Hizmeti Düzeyi Sözleşmelerinde](https://azure.microsoft.com/support/legal/sla/search/v1_0/)bulunabilir.
