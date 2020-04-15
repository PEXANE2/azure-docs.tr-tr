---
title: Uygulama Ağ Geçidi'nde Azure Web Uygulama Güvenlik Duvarı (WAF) v2 özel kuralları
description: Bu makalede, Azure Uygulama Ağ Geçidi'ndeki Web Uygulaması Güvenlik Duvarı (WAF) v2 özel kurallarına genel bir bakış sağlanmaktadır.
services: web-application-firewall
ms.topic: article
author: vhorne
ms.service: web-application-firewall
ms.date: 04/14/2020
ms.author: victorh
ms.openlocfilehash: c0f802f5113e38e811c110ee913099e76fa7be0b
ms.sourcegitcommit: ea006cd8e62888271b2601d5ed4ec78fb40e8427
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81383823"
---
# <a name="custom-rules-for-web-application-firewall-v2-on-azure-application-gateway"></a>Azure Uygulama Ağ Geçidi'nde Web Uygulaması Güvenlik Duvarı v2 için özel kurallar

Azure Uygulama Ağ Geçidi Web Uygulaması Güvenlik Duvarı (WAF) v2, birçok farklı saldırı türüne karşı koruma sağlayan önceden yapılandırılmış, platform tarafından yönetilen bir kural kümesiyle birlikte gelir. Bu saldırılar, site ler arası komut dosyası ekleme, SQL enjeksiyonu ve diğerleri içerir. WAF yöneticisiyseniz, temel kural kümesi (CRS) kurallarını artırmak için kendi kurallarınızı yazmak isteyebilirsiniz. Kurallarınız, eşleşen ölçütlere bağlı olarak istenen trafiği engelleyebilir veya buna izin verebilir.

Özel kurallar, WAF'den geçen her istek için değerlendirilen kendi kurallarınızı oluşturmanıza olanak sağlar. Bu kurallar, yönetilen kural kümelerinde yer alan diğer kurallardan daha yüksek bir önceliğe sahip. Özel kurallar bir kural adı, kural önceliği ve eşleşen koşullar dizisi içerir. Bu koşullar yerine getirilirse, bir eylem yapılır (izin vermek veya engellemek için).

Örneğin, 192.168.5.4/24 aralığındaki bir IP adresinden gelen tüm istekleri engelleyebilirsiniz. Bu kuralda, işleç *IPMatch,* matchValues IP adres aralığı (192.168.5.4/24) ve eylem trafiği engellemek için. Ayrıca kuralın adını ve önceliğini de belirlersiniz.

Özel kurallar, güvenlik gereksinimlerinizi karşılayan daha gelişmiş kurallar yapmak için bileşik mantığı nı destekler. Örneğin, (Durum 1 **ve** Koşul 2) **veya** Durum 3). Bu, Koşul 1 **ve** Koşul 2 karşılanırsa **veya** Koşul 3 karşılanırsa, WAF'ın özel kuralda belirtilen eylemi yapması gerektiği anlamına gelir.

Aynı kural içinde farklı eşleşen koşullar her zaman kullanılarak bileşik **ve**. Örneğin, belirli bir IP adresinden ve yalnızca belirli bir tarayıcı kullanıyorlarsa trafiği engelleyin.

İsterseni **veya** iki farklı koşul istiyorsanız, iki koşul farklı kurallarda olmalıdır. Örneğin, belirli bir IP adresinden gelen trafiği engelleyin veya belirli bir tarayıcı kullanıyorsanız trafiği engelleyin.

> [!NOTE]
> WAF özel kurallarının maksimum sayısı 100'dür. Uygulama Ağ Geçidi sınırları hakkında daha fazla bilgi için [Azure abonelik ve hizmet sınırları, kotalar ve kısıtlamalar](../../azure-resource-manager/management/azure-subscription-service-limits.md#application-gateway-limits)abakın.

Düzenli ifadeler, CRS kural kümelerinde olduğu gibi özel kurallarda da desteklenir. Örneğin, Örnek 3 ve 5'e [bakın, Özel web uygulaması güvenlik duvarı kurallarını oluştur ve kullan.](create-custom-waf-rules.md)

## <a name="allowing-vs-blocking"></a>Engelleme ye izin verme

Özel kurallarla trafiğe izin vermek ve engellemek kolaydır. Örneğin, çeşitli IP adreslerinden gelen tüm trafiği engelleyebilirsiniz. İstek belirli bir tarayıcıdan geliyorsa, trafiğe izin vermek için başka bir kural daha yapabilirsiniz.

Bir şeye izin vermek `-Action` için, parametrenin **İzin Ver olarak**ayarlandığından emin olun. Bir şeyi engellemek için, parametrenin `-Action` **Engelle'ye**ayarlandığından emin olun.

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

Azure `$BlockRule` Kaynak Yöneticisi'nde aşağıdaki özel kuralın önceki eşler:

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

Bu özel kural, bir ad, öncelik, bir eylem ve eylemin gerçekleşmesi için karşılanması gereken eşleşen koşullar dizisini içerir. Bu alanların daha ayrıntılı açıklaması için aşağıdaki alan açıklamalarına bakın. Örneğin özel kurallar, [bkz.](create-custom-waf-rules.md)

## <a name="fields-for-custom-rules"></a>Özel kurallar için alanlar

### <a name="name-optional"></a>Ad [isteğe bağlı]

Kuralın adı.  Günlüklerde görünüyor.

### <a name="priority-required"></a>Öncelik [gerekli]

- Kural değerleme sırasını belirler. Değer ne kadar düşükse, kuralın daha erken değerlendirilmesi o kadar erken. İzin verilebilen aralık 1-100 arasındadır. 
- Tüm özel kurallar arasında benzersiz olmalıdır. Öncelik 40 olan bir kural öncelik 80 ile bir kural önce değerlendirilir.

### <a name="rule-type-required"></a>Kural türü [gerekli]

Şu anda, **MatchRule**olmalıdır.

### <a name="match-variable-required"></a>Eşleştirme değişkeni [gerekli]

Değişkenlerden biri olmalıdır:

- RemoteAddr – uzak bilgisayar bağlantısının IP Adresi/hostname
- RequestMethod – HTTP İstek yöntemi (GET, POST, PUT, DELETE, vb.)
- QueryString – URI'de Değişken
- PostArgs – Post gövdesinde gönderilen argümanlar. Bu eşanlamlı değişkenini kullanan Özel Kurallar yalnızca 'İçerik Türü' üstbilgisi 'application/x-www-form-urlencoded' ve 'multipart/form-data' olarak ayarlanmışsa uygulanır.
- RequestUri – Uri talebi
- RequestHeaders - İsteğin Başlıkları
- RequestBody – Bu bir bütün olarak tüm istek gövdesi içerir. Bu eşanlamlı değişkenini kullanan özel kurallar yalnızca 'İçerik Türü' üstbilgi 'application/x-www-form-urlencoded' olarak ayarlanmışsa uygulanır. 
- İstek Çerezleri – İsteğin Çerezleri

### <a name="selector-optional"></a>Seçici [isteğe bağlı]

MatchVariable koleksiyonunun alanını açıklar. Örneğin, matchVariable RequestHeaders ise, seçici *Kullanıcı-Aracı* üstbilgide olabilir.

### <a name="operator-required"></a>Operatör [gerekli]

Aşağıdaki işleçlerden biri olmalıdır:

- IPMatch - yalnızca Match Variable *RemoteAddr* olduğunda kullanılır
- Eşit – giriş MatchValue ile aynıdır
- Contains
- LessThan
- GreaterThan
- LessThanOrEqual
- GreaterThanOrEqual
- BeginsWith
- EndsWith
- Regex
- Geomatch (önizleme)

### <a name="negate-condition-optional"></a>Negate durumu [isteğe bağlı]

Geçerli durumu inkar eder.

### <a name="transform-optional"></a>[isteğe bağlı] dönüştürme

Eşleşmeden önce yapılacak dönüşümadlarının bulunduğu dizelerin listesi denenir. Aşağıdaki dönüşümler olabilir:

- Küçük harf
- Trim
- UrlDecode
- UrlEncode 
- RemoveNulls
- HtmlEntityDecode

### <a name="match-values-required"></a>Eşleşin değerleri [gerekli]

*OR*'ed olarak düşünülebilir karşı maç değerleri listesi. Örneğin, IP adresleri veya diğer dizeleri olabilir. Değer biçimi önceki işleç bağlıdır.

### <a name="action-required"></a>Eylem [gerekli]

- İzin Ver – Diğer tüm kuralları atlayarak işlemi onaylar. Belirtilen istek izin listesine eklenir ve eşleştikten sonra, istek daha fazla değerlendirmeyi durdurur ve arka uç havuzuna gönderilir. İzin listesinde yer alan kurallar başka özel kurallar veya yönetilen kurallar için değerlendirilmez.
- Blok – *SecDefaultAction* (algılama/önleme modu) dayalı hareketi engeller. Eyleme İzin Ver gibi, istek değerlendirildikten ve blok listesine eklendikten sonra değerlendirme durdurulur ve istek engellenir. Bundan sonraki herhangi bir istek aynı koşulları karşılar değerlendirmez ve sadece engellenir. 
- Günlük – Kuralın günlüğe yazılmasına izin verir, ancak kuralların geri kalanının değerlendirme için çalışmasını sağlar. Diğer özel kurallar öncelik sırasına göre değerlendirilir ve yönetilen kurallar izlenir.

## <a name="geomatch-custom-rules-preview"></a>Geomatch özel kuralları (önizleme)

Özel kurallar, uygulamalarınızın ve güvenlik ilkelerinizin tam gereksinimlerine uygun özel kurallar oluşturmanıza izin sağlar. Web uygulamalarınız için erişimi ülkeye/bölgeye göre kısıtlayabilirsiniz. Daha fazla bilgi için [Geomatch özel kurallarına (önizleme)](geomatch-custom-rules.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Özel kuralları öğrendikten [sonra, kendi özel kurallarınızı oluşturun.](create-custom-waf-rules.md)
