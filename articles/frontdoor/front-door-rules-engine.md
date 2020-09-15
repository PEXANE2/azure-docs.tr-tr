---
title: Azure ön kapı mimarisi ve terminolojisi için kural altyapısı
description: Bu makalede, Azure ön kapısı kuralları altyapı özelliğine bir genel bakış sunulmaktadır.
services: frontdoor
documentationcenter: ''
author: duongau
editor: ''
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 9/14/2020
ms.author: duau
ms.openlocfilehash: 94770e4fcad9659594854f81732c7b4e4a97051c
ms.sourcegitcommit: 03662d76a816e98cfc85462cbe9705f6890ed638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90530888"
---
# <a name="what-is-rules-engine-for-azure-front-door"></a>Azure ön kapısının kural Altyapısı nedir? 

Rules Engine, HTTP isteklerinin kenarda nasıl işlendiğini özelleştirmenizi ve Web uygulamanızın davranışında daha fazla denetim sağladığını sağlar. Azure ön kapısının kural altyapısı, aşağıdakiler de dahil olmak üzere çeşitli önemli özelliklerden oluşur:

- HTTPS 'yi zorunlu kılmak için, tüm son kullanıcılarınızın güvenli bir bağlantı üzerinden içeriklerle etkileşimde bulunduğundan emin olun.
- HTTP Strict-Transport-Security (HSTS), X-XSS-koruma, Content-Security-Policy, X-Frame-Options gibi tarayıcı tabanlı güvenlik açıklarını ve çıkış noktaları arası kaynak paylaşımı (CORS) senaryoları için erişim-denetim-Izin-kaynak üst bilgilerini engellemek için güvenlik üst bilgilerini uygulayın. Güvenlik tabanlı öznitelikler, tanımlama bilgileriyle de tanımlanabilir.
- İstek üst bilgileri, tanımlama bilgileri veya sorgu dizeleri içeriğindeki desenleri temel alarak, istekleri uygulamanızın mobil veya masaüstü sürümlerine yönlendirin.
- Yeni konak adları, yolları veya protokolleri yeniden yönlendirmek üzere istemciye 301, 302, 307 ve 308 yeniden yönlendirme özelliklerini döndürmek için yeniden yönlendirme yeteneklerini kullanın.
- Yönlendirmenize ait önbelleğe alma yapılandırmasını gelen isteklere göre dinamik olarak değiştirin.
- İstek URL 'SI yolunu yeniden yazın ve isteği yapılandırılmış arka uç havuzunuzdaki uygun arka uca iletin.

## <a name="architecture"></a>Mimari 

Kural altyapısı, istekleri kenarda işler. Kural altyapısını yapılandırdıktan sonra, bir istek ön kapı uç noktanıza rastladıktan sonra, önce WAF yürütülür ve ardından ön uç/etki alanınız ile ilişkili kurallar altyapısı yapılandırması gelir. Bir kural Altyapısı yapılandırması yürütüldüğünde, üst yönlendirme kuralının zaten bir eşleşme olduğu anlamına gelir. Kural altyapısı yapılandırmasındaki kuralların her birinde gerçekleştirilen tüm eylemlerin, bu kural içindeki tüm eşleşme koşullarına tabi olup olmadığı. Bir istek, kural altyapısı yapılandırmanızda hiçbir koşuldan biriyle eşleşiyorsa, varsayılan yönlendirme kuralı yürütülür. 

Örneğin, aşağıdaki yapılandırmada, bir kural altyapısı, eşleşme koşulu karşılanırsa önbellek denetiminin maksimum yaşını değiştiren bir yanıt üst bilgisi eklemek üzere yapılandırılmıştır. 

![Yanıt üst bilgisi eylemi](./media/front-door-rules-engine/rules-engine-architecture-3.png)

Başka bir örnekte, kural altyapısının, eşleşme koşulu, cihaz türü true olduğunda bir kullanıcıyı sitenin mobil sürümüne gönderecek şekilde yapılandırıldığını görüyoruz. 

![Yönlendirme yapılandırması geçersiz kılma](./media/front-door-rules-engine/rules-engine-architecture-1.png)

Bu örneklerin her ikisinde de eşleşme koşullarından hiçbiri karşılanmazsa, belirtilen yol kuralı yürütülür. 

## <a name="terminology"></a>Terminoloji 

AFD kural altyapısı ile, her biri bir kural kümesinden oluşan bir dizi kural Altyapısı yapılandırması oluşturabilirsiniz. Aşağıda, kural motorunuz yapılandırılırken daha fazla yararlı terminoloji özetlenmektedir. 

- *Kural Altyapısı yapılandırması*: tek yol kuralına uygulanan bir kurallar kümesi. Her yapılandırma 25 kuralla sınırlıdır. En fazla 10 yapılandırma oluşturabilirsiniz. 
- *Kural altyapısı kuralı*: en fazla 10 eşleşme koşulu ve 5 eylemden oluşan bir kural.
- *Koşulu Eşleştir*: gelen isteklerinizi ayrıştırmak için kullanılabilecek çok sayıda eşleşme koşulu vardır. Bir kural, en fazla 10 eşleşme koşulu içerebilir. Eşleşme koşulları bir **ve** işleciyle değerlendirilir. Eşleşen koşulların tam bir listesi [burada](front-door-rules-engine-match-conditions.md)bulunabilir. 
- *Eylem*: Eylemler, gelen isteklerinize ne olduğunu belirler-istek/yanıt üst bilgisi eylemleri, iletme, yeniden yönlendirmeler ve yeniden gönderme işlemleri bugün kullanılabilir. Bir kural, en fazla 5 eylem içerebilir; Ancak, bir kural yalnızca 1 yol yapılandırması geçersiz kılma içerebilir.  Eylemlerin tam listesini [burada](front-door-rules-engine-actions.md)bulabilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

- İlk [kural altyapısı yapılandırmanızı](front-door-tutorial-rules-engine.md)ayarlamayı öğrenin. 
- [Front Door oluşturmayı](quickstart-create-front-door.md) öğrenin.
- [Front Door’un nasıl çalıştığını](front-door-routing-architecture.md) öğrenin.
