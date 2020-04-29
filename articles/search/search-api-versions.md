---
title: .NET ve REST için API sürüm yönetimi
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama REST API 'Leri ve .NET SDK içindeki istemci kitaplığı için sürüm ilkesi.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 178f56354120bf7a65c51f1c9cf54e34bd011d97
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79137297"
---
# <a name="api-versions-in-azure-cognitive-search"></a>Azure Bilişsel Arama API sürümleri

Azure Bilişsel Arama, özellik güncelleştirmelerini düzenli olarak kaydeder. Bazen, her zaman olmasa da, geriye dönük uyumluluğu korumak için bu güncelleştirmeler API 'nin yeni bir sürümünü gerektirir. Yeni bir sürümü yayımlamak, kodunuzda arama hizmeti güncelleştirmelerini ne zaman ve nasıl tümleştirmenizi denetlemenize olanak tanır.

Bir kural olarak, Azure Bilişsel Arama ekibi, kodunuzu yeni bir API sürümü kullanacak şekilde yükseltmek için biraz çaba içerebileceği için, yalnızca gerektiğinde yeni sürümler yayımlar. Yeni bir sürüm yalnızca API 'nin bazı yönlerinin geriye dönük uyumluluğu kesen bir şekilde değişmesi durumunda gereklidir. Bu tür değişiklikler, mevcut özelliklerle ilgili düzeltmeler veya var olan API yüzey alanını değiştiren yeni özelliklerden kaynaklanıyor olabilir.

Aynı kural SDK güncelleştirmeleri için de geçerlidir. Azure Bilişsel Arama SDK, anlam sürümü [oluşturma](https://semver.org/) kurallarını izler, yani sürümünün üç bölümü vardır: Ana, ikincil ve derleme numarası (örneğin, 1.1.0). SDK 'nın yeni bir ana sürümü yalnızca geriye dönük uyumluluğu kesen değişiklikler için serbest bırakılır. Önemli olmayan özellik güncelleştirmeleri ikincil sürümü artırır ve hata düzeltmeleri yalnızca derleme sürümünü artırır.

> [!NOTE]
> Azure Bilişsel Arama hizmet örneğiniz, en son sürüm dahil olmak üzere birkaç REST API sürümü destekler. Artık en son bir sürüm olmadığında bir sürümü kullanmaya devam edebilirsiniz, ancak kodunuzu en yeni sürümü kullanmak için geçirmeniz önerilir. REST API kullanırken, API sürümü parametresi aracılığıyla her istekte API sürümünü belirtmeniz gerekir. .NET SDK kullanıldığında, kullanmakta olduğunuz SDK sürümü REST API ilgili sürümünü belirler. Daha eski bir SDK kullanıyorsanız, hizmet daha yeni bir API sürümünü destekleyecek şekilde yükseltilse bile, bu kodu hiçbir değişiklik yapmadan çalıştırmaya devam edebilirsiniz.

## <a name="snapshot-of-current-versions"></a>Geçerli sürümlerin anlık görüntüsü
Azure Bilişsel Arama tüm programlama arabirimlerinin geçerli sürümlerinin bir anlık görüntüsü aşağıda verilmiştir.


| Arabirimler | En son ana sürüm | Durum |
| --- | --- | --- |
| [.NET SDK](https://aka.ms/search-sdk) |9.0 |Genel kullanıma sunulan, 2019 Mayıs |
| [.NET SDK önizlemesi](https://aka.ms/search-sdk-preview) |8,0-Önizleme |Önizleme, yayın 2019 Nisan |
| [Hizmet REST API'si](https://docs.microsoft.com/rest/api/searchservice/) |2019-05-06 |Genel olarak kullanılabilir |
| [Service REST API 2019-05-06-Önizleme](search-api-preview.md) |2019-05-06-Önizleme |Önizleme |
| [.NET Yönetim SDK'sı](https://aka.ms/search-mgmt-sdk) |3,0 |Genel olarak kullanılabilir |
| [Yönetim REST API'si](https://docs.microsoft.com/rest/api/searchmanagement/) |2020-03-13|Genel olarak kullanılabilir |

REST API 'Leri için, her bir `api-version` çağrının dahil edilmesi gerekir. Kullanarak `api-version` , önizleme API 'si gibi belirli bir sürümü hedeflemek kolaylaşır. Aşağıdaki örnek, `api-version` parametresinin nasıl belirtildiğini göstermektedir:

    GET https://my-demo-app.search.windows.net/indexes/hotels?api-version=2019-05-06

> [!NOTE]
> Her istek bir `api-version`olsa da, tüm API istekleri için aynı sürümü kullanmanızı öneririz. Bu, yeni API sürümleri önceki sürümler tarafından tanınmayan öznitelikler veya işlemler tanıtdığında özellikle doğrudur. API sürümlerinin karışması istenmeyen sonuçlara sahip olabilir ve kaçınılmalıdır.
>
> Hizmet REST API ve yönetim REST API birbirinden bağımsız olarak sürümlüdür. Sürüm numaralarında benzerlik varsa coarızdır.

Genel olarak kullanılabilir (veya GA) API 'Leri üretimde kullanılabilir ve Azure hizmet düzeyi sözleşmelerine tabidir. Önizleme sürümleri, her zaman bir GA sürümüne geçirilmeyen deneysel özelliklere sahiptir. **Üretim uygulamalarında önizleme API 'Lerini kullanmaktan kaçınmak kesinlikle önerilir.**

## <a name="update-to-the-latest-version-of-the-rest-api-by-october-15-2020"></a>15 Ekim 2020 ' e kadar REST API en son sürümüne güncelleştirin
Azure Bilişsel Arama REST API aşağıdaki sürümleri kullanımdan kaldırılacak ve 15 Ekim 2020 itibariyle artık desteklenmeyecektir: **2014-07-31-Preview**, **2014-10-20-Preview**, **2015-02-28-Preview**ve **2015-02-28**. Ayrıca, Azure Bilişsel Arama .NET SDK 'nın **3.0.0-RC** ' den daha eski sürümleri de bu REST API sürümlerinden birini hedeflerse devre dışı bırakılacaktır. Bu tarihten sonra, kullanımdan kaldırılan REST API veya SDK sürümlerinden herhangi birini kullanan uygulamalar artık çalışmaz ve yükseltilmesi gerekir. Bu türde herhangi bir değişiklik yaparken, 12 aylık bir bildirim sunuyoruz, bu nedenle ayarlamak için yeterli zaman vardır.  Azure Bilişsel Arama 'yi kullanmaya devam etmek için lütfen REST API [REST API](search-api-migration.md) hedefleyen mevcut kodu, 15 Ekim 2020 ' ye [2019-05-06](https://docs.microsoft.com/rest/api/searchservice/) veya daha yeni bir sürümü veya .NET SDK 'sını 3,0 veya daha yeni bir [sürüme](search-dotnet-sdk-migration.md) geçirin.  En son sürüme güncelleştirme hakkında sorularınız varsa, kodunuzu güncelleştirmek için yeterli zamana sahip olduğunuzdan emin azuresearch_contact@microsoft.com olmak için lütfen 15 Mayıs 2020 ' e kadar posta gönderin.

## <a name="about-preview-and-generally-available-versions"></a>Önizleme ve genel kullanıma açık sürümler hakkında
Azure Bilişsel Arama, önce REST API önce, sonra .NET SDK 'nin ön sürüm sürümlerini kullanarak deneysel özellikleri önceden yayınlar.

Önizleme özellikleri, test ve deneme için, özellik tasarımı ve uygulama hakkında geri bildirim toplama hedefi ile kullanılabilir. Bu nedenle, Önizleme özellikleri zaman içinde değişebilir, belki de geriye doğru uyumluluğu kesen şekillerde değişiklik yapabilir. Bu, bir GA sürümündeki özelliklerin aksine, bu durum, küçük ve geriye dönük olarak uyumlu düzeltmeler ve geliştirmeler dışında değişmez ve çok düşüktür. Ayrıca, Önizleme özellikleri her zaman bir GA sürümüne değildir.

Bu nedenlerden dolayı, önizleme sürümlerine bağımlılığı alan üretim kodu yazmayı öneririz. Daha eski bir önizleme sürümü kullanıyorsanız, genel kullanıma açık (GA) sürüme geçiş yapmanızı öneririz.

.NET SDK için: [.NET SDK yükseltme](search-dotnet-sdk-migration-version-9.md)sırasında kod geçişi için rehberlik bulunabilir.

Genel kullanılabilirlik, Azure Bilişsel Arama artık hizmet düzeyi sözleşmesi (SLA) altında olduğu anlamına gelir. SLA, [Azure bilişsel arama hizmet düzeyi sözleşmeleri](https://azure.microsoft.com/support/legal/sla/search/v1_0/)' nde bulunabilir.
