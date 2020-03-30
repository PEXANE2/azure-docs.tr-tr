---
title: Azure CDN profilini Verizon Standard'dan Verizon Premium'a geçirin
description: Verizon Standard'dan Verizon Premium'a profil geçirmenin ayrıntıları hakkında bilgi edinin.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2018
ms.author: magattus
ms.custom: ''
ms.openlocfilehash: 8ab66117be4f05550b00defafc883108646be283
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74083050"
---
# <a name="migrate-an-azure-cdn-profile-from-standard-verizon-to-premium-verizon"></a>Azure CDN profilini Standart Verizon'dan Premium Verizon'a geçirin

Uç noktalarınızı yönetmek için bir Azure İçerik Teslim Ağı (CDN) profili oluşturduğunuzda, Azure CDN aralarından seçim yapabileceğiniz dört farklı ürün sunar. Farklı ürünler ve kullanılabilir özellikleri hakkında daha fazla bilgi için Azure [CDN ürün özelliklerini karşılaştır'a](cdn-features.md)bakın.

Verizon profilinden bir **Azure CDN Standardı** oluşturduysanız ve CDN uç noktalarınızı yönetmek için kullanıyorsanız, bu standardı Verizon **profilinden Azure CDN Premium'a** yükseltme seçeneğiniz vardır. Yükseltme yaptığınızda, CDN uç noktalarınız ve tüm verileriniz korunur. 

> [!IMPORTANT]
> **Verizon profilinden Azure CDN Premium'a** yükselttikten sonra, daha sonra Verizon profilinden Azure **CDN Standardına** dönüştüremezsiniz.
> 

**Verizon profilinden azure CDN Standard'ı** yükseltmek için [Microsoft Destek'e](https://azure.microsoft.com/support/options/)başvurun.

## <a name="profile-comparison"></a>Profil karşılaştırması
**Verizon profillerinden Azure CDN Premium,** Verizon **profillerinden Azure CDN Standard'dan** aşağıdaki önemli farklılıklara sahiptir:
- [Sıkıştırma,](cdn-improve-performance.md) [önbelleğe alma kuralları](cdn-caching-rules.md)ve coğrafi [filtreleme](cdn-restrict-access-by-country.md)gibi belirli Azure CDN özellikleri için Azure CDN arabirimini kullanamazsınız, **Yönet** düğmesi aracılığıyla Verizon portalını kullanmanız gerekir.
- API: Standart Verizon aksine, Premium Verizon portalından erişilen bu özellikleri kontrol etmek için API kullanamazsınız. Ancak, api'yi, uç nokta oluşturma/silme, önbelleğe alınmış varlıkları temizleme/yükleme ve özel bir etki alanını etkinleştirme/devre dışı bırakma gibi diğer yaygın özellikleri denetlemek için kullanabilirsiniz.
- Fiyatlandırma: Premium Verizon Standart Verizon daha veri transferleri için farklı bir fiyatlandırma yapısına sahiptir. Daha fazla bilgi için [İçerik Dağıtım Ağı fiyatlandırması](https://azure.microsoft.com/pricing/details/cdn/)için bkz.

**Verizon profillerinden Azure CDN Premium** aşağıdaki ek özelliklere sahiptir:
- [Belirteç kimlik doğrulaması](cdn-token-auth.md): Kullanıcıların güvenli kaynakları almak için bir belirteç edinmelerine ve kullanmalarına olanak tanır.
- [Kurallar motoru](cdn-rules-engine.md): HTTP isteklerinin nasıl işleneceğini özelleştirmenizi sağlar.
- Gelişmiş analiz araçları:
   - [Ayrıntılı HTTP analitiği](cdn-advanced-http-reports.md)
   - [Üstün performans analitiği](cdn-edge-performance.md)
   - [Gerçek zamanlı analitik](cdn-real-time-alerts.md)


## <a name="next-steps"></a>Sonraki adımlar
Kurallar altyapısı hakkında daha fazla bilgi edinmek için [Azure CDN kuralları motoru başvurusuna](cdn-rules-engine-reference.md)bakın.

