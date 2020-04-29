---
title: Verizon Standard 'dan Verizon Premium 'a Azure CDN profili geçirme
description: Verizon Standard 'dan Verizon Premium 'a profil geçirme ayrıntıları hakkında bilgi edinin.
services: cdn
documentationcenter: ''
author: asudbring
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2018
ms.author: allensu
ms.custom: ''
ms.openlocfilehash: 61c472281c64fba451bb9b05f69ebee09bc763fd
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81260477"
---
# <a name="migrate-an-azure-cdn-profile-from-standard-verizon-to-premium-verizon"></a>Standart Verizon 'den Premium Verizon 'e bir Azure CDN profili geçirme

Uç noktalarınızı yönetmek için bir Azure Content Delivery Network (CDN) profili oluşturduğunuzda Azure CDN seçebileceğiniz dört farklı ürün sunar. Farklı ürünler ve kullanılabilir özellikleri hakkında daha fazla bilgi için bkz. [Azure CDN ürün özelliklerini karşılaştırma](cdn-features.md).

Verizon profilinden bir **Azure CDN standardı** OLUŞTURDUYSANıZ ve CDN uç noktalarınızı yönetmek için kullanıyorsanız, Verizon profilinden bir **Azure CDN Premium** 'a yükseltme seçeneğiniz vardır. Yükselttiğinizde, CDN uç noktalarınız ve tüm verileriniz korunur. 

> [!IMPORTANT]
> Verizon profilinden bir **Azure CDN Premium** sürümüne yükselttikten sonra, daha sonra **Verizon profilinden Azure CDN bir standarda** geri dönüştüremezsiniz.
> 

Verizon profilinden bir **Azure CDN standardını** yükseltmek için [Microsoft desteği](https://azure.microsoft.com/support/options/)başvurun.

## <a name="profile-comparison"></a>Profil karşılaştırması
**Verizon profillerinden Azure CDN Premium** , **Verizon profillerden Azure CDN Standart** olan aşağıdaki temel farklılıklara sahiptir:
- [Sıkıştırma](cdn-improve-performance.md), [önbelleğe alma kuralları](cdn-caching-rules.md)ve [coğrafi filtreleme](cdn-restrict-access-by-country.md)gibi bazı Azure CDN Özellikler Için, Azure CDN arabirimini kullanamazsınız, **Yönet** düğmesi aracılığıyla Verizon portalını kullanmanız gerekir.
- API: Standart Verizon aksine, Premium Verizon portalından erişilen özellikleri denetlemek için API 'yi kullanamazsınız. Ancak, bir uç nokta oluşturma/silme, önbelleğe alınmış varlıkları temizleme/yükleme ve özel bir etki alanını etkinleştirme/devre dışı bırakma gibi diğer yaygın özellikleri denetlemek için API 'yi kullanabilirsiniz.
- Fiyatlandırma: Premium Verizon, standart Verizon 'den veri aktarımları için farklı bir fiyatlandırma yapısına sahiptir. Daha fazla bilgi için bkz. [Content Delivery Network fiyatlandırması](https://azure.microsoft.com/pricing/details/cdn/).

**Verizon profillerden Azure CDN Premium** aşağıdaki ek özelliklere sahiptir:
- [Belirteç kimlik doğrulaması](cdn-token-auth.md): kullanıcıların güvenli kaynakları getirmek için bir belirteç almasına ve kullanmasına izin verir.
- [Kural altyapısı](cdn-rules-engine.md): http isteklerinin işlenme biçimini özelleştirmenize olanak sağlar.
- Gelişmiş analiz araçları:
   - [Ayrıntılı HTTP Analizi](cdn-advanced-http-reports.md)
   - [Uç performans analizi](cdn-edge-performance.md)
   - [Gerçek zamanlı analiz](cdn-real-time-alerts.md)


## <a name="next-steps"></a>Sonraki adımlar
Kural altyapısı hakkında daha fazla bilgi edinmek için bkz. [Azure CDN kuralları altyapısı başvurusu](cdn-rules-engine-reference.md).

