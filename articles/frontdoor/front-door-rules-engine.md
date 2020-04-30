---
title: Azure ön kapısı | Microsoft Docs
description: Bu makalede Azure Front Door’a genel bir bakış sağlanır. Uygulamanız için Yük Dengeleme Kullanıcı trafiği için doğru seçim olup olmadığını öğrenin.
services: frontdoor
documentationcenter: ''
author: megan-beatty
editor: ''
ms.service: frontdoor
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 4/30/2020
ms.author: mebeatty
ms.openlocfilehash: 19deb763c8e750490854892c90d0293d3e209c09
ms.sourcegitcommit: eaec2e7482fc05f0cac8597665bfceb94f7e390f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82515555"
---
# <a name="what-is-rules-engine-for-azure-front-door"></a>Azure ön kapısının kural Altyapısı nedir? 

Rules Engine, HTTP isteklerinin kenarda nasıl işlendiğini özelleştirmenizi ve Web uygulamanızın davranışında daha fazla denetim sağladığını sağlar. Azure ön kapısının kural altyapısı, aşağıdakiler de dahil olmak üzere çeşitli önemli özelliklerden oluşur:

- Üst bilgi tabanlı yönlendirme – istek üst bilgileri, tanımlama bilgileri ve sorgu dizelerinin içeriklerindeki desenleri temel alarak istekleri yönlendirin.
- Parametre tabanlı yönlendirme – istekleri HTTP istek parametrelerine göre yönlendirmek için post args, sorgu dizeleri, tanımlama bilgileri ve istek yöntemleri dahil olmak üzere bir dizi eşleştirme koşullarından yararlanın. 
- Yol yapılandırmalarının geçersiz kılmaları: 
    - Yeni konak adları, yolları ve protokolleri yeniden yönlendirmek üzere istemciye 301/302/307/308 yeniden yönlendirme döndürmek için yeniden yönlendirme özelliklerini kullanın. 
    - İstek URL yolunu geleneksel bir yeniden yönlendirme yapmadan yeniden yazmak ve isteği yapılandırılmış arka uç havuzunuzdaki uygun arka uca iletmek için iletme özelliklerini kullanın. 
    - Önbelleğe alma yapılandırmanızı özelleştirin ve eşleştirme koşullarına bağlı olarak, bir yolun iletilmesini dinamik olarak değiştirin. 

> [!IMPORTANT]
> Bu genel önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılmamalıdır. Belirli özellikler desteklenmiyor olabilir, kısıtlı yeteneklere sahip olabilir veya tüm Azure konumlarında mevcut olmayabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

## <a name="architecture"></a>Mimari 

Kural altyapısı, istekleri kenarda işler. Kural altyapısını yapılandırdıktan sonra, bir istek ön kapı uç noktanıza rastladıktan sonra, önce WAF yürütülür ve ardından ön uç/etki alanınız ile ilişkili kurallar altyapısı yapılandırması gelir. Bir kural Altyapısı yapılandırması yürütüldüğünde, üst yönlendirme kuralının zaten bir eşleşme olduğu anlamına gelir. Kural altyapısı yapılandırmasındaki kuralların her birinde gerçekleştirilen tüm eylemlerin, bu kural içindeki tüm eşleşme koşullarına tabi olup olmadığı. Bir istek, kural altyapısı yapılandırmanızda hiçbir koşuldan biriyle eşleşiyorsa, varsayılan yönlendirme kuralı yürütülür. 

Örneğin, aşağıdaki yapılandırmada, bir kural altyapısı, eşleşme koşulu karşılanırsa önbellek denetiminin maksimum yaşını değiştiren bir yanıt üst bilgisi eklemek üzere yapılandırılmıştır. 

![Yanıt üst bilgisi eylemi](./media/front-door-rules-engine/rules-engine-architecture-3.png)

Başka bir örnekte, kural altyapısının, eşleşme koşulu, cihaz türü true olduğunda bir kullanıcıyı sitenin mobil sürümüne gönderecek şekilde yapılandırıldığını görüyoruz. 

![Yönlendirme yapılandırması geçersiz kılma](./media/front-door-rules-engine/rules-engine-architecture-1.png)

Bu örneklerin her ikisinde de eşleşme koşullarından hiçbiri karşılanmazsa, belirtilen yol kuralı yürütülür. 

## <a name="terminology"></a>Terminoloji 

AFD kural altyapısı ile, her biri bir kural kümesinden oluşan bir dizi kural Altyapısı yapılandırması oluşturabilirsiniz. Aşağıda, kural motorunuz yapılandırılırken daha fazla yararlı terminoloji özetlenmektedir. 

- *Kural Altyapısı yapılandırması*: tek yol kuralına uygulanan bir kurallar kümesi. Her yapılandırma 5 kuralla sınırlıdır. En fazla 10 yapılandırma oluşturabilirsiniz. 
- *Kural altyapısı kuralı*: en fazla 10 eşleşme koşulu ve 5 eylemden oluşan bir kural.
- *Koşulu Eşleştir*: gelen isteklerinizi ayrıştırmak için kullanılabilecek çok sayıda eşleşme koşulu vardır. Bir kural, en fazla 10 eşleşme koşulu içerebilir. Eşleşme koşulları bir **ve** işleciyle değerlendirilir. Eşleşen koşulların tam bir listesi [burada](front-door-rules-engine-match-conditions.md)bulunabilir. 
- *Eylem*: Eylemler, gelen isteklerinize ne olduğunu belirler-istek/yanıt üst bilgisi eylemleri, iletme, yeniden yönlendirmeler ve yeniden gönderme işlemleri bugün kullanılabilir. Bir kural, en fazla 5 eylem içerebilir; Ancak, bir kural yalnızca 1 yol yapılandırması geçersiz kılma içerebilir.  Eylemlerin tam listesini [burada](front-door-rules-engine-actions.md)bulabilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

- İlk [kural altyapısı yapılandırmanızı](front-door-tutorial-rules-engine.md)ayarlamayı öğrenin. 
- [Front Door oluşturmayı](quickstart-create-front-door.md) öğrenin.
- [Front Door’un nasıl çalıştığını](front-door-routing-architecture.md) öğrenin.
