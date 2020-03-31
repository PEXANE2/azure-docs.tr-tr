---
title: Azure Ön Kapı için Web uygulaması güvenlik duvarı özel kuralı
description: Web uygulamalarınızı kötü amaçlı saldırılara karşı koruyan Web Uygulaması Güvenlik Duvarı (WAF) özel kurallarını nasıl kullanacağınızı öğrenin.
author: vhorne
ms.service: web-application-firewall
ms.topic: article
services: web-application-firewall
ms.date: 09/05/2019
ms.author: victorh
ms.openlocfilehash: 158bfe30bf48ee420be8efb9ff32fff0e555d9e7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79475833"
---
#  <a name="custom-rules-for-web-application-firewall-with-azure-front-door"></a>Azure Ön Kapı ile Web Uygulaması Güvenlik Duvarı için özel kurallar

Ön Kapılı Azure Web Uygulama Güvenlik Duvarı (WAF), tanımladığınız koşullara bağlı olarak web uygulamalarınıza erişimi denetlemenize olanak tanır. Özel bir WAF kuralı öncelik numarası, kural türü, eşleşme koşulları ve bir eylemden oluşur. İki tür özel kural vardır: maç kuralları ve fiyat sınırı kuralları. Eşmatch kuralı, bir oran sınırı kuralı eşleşen koşullara ve gelen istek oranlarına göre erişimi denetlerken, eşleşen koşullar kümesine göre erişimi denetler. Değerlendirilmesini önlemek için özel bir kuralı devre dışı kılabilir, ancak yapılandırmayı yine de saklarsınız. 

## <a name="priority-match-conditions-and-action-types"></a>Öncelik, eşleştirme koşulları ve eylem türleri

Erişimi, öncelik numarası, kural türü, bir dizi eşleşme koşulu ve bir eylemi tanımlayan özel bir WAf kuralıyla denetleyebilirsiniz. 

- **Öncelik:** WAF kurallarının değerlendirilmesi sırasını açıklayan benzersiz bir tamsayıdır. Daha düşük öncelik değerlerine sahip kurallar, daha yüksek değerlere sahip kurallardan önce değerlendirilir. Öncelik numaraları tüm özel kurallar arasında benzersiz olmalıdır.

- **Eylem:** WAF kuralı eşleşirse isteğin nasıl yönlendirilenin ibaresini tanımlar. Bir istek özel bir kuralla eşleştiğinde uygulanacak aşağıdaki eylemlerden birini seçebilirsiniz.

    - *İzin ver* - WAF arayışı arka uca ileter, WAF günlüklerinde ve çıkışlarında bir girişi kaydeder.
    - *Blok* - İstek engellenir, WAF isteği arka uca iletmeden istemciye yanıt gönderir. WAF, WAF günlüklerinde bir girişi kaydeder.
    - *Günlük* - WAF, WAF günlüklerinde bir girişi kaydeder ve bir sonraki kuralı değerlendirmeye devam eder.
    - *Yönlendirme* - WAF isteği belirli bir URI'ye yönlendirir, WAF günlüklerinde bir giriş kaydeder ve çıkışlar.

- **Eşleştirme koşulu:** bir eşleşme değişkeni, bir işleç ve eşleme değeri tanımlar. Her kural birden çok eşleşme koşulu içerebilir. Eşleşme koşulu coğrafi konuma, istemci IP adreslerine (CIDR), boyut veya dize eşleşmesi temel alabilir. String eşleşmesi eşleme değişkenleri listesine karşı olabilir.
  - **Eşleştirme değişkeni:**
    - İstek Yöntemi
    - QueryString
    - PostArgs
    - Requesturi
    - İstek Header
    - İstek Gövdesi
    - Tanımlama bilgileri
  - **Işleç:**
    - Any: kural eşleşmezse genellikle varsayılan eylemi tanımlamak için kullanılır. Herhangi bir maç tüm operatör.
    - Eşittir
    - Contains
    - LessThan: boyut kısıtlaması
    - GreaterThan: boyut kısıtlaması
    - LessThanOrEqual: boyut kısıtlaması
    - GreaterThanOrEqual: boyut kısıtlaması
    - BeginsWith
    - EndsWith
    - Regex
  
  - **Regex** aşağıdaki işlemleri desteklemez: 
    - Geri göndermeler ve alt ifadeleri yakalama
    - Rasgele sıfır genişlik iddiaları
    - Alt yordamreferansları ve özyinelemeli desenler
    - Koşullu desenler
    - Geri izleme denetim fiilleri
    - \C tek bayt yönergesi
    - \R newline eşleşmesi yönergesi
    - Maç sıfırlama yönergesinin \K başlangıcı
    - Ek çalışmalar ve gömülü kod
    - Atomik gruplama ve sahiplenici niceleyiciler

  - **[isteğe bağlı]:** Bir koşulun sonucu inkar *edilirse, negate* koşulunu doğru ayarlayabilirsiniz.
      
  - **[isteğe bağlı]dönüştürün:** Eşleşmeden önce yapılacak dönüşümadlarının bulunduğu dizelerin listesi denenir. Aşağıdaki dönüşümler olabilir:
     - Büyük harfe 
     - Küçük harf
     - Trim
     - RemoveNulls
     - UrlDecode
     - UrlEncode
     
   - **Maç değeri:** Desteklenen HTTP istek yöntemi değerleri şunlardır:
     - GET
     - POST
     - PUT
     - HEAD
     - DELETE
     - Kilit
     - Kilidini
     - PROFİL
     - Seçenekler
     - PROPFIND
     - Proppatch
     - Mkcol
     - Kopya
     - Hareket

## <a name="examples"></a>Örnekler

### <a name="waf-custom-rules-example-based-on-http-parameters"></a>WAF özel kurallar örneği http parametrelerine dayalı

Aşağıda, iki eşleşme koşuluyla özel bir kuralın yapılandırmasını gösteren bir örnek verilmiştir. İstekler, başvurucu tarafından tanımlandığı şekilde belirli bir siteden gelir ve sorgu dizesi "parola" içermez.

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
"PUT" yöntemini engellemek için örnek bir yapılandırma aşağıdaki gibi gösterilmiştir:

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

Gelen isteğin bir bölümünde boyut kısıtlaması belirten özel bir kural oluşturabilirsiniz. Örneğin, aşağıda kural 100 karakterden uzun bir Url'yi engeller.

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
- [Azure PowerShell'i kullanarak Bir Web Uygulaması Güvenlik Duvarı ilkesini yapılandırma](waf-front-door-custom-rules-powershell.md) 
- Ön [Kapılı web Uygulaması Güvenlik Duvarı](afds-overview.md) hakkında bilgi edinin
- [Front Door oluşturmayı](../../frontdoor/quickstart-create-front-door.md) öğrenin.

