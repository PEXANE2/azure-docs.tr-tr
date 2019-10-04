---
title: Azure Web uygulaması güvenlik duvarı (WAF) v2 özel kuralları
description: Bu makalede, Azure Application Gateway 'de Web uygulaması güvenlik duvarı (WAF) v2 özel kurallarına genel bir bakış sunulmaktadır.
services: application-gateway
ms.topic: article
author: vhorne
ms.service: application-gateway
ms.date: 6/18/2019
ms.author: victorh
ms.openlocfilehash: 154317e558c2c9a22f569f569684cced467900d5
ms.sourcegitcommit: f2d9d5133ec616857fb5adfb223df01ff0c96d0a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2019
ms.locfileid: "71937473"
---
# <a name="custom-rules-for-web-application-firewall-v2"></a>Web uygulaması güvenlik duvarı v2 için özel kurallar

Azure Application Gateway Web uygulaması güvenlik duvarı (WAF) v2, birçok farklı saldırı türünden koruma sağlayan, önceden yapılandırılmış, platform tarafından yönetilen bir kural kümesiyle birlikte gelir. Bu saldırılar siteler arası betik oluşturma, SQL ekleme ve diğer kişiler içerir. Bir WAF yöneticisiyseniz, çekirdek kural kümesi (sp_configure) kurallarını artırmak için size kendi kuralları yazmak isteyebilirsiniz. Kurallarınız, eşleşen ölçütlere göre istenen trafiği engelleyebilir veya izin verebilir.

Özel kurallar, WAF üzerinden geçen her istek için değerlendirilen kendi kurallarınızı oluşturmanızı sağlar. Bu kurallar, yönetilen kural kümelerindeki kuralların geri kalanından daha yüksek bir öncelik tutar. Özel kurallar bir kural adı, kural önceliği ve eşleşen koşulların bir dizisini içerir. Bu koşullar karşılanıyorsa, bir eylem yapılır (izin vermek veya engellemek için).

Örneğin, 192.168.5.4/24 aralığındaki bir IP adresinden gelen tüm istekleri engelleyebilirsiniz. Bu kuralda, işleç *Ipmatch*Ise, MATCHVALUES değeri IP adres aralığıdır (192.168.5.4/24) ve işlem trafiği engeller. Kuralın adını ve önceliğini de ayarlarsınız.

Özel kurallar, güvenlik gereksinimlerinizi karşılayan daha gelişmiş kurallar oluşturmak için bileşik mantık kullanımını destekler. Örneğin, (koşul 1 **ve** koşul 2) **veya** koşul 3).  Bu örnekte, koşul 1 **ve** koşul 2 karşılanırsa **veya** koşul 3 karşılanırsa, WAF 'nin özel kuralda belirtilen eylemi yapması gerekir.

Aynı kural içindeki farklı eşleşen koşullar, **ve**kullanarak her zaman bileşik bir şekilde yapılır. Örneğin, belirli bir IP adresinden gelen trafiği ve yalnızca belirli bir tarayıcıyı kullanıyorsa engelleyin.

**Ya** da iki farklı koşul istiyorsanız, iki koşulun farklı kurallarda olması gerekir. Örneğin, belirli bir IP adresinden gelen trafiği engelleyin veya belirli bir tarayıcı kullanılıyorsa trafiği engelleyin.

> [!NOTE]
> WAF özel kural sayısı üst sınırı 100 ' dir. Application Gateway limitleri hakkında daha fazla bilgi için bkz. [Azure aboneliği ve hizmet limitleri, Kotalar ve kısıtlamalar](../azure-subscription-service-limits.md#application-gateway-limits).

Normal ifadeler Ayrıca, benzer RuleSets gibi özel kurallarda de desteklenir. Bunlara örnekler için bkz. örnek 3 ve 5 [özel Web uygulaması güvenlik duvarı kuralları oluşturma ve kullanma](create-custom-waf-rules.md).

## <a name="allowing-vs-blocking"></a>Vs. engellemeye izin verme

Özel kurallar sayesinde trafiğin izin verilmesi ve engellenmesi basittir. Örneğin, bir IP adresi aralığından gelen tüm trafiği engelleyebilirsiniz. İstek belirli bir tarayıcıdan geliyorsa, trafiğe izin vermek için başka bir kural yapabilirsiniz.

Bir şeye izin vermek için `-Action` parametresinin **Izin ver**olarak ayarlandığından emin olun. Bir şeyi engellemek için `-Action` parametresinin **Engelle**olarak ayarlandığından emin olun.

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

Önceki `$BlockRule` Azure Resource Manager aşağıdaki özel kurala eşlenir:

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

Bu özel kural bir ad, öncelik, bir eylem ve eylemin gerçekleşmesi için karşılanması gereken eşleşen koşulların dizisini içerir. Bu alanların daha ayrıntılı açıklaması için aşağıdaki alan açıklamalarına bakın. Örneğin özel kurallar için bkz. [özel Web uygulaması güvenlik duvarı kuralları oluşturma ve kullanma](create-custom-waf-rules.md).

## <a name="fields-for-custom-rules"></a>Özel kurallar için alanlar

### <a name="name-optional"></a>Ad [isteğe bağlı]

Kuralın adıdır. Bu ad günlüklerde görüntülenir.

### <a name="priority-required"></a>Öncelik [gerekli]

- Kuralların değerlendirildiği sırayı belirler. Değer ne kadar düşükse, kural değerlendirme daha erken yapılır. İzin verilen Aralık 1-100 ' dir. 
- Tüm özel kurallar arasında benzersiz olmalıdır. Öncelik 40 olan bir kural, öncelik 80 olan bir kuraldan önce değerlendirilir.

### <a name="rule-type-required"></a>Kural türü [gerekli]

Şu anda **Matchrule**olmalıdır.

### <a name="match-variable-required"></a>Match değişkeni [gerekli]

Değişkenlerden biri olmalıdır:

- RemoteAddr – uzak bilgisayar bağlantısının IP adresi/ana bilgisayar adı
- RequestMethod – HTTP Istek yöntemi (GET, POST, PUT, DELETE vb.)
- QueryString – URI 'de değişken
- PostArgs: POST gövdesinde gönderilen bağımsız değişkenler. Bu Match değişkenini kullanan özel kurallar yalnızca ' Content-Type ' üst bilgisi ' Application/x-www-form-urlencoded ' ve ' multipart/form-Data ' olarak ayarlandıysa geçerlidir.
- RequestUri – isteğin URI 'SI
- RequestHeaders – isteğin üst bilgileri
- RequestBody: Bu, tüm istek gövdesini bir bütün olarak içerir. Bu Match değişkenini kullanan özel kurallar yalnızca ' Content-Type ' üst bilgisi ' Application/x-www-form-urlencoded ' olarak ayarlandığında uygulanır. 
- RequestCookies – isteğin tanımlama bilgileri

### <a name="selector-optional"></a>Seçici [isteğe bağlı]

MatchVariable koleksiyonunun alanını açıklar. Örneğin, matchVariable, RequestHeaders ise, seçici *Kullanıcı Aracısı* üstbilgisinde olabilir.

### <a name="operator-required"></a>İşleç [gerekli]

Aşağıdaki işleçlerden biri olmalıdır:

- Ipmatch-yalnızca Match değişkeni *Remoteaddr* olduğunda kullanılır
- Eşittir – giriş, MatchValue ile aynıdır
- İçerir
- LessThan
- GreaterThan
- Yetersiz Otalep al
- GreaterThanOrEqual
- Ile başlıyor
- EndsWith
- Düzenli ifadesi

### <a name="negate-condition-optional"></a>Negate koşulu [isteğe bağlı]

Geçerli koşulu geçersiz kılar.

### <a name="transform-optional"></a>Dönüştürme [isteğe bağlı]

Eşleşmesinden önce yapılacak dönüşümlerdeki adlara sahip dizelerin listesi. Bunlar aşağıdaki dönüşümler olabilir:

- Küçük
- kırpma
- URL kod çözme
- UrlEncode 
- RemoveNulls
- HtmlEntityDecode

### <a name="match-values-required"></a>Değerleri Eşleştir [gerekli]

Eşleştirilecek değerler listesi, *veya*' Ed olarak düşünülebilir. Örneğin, bu IP adresleri veya diğer dizeler olabilir. Değer biçimi Previous işlecine bağlıdır.

### <a name="action-required"></a>Eylem [gerekli]

- Allow: işlemi, sonraki tüm kuralları atlayarak bir şekilde yetkilendirir. Bu, belirtilen isteğin izin verilenler listesine eklendiği ve eşleştirildiği anlamına gelir, istek daha fazla değerlendirmeyi durduruyor ve arka uç havuzuna gönderilir. İzin verilenler listesindeki kurallar, başka özel kurallar veya yönetilen kurallar için değerlendirilmez.
- Block: *Secdefaultaction* (algılama/önleme modu) temelinde işlemi engeller. Izin verme eyleminde olduğu gibi, istek değerlendirildikten ve blok listesine eklendikten sonra, değerlendirme durdurulur ve istek engellenir. Aynı koşulları karşıladıktan sonra herhangi bir istek değerlendirilmeyecektir ve yalnızca engellenir. 
- GNLK – kuralın günlüğe yazmasına Izin verir, ancak kuralların geri kalanının değerlendirme için çalışmasına izin verir. Sonraki özel kurallar öncelik sırasına göre değerlendirilir ve ardından yönetilen kuralları izler.

## <a name="next-steps"></a>Sonraki adımlar

Özel kurallar hakkında bilgi aldıktan sonra [kendi özel kurallarınızı oluşturun](create-custom-waf-rules.md).
