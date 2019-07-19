---
title: Azure ön kapısının Web uygulaması güvenlik duvarı özel kuralı
description: Web uygulaması güvenlik duvarı (WAF) özel kurallarını kullanarak Web uygulamalarınızı kötü amaçlı saldırılara karşı koruma hakkında bilgi edinin.
author: KumudD
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/07/2019
ms.author: kumud
ms.reviewer: tyao
ms.openlocfilehash: 02b335de7f105d768168d5f798ec9109136d7430
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2019
ms.locfileid: "67846273"
---
#  <a name="custom-rules-for-web-application-firewall-with-azure-front-door"></a>Azure ön kapısına sahip Web uygulaması güvenlik duvarı için özel kurallar
Ön kapı hizmeti olan Azure Web uygulaması güvenlik duvarı (WAF), tanımladığınız koşullara göre Web uygulamalarınıza erişimi denetlemenize olanak tanır. Özel bir WAF kuralı bir öncelik numarası, bir kural türü, eşleşme koşulları ve bir eylemden oluşur. İki tür özel kural vardır: eşleştirme kuralları ve hız sınırı kuralları. Bir eşleştirme kuralı, eşleşen koşullara göre erişimi denetler, ancak bir hız sınırı kuralı, eşleşen koşullara ve gelen isteklerin ücretlerine göre erişimi denetler. Hesaplanmasını önlemek için özel bir kuralı devre dışı bırakabilirsiniz, ancak yine de yapılandırmayı devam edebilirsiniz. Bu makalede, http parametrelerine dayanan eşleşme kuralları ele alınmaktadır.

## <a name="priority-match-conditions-and-action-types"></a>Öncelik, eşleşme koşulları ve eylem türleri
Bir öncelik numarası, bir kural türü, eşleşme koşulları ve bir eylem tanımlayan özel bir WAf kuralıyla erişimi denetleyebilirsiniz. 

- **Priority:** WAF kurallarının değerlendirilme sırasını açıklayan benzersiz bir tamsayıdır. Daha düşük değerlere sahip kurallar, daha yüksek değerlere sahip kurallardan önce değerlendirilir

- **Eylem:** bir WAF kuralı eşleştirildiği takdirde bir isteğin nasıl yönlendirileceğini tanımlar. Bir istek özel bir kuralla eşleştiğinde uygulanacak aşağıdaki eylemlerden birini seçebilirsiniz.

    - *Allow* -WAF, isteği arka uca iletir, WAF günlüklerinde bir girişi günlüğe kaydeder ve çıkar.
    - *Blok* -istek engellendi, WAF, isteği arka uca iletmeksizin istemciye yanıt gönderiyor. WAF, WAF günlüklerinde bir girişi günlüğe kaydeder.
    - *Log* -WAF WAF günlüklerinde bir girişi günlüğe kaydeder ve sonraki kuralı değerlendir.
    - *Yeniden yönlendirme* -WAF isteği BELIRTILEN bir URI 'ye yönlendirir, WAF günlüklerinde bir girişi günlüğe kaydeder ve çıkar.

- **Koşulu eşleştir:** bir Match değişkeni, işleç ve Match değeri tanımlar. Her kural birden fazla eşleşme koşulu içerebilir. Bir eşleşme koşulu aşağıdaki *eşleşme değişkenlerini*temel alabilir:
    - RemoteAddr (istemci IP 'si)
    - RequestMethod
    - QueryString
    - PostArgs
    - RequestUri
    - RequestHeader
    - Istek gövdesi

- **İşleç:** List şunları içerir:
    - Any: herhangi bir kural eşleştirilyoksa varsayılan eylemi tanımlamak için genellikle kullanılır. Any, Match All işleçtir.
    - IPMatch: RemoteAddr değişkeni için IP kısıtlaması tanımlayın
    - GeoMatch: RemoteAddr değişkeni için coğrafi filtreleme tanımlayın
    - Sıfıra
    - İçerir
    - LessThan: boyut kısıtlaması
    - GreaterThan: boyut kısıtlaması
    - Yetersiz okarşılandığından al: boyut kısıtlaması
    - GreaterThanOrEqual: boyut kısıtlaması
    - Ile başlıyor
     - EndsWith

Bir koşulun sonucu bir değer elde edilmeliyse, *Negate* koşulunu true olarak ayarlayabilirsiniz.

*Match değeri* , olası eşleşme değerlerinin listesini tanımlar.
Desteklenen HTTP istek yöntemi değerleri şunlardır:
- GET
- POST
- PUT
- BAŞLI
- DELETE
- INE
- KALDIRIN
- PROFILINIZI
- SEÇENEKLER
- FIND
- PRO
- MKCOL
- KOPYA
- GEÇIŞ

## <a name="examples"></a>Örnekler

### <a name="waf-custom-rules-example-based-on-http-parameters"></a>Http parametrelerine göre WAF özel kurallar örneği

İki eşleşme koşulu olan özel bir kuralın yapılandırmasını gösteren bir örnek aşağıda verilmiştir. İstekler, başvuran tarafından tanımlanan belirli bir siteden ve sorgu dizesinde "Password" bulunmaz.

```
# http rules example
{
  "name": "AllowFromTrustedSites",
  "priority": 1,
  "ruleType": "MatchRule",
  "matchConditions": [
    {
      "matchVariable": "RequestHeader",
      "selector": "Referer",
      "operator": "Equal",
      "negateCondition": false,
      "matchValue": [
        "www.mytrustedsites.com/referpage.html"
      ]
    },
    {
      "matchVariable": "QueryString",
      "operator": "Contains",
      "matchValue": [
        "password"
      ],
      "negateCondition": true
    }
  ],
  "action": "Allow",
  "transforms": []
}

```
"PUT" metodunu engellemeye yönelik örnek bir yapılandırma aşağıda gösterildiği gibi gösterilmiştir:

``` 
# http Request Method custom rules
{
  "name": "BlockPUT",
  "priority": 2,
  "ruleType": "MatchRule",
  "matchConditions": [
    {
      "matchVariable": "RequestMethod",
      "selector": null,
      "operator": "Equal",
      "negateCondition": false,
      "matchValue": [
        "PUT"
      ]
    }
  ],
  "action": "Block",
  "transforms": []
}
```

### <a name="size-constraint"></a>Boyut kısıtlaması

Gelen isteğin bir bölümünde boyut kısıtlamasını belirten özel bir kural oluşturabilirsiniz. Örneğin, aşağıdaki kural 100 karakterden daha uzun bir URL 'Yi engelliyor.

```
# http parameters size constraint
{
  "name": "URLOver100",
  "priority": 5,
  "ruleType": "MatchRule",
  "matchConditions": [
    {
      "matchVariable": "RequestUri",
      "selector": null,
      "operator": "GreaterThanOrEqual",
      "negateCondition": false,
      "matchValue": [
        "100"
      ]
    }
  ],
  "action": "Block",
  "transforms": []
}
```

## <a name="next-steps"></a>Sonraki adımlar
- [Web uygulaması güvenlik duvarı](waf-overview.md) hakkında bilgi edinin
- [Front Door oluşturmayı](quickstart-create-front-door.md) öğrenin.

