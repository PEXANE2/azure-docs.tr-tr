---
title: .NET SDK ve REST API 'Ler için API sürüm yönetimi-Azure Search
description: Azure Search REST API 'Leri ve .NET SDK içindeki istemci kitaplığı için sürüm ilkesi.
author: brjohnstmsft
manager: nitinme
services: search
ms.service: search
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: brjohnst
ms.openlocfilehash: f400c6fcb2b35e2adcf605c96bb802041cd9e0a9
ms.sourcegitcommit: 7a6d8e841a12052f1ddfe483d1c9b313f21ae9e6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70182345"
---
# <a name="api-versions-in-azure-search"></a>Azure Search API sürümleri
Azure Search, özellik güncelleştirmelerini düzenli olarak kaydeder. Bazen, her zaman olmasa da, geriye dönük uyumluluğu korumak için bu güncelleştirmeler API 'nin yeni bir sürümünü gerektirir. Yeni bir sürümü yayımlamak, kodunuzda arama hizmeti güncelleştirmelerini ne zaman ve nasıl tümleştirmenizi denetlemenize olanak tanır.

Bir kural olarak, Azure Search ekibi, kodunuzu yeni bir API sürümü kullanmak üzere yükseltmek için biraz çaba içerebileceği için, yalnızca gerektiğinde yeni sürümler yayımlar. Yeni bir sürüm yalnızca API 'nin bazı yönlerinin geriye dönük uyumluluğu kesen bir şekilde değişmesi durumunda gereklidir. Bu tür değişiklikler, mevcut özelliklerle ilgili düzeltmeler veya var olan API yüzey alanını değiştiren yeni özelliklerden kaynaklanıyor olabilir.

Aynı kural SDK güncelleştirmeleri için de geçerlidir. Azure Search SDK, anlam sürümü [oluşturma](https://semver.org/) kurallarını izler, yani sürümü üç bölümden oluşur: Ana, ikincil ve derleme numarası (örneğin, 1.1.0). SDK 'nın yeni bir ana sürümü yalnızca geriye dönük uyumluluğu kesen değişiklikler için serbest bırakılır. Önemli olmayan özellik güncelleştirmeleri ikincil sürümü artırır ve hata düzeltmeleri yalnızca derleme sürümünü artırır.

> [!NOTE]
> Azure Search hizmet örneğiniz, en son sürüm dahil olmak üzere birkaç REST API sürümü destekler. Artık en son bir sürüm olmadığında bir sürümü kullanmaya devam edebilirsiniz, ancak kodunuzu en yeni sürümü kullanmak için geçirmeniz önerilir. REST API kullanırken, API sürümü parametresi aracılığıyla her istekte API sürümünü belirtmeniz gerekir. .NET SDK kullanıldığında, kullanmakta olduğunuz SDK sürümü REST API ilgili sürümünü belirler. Daha eski bir SDK kullanıyorsanız, hizmet daha yeni bir API sürümünü destekleyecek şekilde yükseltilse bile, bu kodu hiçbir değişiklik yapmadan çalıştırmaya devam edebilirsiniz.

## <a name="snapshot-of-current-versions"></a>Geçerli sürümlerin anlık görüntüsü
Azure Search için tüm programlama arabirimlerinin geçerli sürümlerinin bir anlık görüntüsü aşağıda verilmiştir.


| Arabirimler | En son ana sürüm | Durum |
| --- | --- | --- |
| [.NET SDK](https://aka.ms/search-sdk) |9.0 |Genel kullanıma sunulan, 2019 Mayıs |
| [.NET SDK önizlemesi](https://aka.ms/search-sdk-preview) |8,0-Önizleme |Önizleme, yayın 2019 Nisan |
| [Hizmet REST API'si](https://docs.microsoft.com/rest/api/searchservice/) |2019-05-06 |Genel Kullanıma Sunuldu |
| [Service REST API 2019-05-06-Önizleme](search-api-preview.md) |2019-05-06-Önizleme |Önizleme |
| [.NET Yönetim SDK'sı](https://aka.ms/search-mgmt-sdk) |3,0 |Genel Kullanıma Sunuldu |
| [Yönetim REST API'si](https://docs.microsoft.com/rest/api/searchmanagement/) |2015-08-19 |Genel Kullanıma Sunuldu |

REST API 'leri için, her bir `api-version` çağrının dahil edilmesi gerekir. Kullanarak `api-version` , önizleme API 'si gibi belirli bir sürümü hedeflemek kolaylaşır. Aşağıdaki örnek, `api-version` parametresinin nasıl belirtildiğini göstermektedir:

    GET https://my-demo-app.search.windows.net/indexes/hotels?api-version=2019-05-06

> [!NOTE]
> Her istek bir `api-version`olsa da, tüm API istekleri için aynı sürümü kullanmanızı öneririz. Bu, yeni API sürümleri önceki sürümler tarafından tanınmayan öznitelikler veya işlemler tanıtdığında özellikle doğrudur. API sürümlerinin karışması istenmeyen sonuçlara sahip olabilir ve kaçınılmalıdır.
>
> Hizmet REST API ve yönetim REST API birbirinden bağımsız olarak sürümlüdür. Sürüm numaralarında benzerlik varsa coarızdır.

Genel olarak kullanılabilir (veya GA) API 'Leri üretimde kullanılabilir ve Azure hizmet düzeyi sözleşmelerine tabidir. Önizleme sürümleri, her zaman bir GA sürümüne geçirilmeyen deneysel özelliklere sahiptir. **Üretim uygulamalarında önizleme API 'Lerini kullanmaktan kaçınmak kesinlikle önerilir.**

## <a name="about-preview-and-generally-available-versions"></a>Önizleme ve genel kullanıma açık sürümler hakkında
Azure Search her zaman deneysel özellikleri önce REST API, sonra .NET SDK 'nin yayın öncesi sürümleri aracılığıyla serbest bırakır.

Önizleme özellikleri, test ve deneme için, özellik tasarımı ve uygulama hakkında geri bildirim toplama hedefi ile kullanılabilir. Bu nedenle, Önizleme özellikleri zaman içinde değişebilir, belki de geriye doğru uyumluluğu kesen şekillerde değişiklik yapabilir. Bu, bir GA sürümündeki özelliklerin aksine, bu durum, küçük ve geriye dönük olarak uyumlu düzeltmeler ve geliştirmeler dışında değişmez ve çok düşüktür. Ayrıca, Önizleme özellikleri her zaman bir GA sürümüne değildir.

Bu nedenlerden dolayı, önizleme sürümlerine bağımlılığı alan üretim kodu yazmayı öneririz. Daha eski bir önizleme sürümü kullanıyorsanız, genel kullanıma açık (GA) sürüme geçiş yapmanızı öneririz.

.NET SDK için: [.NET SDK yükseltme](search-dotnet-sdk-migration-version-9.md)sırasında kod geçişine yönelik rehberlik bulunabilir.

Genel kullanılabilirlik, Azure Search artık hizmet düzeyi sözleşmesi (SLA) altında olduğu anlamına gelir. SLA [Azure Search hizmet düzeyi sözleşmelerinden](https://azure.microsoft.com/support/legal/sla/search/v1_0/)bulunabilir.
