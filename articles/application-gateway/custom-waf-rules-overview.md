---
title: Azure Web uygulaması güvenlik duvarı (WAF) v2 özel kuralları
description: Bu makalede, Azure Application Gateway 'de Web uygulaması güvenlik duvarı (WAF) v2 özel kurallarına genel bir bakış sunulmaktadır.
services: application-gateway
ms.topic: article
author: vhorne
ms.service: application-gateway
ms.date: 6/18/2019
ms.author: victorh
ms.openlocfilehash: 2e96a2a2dd5504c906b5fb84b643467a83518f21
ms.sourcegitcommit: d47a30e54c5c9e65255f7ef3f7194a07931c27df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027575"
---
# <a name="overview-custom-rules-for-web-application-firewall-v2"></a>Genel Bakış: Web uygulaması güvenlik duvarı v2 için özel kurallar

Azure Application Gateway Web uygulaması güvenlik duvarı (WAF) v2, birçok farklı saldırı türünden koruma sağlayan, önceden yapılandırılmış, platform tarafından yönetilen bir kural kümesiyle birlikte gelir. Bu saldırılar siteler arası betik oluşturma, SQL ekleme ve diğer kişiler de içerir. Bir WAF yöneticisiyseniz, çekirdek kural kümesi kurallarını genişletmek için kendi kurallarınızı yazmak isteyebilirsiniz. Kurallarınız, eşleşen ölçütlere göre istenen trafiği engelleyebilir veya izin verebilir.

Özel kurallarla, WAF üzerinden geçen her istek için değerlendirilen kendi kurallarınızı oluşturabilirsiniz. Bu kurallar, yönetilen kural kümelerindeki kuralların geri kalanından daha yüksek bir öncelik tutar. Özel kurallar bir kural adı, kural önceliği ve eşleşen koşullardan oluşan bir dizi içerir. Bu koşullar karşılanıyorsa, izin vermek veya engellemek için bir eylem yapılır.

Örneğin, 192.168.5.4/24 aralığındaki bir IP adresinden gelen tüm istekleri engellemek için bir kural oluşturabilirsiniz. Bu kuralda, işleç *Ipmatch*olur, *matchvalues* IP adres aralığıdır (192.168.5.4/24) ve *işlem* trafiği engeller. Kuralın adını ve önceliğini de ayarlarsınız.

Özel kurallar, güvenlik gereksinimlerinizi karşılayan daha gelişmiş kurallar oluşturmak için bileşik mantık kullanımını destekler. Örneğin, "(koşul 1 *ve* koşul 2) *veya* koşul 3)", koşul 1 *ve* koşul 2 karşılanırsa *veya* koşul 3 karşılanıyorsa, WAF 'in özel kuralda belirtilen eylemi yapması gerekir.

Aynı kural içindeki farklı eşleşen koşullar, *ve*kullanarak her zaman bileşik bir şekilde yapılır. Örneğin, *ve* kullanan bir kural belırlı bir IP adresinden gelen trafiği engellemeyi ve yalnızca belirli bir tarayıcı kullanılıyorsa belirtebilir.

*Ya da* iki farklı koşul için kullanmak istiyorsanız, iki koşulun farklı kurallarda olması gerekir. Örneğin, *veya* kullanan kural belırlı bir IP adresinden trafiği engellemeyi veya belirli bir tarayıcı kullanılıyorsa trafiği engellemeyi belirtebilir.

> [!NOTE]
> WAF özel kural sayısı üst sınırı 100 ' dir. Application Gateway limitleri hakkında daha fazla bilgi için bkz. [Azure aboneliği ve hizmet limitleri, Kotalar ve kısıtlamalar](../azure-subscription-service-limits.md#application-gateway-limits).

Normal ifadeler, çekirdek kural kümelerinde olduğu gibi özel kurallarda de desteklenir. Bu kuralların örnekleri için [özel Web uygulaması güvenlik duvarı kuralları oluşturma ve kullanma](create-custom-waf-rules.md)bölümünde "örnek 3" ve "örnek 5" başlığına bakın.

> [!NOTE]
> Özel kurallar v1 SKU 'SU WAF ' de kullanılamaz.

## <a name="allowing-or-blocking-traffic"></a>Trafiğe izin verme veya trafiği engelleme

Trafiğe izin verilmesi veya engellenmesi özel kurallarla basittir. Örneğin, bir IP adresi aralığından gelen tüm trafiği engelleyebilirsiniz. İstek belirli bir tarayıcıdan geliyorsa, trafiğe izin vermek için başka bir kural yapabilirsiniz.

Bir şeye izin vermek için `-Action` parametresinin **Izin ver**olarak ayarlandığından emin olun. Bir şeyi engellemek için, aşağıdaki kodda gösterildiği gibi `-Action` parametresinin **Engelle**olarak ayarlandığından emin olun:

```azurepowershell
$AllowRule = New-AzApplicationGatewayFirewallCustomRule `
   -Name example1 `
   -Priority 2 `
   -RuleType MatchRule `
   -MatchCondition $condition `
   -Action Allow

$BlockRule = New-AzApplicationGatewayFirewallCustomRule `
   -Name example2 `
   -Priority 2 `
   -RuleType MatchRule `
   -MatchCondition $condition `
   -Action Block
```

Yukarıdaki `$BlockRule` Azure Resource Manager aşağıdaki özel kurala eşlenir:

```json
"customRules": [
      {
        "name": "blockEvilBot",
        "priority": 2,
        "ruleType": "MatchRule",
        "action": "Block",
        "matchConditions": [
          {
            "matchVariables": [
              {
                "variableName": "RequestHeaders",
                "selector": "User-Agent"
              }
            ],
            "operator": "Contains",
            "negationConditon": false,
            "matchValues": [
              "evilbot"
            ],
            "transforms": [
              "Lowercase"
            ]
          }
        ]
      }
    ], 
```

Bu özel kural bir ad, öncelik, eylem ve eylemin gerçekleşmesi için karşılanması gereken eşleşen koşulların bir dizisini içerir. Özel kural alanlarının açıklamaları için aşağıdaki bölümlere bakın. Özel kuralların örnekleri için bkz. [özel Web uygulaması güvenlik duvarı kuralları oluşturma ve kullanma](create-custom-waf-rules.md).

## <a name="custom-rule-fields"></a>Özel kural alanları

### <a name="name-optional"></a>Ad (isteğe bağlı)

Kuralın adıdır. Ad günlüklerde görüntülenir.

### <a name="priority-required"></a>Öncelik (gerekli)

- Öncelik, kuralların değerlendirildiği sırayı belirler. Değer ne kadar düşükse, kural değerlendirme daha erken yapılır. İzin verilen Aralık 1 ile 100 arasındadır.
- Öncelik, tüm özel kurallar arasında benzersiz olmalıdır. Önceliği 40 olan bir kural, 80 önceliğine sahip bir kuraldan önce değerlendirilir.

### <a name="rule-type-required"></a>Kural türü (gerekli)

Şu anda kural türü **Matchrule**olmalıdır.

### <a name="match-variable-required"></a>Match değişkeni (zorunlu)

Match değişkeni aşağıdakilerden biri olmalıdır:

- RemoteAddr: uzak bilgisayar bağlantısının IP adresi veya ana bilgisayar adı
- RequestMethod: HTTP istek yöntemi (GET, POST, PUT, DELETE, vb.).
- QueryString: URI içindeki değişken.
- PostArgs: POST gövdesinde gönderilen bağımsız değişkenler. Bu Match değişkenini kullanan özel kurallar yalnızca Content-Type üst bilgisi "application/x-www-form-urlencoded" ve "multipart/form-Data" olarak ayarlandığında uygulanır.
- RequestUri: isteğin URI 'SI.
- RequestHeaders: isteğin üst bilgileri.
- RequestBody: tüm istek gövdesini içeren değişken. Bu Match değişkenini kullanan özel kurallar yalnızca Content-Type üst bilgisi "application/x-www-form-urlencoded" olarak ayarlandığında uygulanır. 
- RequestCookies: isteğin tanımlama bilgileri.

### <a name="selector-optional"></a>Seçici (isteğe bağlı)

Seçici, matchVariable koleksiyonunun alanını açıklar. Örneğin, matchVariable "RequestHeaders" ise, seçici Kullanıcı Aracısı üstbilgisinde olabilir.

### <a name="operator-required"></a>İşleç (gerekli)

İşleci aşağıdakilerden biri olmalıdır:

- IPMatch: Bu işleç yalnızca Match değişkeni *Remoteaddr*olduğunda kullanılır.
- Eşittir: giriş, MatchValue ile aynıdır.
- Contains
- LessThan
- GreaterThan
- LessThanOrEqual
- GreaterThanOrEqual
- Ile başlıyor
- EndsWith
- Regex

### <a name="negate-condition-optional"></a>Negate koşulu (isteğe bağlı)

Geçerli koşulu geçersiz kılar.

### <a name="transform-optional"></a>Dönüştürme (isteğe bağlı)

Eşleşme denendiğinde önce tamamlanacak dönüştürmelerin adlarına sahip dizelerin listesi. Dönüşümler şunları içerir:

- Küçük harf
- Kırpma
- URL kod çözme
- UrlEncode 
- RemoveNulls
- HtmlEntityDecode

### <a name="match-values-required"></a>Değerleri Eşleştir (gerekli)

MatchValues alanı, *ya*da ' Ed ' olarak düşünülebilir. Örneğin, değerler IP adresleri veya diğer dizeler olabilir. Değer biçimi Previous işlecine bağlıdır.

### <a name="action-required"></a>Eylem (gerekli)

Eylem alanı aşağıdaki seçenekleri sunar: 

- İzin ver: işlemi yetkilendirir ve sonraki tüm kuralları atlar. Bu, belirtilen isteğin Izin verilenler listesine eklendiği ve eşleştirdikten sonra isteğin daha fazla değerlendirmeyi durdurduğu ve arka uç havuzuna gönderildiği anlamına gelir. Izin verilenler listesindeki kurallar, daha fazla özel kural veya yönetilen kurallar için değerlendirilmez.

- Engelle: *Secdefaultaction* (algılama/önleme modu) temelinde işlemi engeller. Izin verme eylemi gibi, istek değerlendirildikten ve blok listesine eklendikten sonra, değerlendirme durdurulur ve istek engellenir. Aynı koşulları karşılayan sonraki istekler değerlendirilmez. Yalnızca engellenirler. 

- GNLK: kuralın günlüğe yazmasına Izin verir ve kuralların geri kalanının değerlendirme için çalışmasına izin verir. Sonraki özel kurallar öncelik sırasına göre değerlendirilir ve ardından yönetilen kuralları izler.

## <a name="next-steps"></a>Sonraki adımlar

Özel kurallar hakkında bilgi edindiğinize göre, [kendi özel kurallarınızı oluşturabilirsiniz](create-custom-waf-rules.md).
