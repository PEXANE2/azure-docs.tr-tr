---
title: Sorun Giderme - Azure Web Uygulaması Güvenlik Duvarı
description: Bu makale, Azure Uygulama Ağ Geçidi için Web Uygulama Güvenlik Duvarı (WAF) için sorun giderme bilgileri sağlar
services: web-application-firewall
author: vhorne
ms.service: web-application-firewall
ms.date: 11/14/2019
ms.author: ant
ms.topic: conceptual
ms.openlocfilehash: 33c85752903edd618044ccbab06aff7df9a791da
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74046186"
---
# <a name="troubleshoot-web-application-firewall-waf-for-azure-application-gateway"></a>Azure Uygulama Ağ Geçidi için Sorun Giderme Web Uygulaması Güvenlik Duvarı (WAF)

Web Uygulama Güvenlik Duvarınızdan (WAF) geçmesi gereken istekler engellenirse yapabileceğiniz birkaç şey vardır.

İlk olarak, [WAF genel görünümünü](ag-overview.md) ve [WAF yapılandırma](application-gateway-waf-configuration.md) belgelerini okuduğunuzdan emin olun. Ayrıca, [WAF izleme](../../application-gateway/application-gateway-diagnostics.md) etkin olduğundan emin olun Bu makaleler waf nasıl çalıştığını açıklamak, WAF kural ayarlar nasıl çalıştığını ve nasıl WAF günlükleri erişmek için.

## <a name="understanding-waf-logs"></a>WAF günlüklerini anlama

WAF günlüklerinin amacı, WAF tarafından eşleşen veya engellenen her isteği göstermektir. Eşleşen veya engellenen tüm değerlendirilen isteklerin genel muhasebesidir. WAF'ın olmaması gereken bir isteği engellediğini fark ederseniz (yanlış pozitif), birkaç şey yapabilirsiniz. İlk olarak, daraltın ve belirli isteği bulun. İsteğin belirli URI, zaman damgası veya işlem kimliğini bulmak için günlüklere bakın. İlişkili günlük girişlerini bulduğunuzda, yanlış pozitif ler üzerinde hareket etmeye başlayabilirsiniz.

Örneğin, WAF'ınızdan geçmek istediğiniz *1=1* dizesini içeren yasal bir trafiğiniz olduğunu varsam. İsteğe çalışırsanız, WAF herhangi bir parametre veya alanda *1=1* dizenizi içeren trafiği engeller. Bu genellikle bir SQL enjeksiyon saldırısı ile ilişkili bir dize. Günlüklere bakabilir ve isteğin zaman damgasını ve engellenen/eşleşen kuralları görebilirsiniz.

Aşağıdaki örnekte, aynı istek sırasında (TransactionId alanını kullanarak) dört kuralın tetiklediğini görebilirsiniz. İlki, kullanıcının istek için sayısal/IP URL'si kullandığı için eşleşip eşleştildiği ve bunun da bir uyarı olduğu için anomali puanını üç artırdığı yazıyor. Eşleşen bir sonraki kural 942130, aradığınız olan. `details.data` *1=1'i* sahada görebilirsiniz. Bu da bir uyarı olduğu için anomali skorunu tekrar üçe kadar arttırır. Genellikle, eylem **Matched** olan her kural anomali puanı artar ve bu noktada anomali puanı altı olacaktır. Daha fazla bilgi için [Anomaly puanlama moduna](ag-overview.md#anomaly-scoring-mode)bakın.

Son iki günlük girişi, anomali skoru yeterince yüksek olduğu için isteğin engellendiğini gösteriyor. Bu girişlerin diğer ikisinden farklı bir eylemi var. İsteği gerçekten *engellediklerini* gösteriyorlar. Bu kurallar zorunludur ve devre dışı tutulamaz. Onlar kurallar olarak düşünülmemelidir, ama WAF iç çekirdek altyapı olarak daha fazla.

```json
{ 
    "resourceId": "/SUBSCRIPTIONS/A6F44B25-259E-4AF5-888A-386FED92C11B/RESOURCEGROUPS/DEMOWAF_V2/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/DEMOWAF-V2", 
    "operationName": "ApplicationGatewayFirewall", 
    "category": "ApplicationGatewayFirewallLog", 
    "properties": { 
        "instanceId": "appgw_3", 
        "clientIp": "167.220.2.139", 
        "clientPort": "", 
        "requestUri": "\/", 
        "ruleSetType": "OWASP_CRS", 
        "ruleSetVersion": "3.0.0", 
        "ruleId": "920350", 
        "message": "Host header is a numeric IP address", 
        "action": "Matched", 
        "site": "Global", 
        "details": { 
            "message": "Warning. Pattern match \\\"^[\\\\\\\\d.:]+$\\\" at REQUEST_HEADERS:Host. ", 
            "data": "40.90.218.160", 
            "file": "rules\/REQUEST-920-PROTOCOL-ENFORCEMENT.conf\\\"", 
            "line": "791" 
        }, 
        "hostname": "vm000003", 
        "transactionId": "AcAcAcAcAKH@AcAcAcAcAyAt" 
    } 
} 
{ 
    "resourceId": "/SUBSCRIPTIONS/A6F44B25-259E-4AF5-888A-386FED92C11B/RESOURCEGROUPS/DEMOWAF_V2/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/DEMOWAF-V2", 
    "operationName": "ApplicationGatewayFirewall", 
    "category": "ApplicationGatewayFirewallLog", 
    "properties": { 
        "instanceId": "appgw_3", 
        "clientIp": "167.220.2.139", 
        "clientPort": "", 
        "requestUri": "\/", 
        "ruleSetType": "OWASP_CRS", 
        "ruleSetVersion": "3.0.0", 
        "ruleId": "942130", 
        "message": "SQL Injection Attack: SQL Tautology Detected.", 
        "action": "Matched", 
        "site": "Global", 
        "details": { 
            "message": "Warning. Pattern match \\\"(?i:([\\\\\\\\s'\\\\\\\"`\\\\\\\\(\\\\\\\\)]*?)([\\\\\\\\d\\\\\\\\w]++)([\\\\\\\\s'\\\\\\\"`\\\\\\\\(\\\\\\\\)]*?)(?:(?:=|\\u003c=\\u003e|r?like|sounds\\\\\\\\s+like|regexp)([\\\\\\\\s'\\\\\\\"`\\\\\\\\(\\\\\\\\)]*?)\\\\\\\\2|(?:!=|\\u003c=|\\u003e=|\\u003c\\u003e|\\u003c|\\u003e|\\\\\\\\^|is\\\\\\\\s+not|not\\\\\\\\s+like|not\\\\\\\\s+regexp)([\\\\\\\\s'\\\\\\\"`\\\\\\\\(\\\\\\\\)]*?)(?!\\\\\\\\2)([\\\\\\\\d\\\\\\\\w]+)))\\\" at ARGS:text1. ", 
            "data": "Matched Data: 1=1 found within ARGS:text1: 1=1", 
            "file": "rules\/REQUEST-942-APPLICATION-ATTACK-SQLI.conf\\\"", 
            "line": "554" 
        }, 
        "hostname": "vm000003", 
        "transactionId": "AcAcAcAcAKH@AcAcAcAcAyAt" 
    } 
} 
{ 
    "resourceId": "/SUBSCRIPTIONS/A6F44B25-259E-4AF5-888A-386FED92C11B/RESOURCEGROUPS/DEMOWAF_V2/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/DEMOWAF-V2", 
    "operationName": "ApplicationGatewayFirewall", 
    "category": "ApplicationGatewayFirewallLog", 
    "properties": { 
        "instanceId": "appgw_3", 
        "clientIp": "167.220.2.139", 
        "clientPort": "", 
        "requestUri": "\/", 
        "ruleSetType": "", 
        "ruleSetVersion": "", 
        "ruleId": "0", 
        "message": "Mandatory rule. Cannot be disabled. Inbound Anomaly Score Exceeded (Total Score: 8)", 
        "action": "Blocked", 
        "site": "Global", 
        "details": { 
            "message": "Access denied with code 403 (phase 2). Operator GE matched 5 at TX:anomaly_score. ", 
            "data": "", 
            "file": "rules\/REQUEST-949-BLOCKING-EVALUATION.conf\\\"", 
            "line": "57" 
        }, 
        "hostname": "vm000003", 
        "transactionId": "AcAcAcAcAKH@AcAcAcAcAyAt" 
    } 
} 
{ 
    "resourceId": "/SUBSCRIPTIONS/A6F44B25-259E-4AF5-888A-386FED92C11B/RESOURCEGROUPS/DEMOWAF_V2/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/DEMOWAF-V2", 
    "operationName": "ApplicationGatewayFirewall", 
    "category": "ApplicationGatewayFirewallLog", 
    "properties": { 
        "instanceId": "appgw_3", 
        "clientIp": "167.220.2.139", 
        "clientPort": "", 
        "requestUri": "\/", 
        "ruleSetType": "", 
        "ruleSetVersion": "", 
        "ruleId": "0", 
        "message": "Mandatory rule. Cannot be disabled. Inbound Anomaly Score Exceeded (Total Inbound Score: 8 - SQLI=5,XSS=0,RFI=0,LFI=0,RCE=0,PHPI=0,HTTP=0,SESS=0): SQL Injection Attack: SQL Tautology Detected.", 
        "action": "Blocked", 
        "site": "Global", 
        "details": { 
            "message": "Warning. Operator GE matched 5 at TX:inbound_anomaly_score. ", 
            "data": "", 
            "file": "rules\/RESPONSE-980-CORRELATION.conf\\\"", 
            "line": "73" 
        }, 
        "hostname": "vm000003", 
        "transactionId": "AcAcAcAcAKH@AcAcAcAcAyAt" 
    }
}
```

## <a name="fixing-false-positives"></a>Yanlış pozitifleri düzeltme

Bu bilgiler ve 942130 kuralının *1=1* dizesiyle eşleşen bilgiyle, bunun trafiğinizi engellemesini engellemek için birkaç şey yapabilirsiniz:

- Dışlama Listesi Kullanma

   Dışlama listeleri hakkında daha fazla bilgi için [WAF yapılandırmasına](application-gateway-waf-configuration.md#waf-exclusion-lists) bakın.
- Kuralı devre dışı kılmış olur.

### <a name="using-an-exclusion-list"></a>Dışlama listesini kullanma

Yanlış pozitif uygulama konusunda bilinçli bir karar vermek için, uygulamanızın kullandığı teknolojileri tanımak önemlidir. Örneğin, teknoloji yığınınızda bir SQL sunucusu olmadığını ve bu kurallarla ilgili yanlış pozitif sonuçlar aldığınızı varsan. Bu kuralları devre dışı bırakmak güvenliğinizi zayıflatmaz.

Dışlama listesini kullanmanın bir yararı, isteğin yalnızca belirli bir bölümünün devre dışı bırakılmış olmasıdır. Ancak, bu, genel bir ayar olduğundan, WAF'ınızdan geçen tüm trafik için belirli bir dışlamanın geçerli olduğu anlamına gelir. Örneğin, *1=1* belirli bir uygulama için gövdede geçerli bir istekse, ancak diğerleri için geçerli değilse, bu bir soruna yol açabilir. Başka bir yararı, tüm istek hariç aksine, belirli bir durum yerine getirilirse hariç tutulacak gövde, üstbilgi ve çerezler arasında seçim yapabilirsiniz.

Bazen, belirli parametrelerin sezgisel olmayan bir şekilde WAF'ye aktarıldığı durumlar vardır. Örneğin, Azure Etkin Dizini'ni kullanarak kimlik doğrulaması yapılırken geçirilen bir belirteç vardır. Bu belirteç, *__RequestVerificationToken,* genellikle bir İstek Çerez olarak geçirilen olsun. Ancak, tanımlama bilgilerinin devre dışı bırakıldığı bazı durumlarda, bu belirteç de istek özniteliği veya "arg" olarak geçirilir. Bu durumda, *__RequestVerificationToken'nin* de **İstek özniteliği adı** olarak dışlama listesine eklenmediğinden emin olmanız gerekir.

![Dışlamalar](../media/web-application-firewall-troubleshoot/exclusion-list.png)

Bu örnekte, *metin1*eşittir **İstek özniteliği adını** dışlamak istiyorum. Güvenlik duvarı günlüklerinde öznitelik adını görebildiğiniz için bu belirgindir: **veri: Eşleşen Veriler: ARGS içinde bulunan 1=1:text1: 1=1**. Öznitelik **text1'dir.** Ayrıca bu öznitelik adını birkaç başka yol la bulabilirsiniz, [bkz.](#finding-request-attribute-names)

![WAF dışlama listeleri](../media/web-application-firewall-troubleshoot/waf-config.png)

### <a name="disabling-rules"></a>Kuralları devre dışı bırakma

Yanlış pozitif etrafında almak için başka bir yolu, WAF kötü niyetli olduğunu düşündüm giriş eşleşen kuralı devre dışı kalmaktır. WAF günlüklerini ayrıştırdığınız ve kuralı 942130'a indirgediğiniz için Azure portalında devre dışı kullanabilirsiniz. Azure [portalı üzerinden web uygulaması güvenlik duvarı kurallarını özelleştir'e](application-gateway-customize-waf-rules-portal.md)bakın.

Bir kuralı devre dışı bırakmanın bir yararı, normalde engellenecek belirli bir koşulu içeren tüm trafiği biliyorsanız, tüm WAF için bu kuralı devre dışı bırakabilirsiniz. Ancak, yalnızca belirli bir kullanım durumunda geçerli bir trafikse, genel bir ayar olduğu için bu kuralı tüm WAF için devre dışı bırakarak bir güvenlik açığı açarsınız.

Azure PowerShell'i kullanmak istiyorsanız, [PowerShell aracılığıyla web uygulaması güvenlik duvarı kurallarını özelleştir'e](application-gateway-customize-waf-rules-powershell.md)bakın. Azure CLI'yi kullanmak istiyorsanız, [Azure CLI aracılığıyla web uygulaması güvenlik duvarı kurallarını özelleştir'e](application-gateway-customize-waf-rules-cli.md)bakın.

![WAF kuralları](../media/web-application-firewall-troubleshoot/waf-rules.png)

## <a name="finding-request-attribute-names"></a>İstek öznitelik adlarını bulma

[Fiddler](https://www.telerik.com/fiddler)yardımıyla, tek tek istekleri incelemek ve bir web sayfasının belirli alanları denir belirlemek. Bu, Dışlama Listeleri'ni kullanarak belirli alanların denetimden çıkarılmasını hariç tutmanın yardımcı olabilir.

Bu örnekte, *1=1* dizesinin girildiği alana **text1**olarak adverilir.

![Fiddler](../media/web-application-firewall-troubleshoot/fiddler-1.png)

Bu, hariç tutabileceğiniz bir alandır. Dışlama listeleri hakkında daha fazla bilgi edinmek için [Bkz. Web uygulaması güvenlik duvarı isteği boyut sınırları ve dışlama listeleri.](application-gateway-waf-configuration.md#waf-exclusion-lists) Bu durumda değerlendirmeyi aşağıdaki dışlamayı yapılandırarak hariç tutabilirsiniz:

![WAF hariç tutma](../media/web-application-firewall-troubleshoot/waf-exclusion-02.png)

Ayrıca, dışlama listesine ne eklemeniz gerektiğini görmek için bilgileri almak için güvenlik duvarı günlüklerini de inceleyebilirsiniz. Günlüğe kaydetmeyi etkinleştirmek [için, Uygulama Ağ Geçidi için Arka uç durumu, tanılama günlükleri ve ölçümlere](../../application-gateway/application-gateway-diagnostics.md)bakın.

Güvenlik duvarı günlüğünü inceleyin ve incelemek istediğiniz isteğin gerçekleştiği saat için PT1H.json dosyasını görüntüleyin.

Bu örnekte, aynı İşlem Kimliği'ne sahip dört kuralınız olduğunu ve bunların hepsinin aynı anda oluştuğunu görebilirsiniz:

```json
-   {
-       "resourceId": "/SUBSCRIPTIONS/A6F44B25-259E-4AF5-888A-386FED92C11B/RESOURCEGROUPS/DEMOWAF_V2/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/DEMOWAF-V2",
-       "operationName": "ApplicationGatewayFirewall",
-       "category": "ApplicationGatewayFirewallLog",
-       "properties": {
-           "instanceId": "appgw_3",
-           "clientIp": "167.220.2.139",
-           "clientPort": "",
-           "requestUri": "\/",
-           "ruleSetType": "OWASP_CRS",
-           "ruleSetVersion": "3.0.0",
-           "ruleId": "920350",
-           "message": "Host header is a numeric IP address",
-           "action": "Matched",
-           "site": "Global",
-           "details": {
-               "message": "Warning. Pattern match \\\"^[\\\\\\\\d.:]+$\\\" at REQUEST_HEADERS:Host. ",
-               "data": "40.90.218.160",
-               "file": "rules\/REQUEST-920-PROTOCOL-ENFORCEMENT.conf\\\"",
-               "line": "791"
-           },
-           "hostname": "vm000003",
-           "transactionId": "AcAcAcAcAKH@AcAcAcAcAyAt"
-       }
-   }
-   {
-       "resourceId": "/SUBSCRIPTIONS/A6F44B25-259E-4AF5-888A-386FED92C11B/RESOURCEGROUPS/DEMOWAF_V2/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/DEMOWAF-V2",
-       "operationName": "ApplicationGatewayFirewall",
-       "category": "ApplicationGatewayFirewallLog",
-       "properties": {
-           "instanceId": "appgw_3",
-           "clientIp": "167.220.2.139",
-           "clientPort": "",
-           "requestUri": "\/",
-           "ruleSetType": "OWASP_CRS",
-           "ruleSetVersion": "3.0.0",
-           "ruleId": "942130",
-           "message": "SQL Injection Attack: SQL Tautology Detected.",
-           "action": "Matched",
-           "site": "Global",
-           "details": {
-               "message": "Warning. Pattern match \\\"(?i:([\\\\\\\\s'\\\\\\\"`\\\\\\\\(\\\\\\\\)]*?)([\\\\\\\\d\\\\\\\\w]++)([\\\\\\\\s'\\\\\\\"`\\\\\\\\(\\\\\\\\)]*?)(?:(?:=|\\u003c=\\u003e|r?like|sounds\\\\\\\\s+like|regexp)([\\\\\\\\s'\\\\\\\"`\\\\\\\\(\\\\\\\\)]*?)\\\\\\\\2|(?:!=|\\u003c=|\\u003e=|\\u003c\\u003e|\\u003c|\\u003e|\\\\\\\\^|is\\\\\\\\s+not|not\\\\\\\\s+like|not\\\\\\\\s+regexp)([\\\\\\\\s'\\\\\\\"`\\\\\\\\(\\\\\\\\)]*?)(?!\\\\\\\\2)([\\\\\\\\d\\\\\\\\w]+)))\\\" at ARGS:text1. ",
-               "data": "Matched Data: 1=1 found within ARGS:text1: 1=1",
-               "file": "rules\/REQUEST-942-APPLICATION-ATTACK-SQLI.conf\\\"",
-               "line": "554"
-           },
-           "hostname": "vm000003",
-           "transactionId": "AcAcAcAcAKH@AcAcAcAcAyAt"
-       }
-   }
-   {
-       "resourceId": "/SUBSCRIPTIONS/A6F44B25-259E-4AF5-888A-386FED92C11B/RESOURCEGROUPS/DEMOWAF_V2/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/DEMOWAF-V2",
-       "operationName": "ApplicationGatewayFirewall",
-       "category": "ApplicationGatewayFirewallLog",
-       "properties": {
-           "instanceId": "appgw_3",
-           "clientIp": "167.220.2.139",
-           "clientPort": "",
-           "requestUri": "\/",
-           "ruleSetType": "",
-           "ruleSetVersion": "",
-           "ruleId": "0",
-           "message": "Mandatory rule. Cannot be disabled. Inbound Anomaly Score Exceeded (Total Score: 8)",
-           "action": "Blocked",
-           "site": "Global",
-           "details": {
-               "message": "Access denied with code 403 (phase 2). Operator GE matched 5 at TX:anomaly_score. ",
-               "data": "",
-               "file": "rules\/REQUEST-949-BLOCKING-EVALUATION.conf\\\"",
-               "line": "57"
-           },
-           "hostname": "vm000003",
-           "transactionId": "AcAcAcAcAKH@AcAcAcAcAyAt"
-       }
-   }
-   {
-       "resourceId": "/SUBSCRIPTIONS/A6F44B25-259E-4AF5-888A-386FED92C11B/RESOURCEGROUPS/DEMOWAF_V2/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/DEMOWAF-V2",
-       "operationName": "ApplicationGatewayFirewall",
-       "category": "ApplicationGatewayFirewallLog",
-       "properties": {
-           "instanceId": "appgw_3",
-           "clientIp": "167.220.2.139",
-           "clientPort": "",
-           "requestUri": "\/",
-           "ruleSetType": "",
-           "ruleSetVersion": "",
-           "ruleId": "0",
-           "message": "Mandatory rule. Cannot be disabled. Inbound Anomaly Score Exceeded (Total Inbound Score: 8 - SQLI=5,XSS=0,RFI=0,LFI=0,RCE=0,PHPI=0,HTTP=0,SESS=0): SQL Injection Attack: SQL Tautology Detected.",
-           "action": "Blocked",
-           "site": "Global",
-           "details": {
-               "message": "Warning. Operator GE matched 5 at TX:inbound_anomaly_score. ",
-               "data": "",
-               "file": "rules\/RESPONSE-980-CORRELATION.conf\\\"",
-               "line": "73"
-           },
-           "hostname": "vm000003",
-           "transactionId": "AcAcAcAcAKH@AcAcAcAcAyAt"
-       }
-   }
```

CRS kuralının nasıl çalıştığı ve CRS kural kümesi 3.0'ın bir anormallik puanlama sistemiyle çalıştığını bildiğiniziçin [(Azure Uygulama Ağ Geçidi için Web Uygulama Güvenlik Duvarı'na](ag-overview.md)bakın) eylemle birlikte en alttaki iki kuralın **engellendiğini biliyorsunuz: Engellenen** özellik toplam anormallik puanına göre engellenir. Üzerinde durulması gereken kurallar ilk ikidir.

Kullanıcı, Uygulama Ağ Geçidi'ne gitmek için sayısal bir IP adresi kullandığından ilk giriş günlüğe kaydedilir ve bu durumda göz ardı edilebilir.

İkincisi (kural 942130) ilginç biridir. Ayrıntılarda bir desenle eşleşip (1=1) ve alanın **text1**olarak adlandırıldığını görebilirsiniz. **1=1'e** **eşit** **İstek Özniteliği Adı'nı** hariç tutmak için önceki adımları izleyin.

## <a name="finding-request-header-names"></a>İstek üstbilgi adlarını bulma

Fiddler bir kez daha istek üstbilgi adlarını bulmak için yararlı bir araçtır. Aşağıdaki ekran görüntüsünde, *İçerik Türü,* *Kullanıcı Aracısı*ve benzeri bu GET isteğinin üstbilgilerini görebilirsiniz.

![Fiddler](../media/web-application-firewall-troubleshoot/fiddler-2.png)

İstek ve yanıt üsteerlerini görüntülemenin başka bir yolu da Chrome'un geliştirici araçlarının içine bakmaktır. F12 tuşuna veya ->**Denetim Geliştirici Araçlarını** **Tıklatabilir** -> ve **Ağ** sekmesini seçebilirsiniz.

![Krom F12](../media/web-application-firewall-troubleshoot/chrome-f12.png)

## <a name="finding-request-cookie-names"></a>İstek çerez adlarını bulma

İstek çerezleri içeriyorsa, Bunları Fiddler'da görüntülemek için **Çerezler** sekmesi seçilebilir.

## <a name="restrict-global-parameters-to-eliminate-false-positives"></a>Yanlış pozitifleri ortadan kaldırmak için küresel parametreleri kısıtlama

- İstek vücut denetimini devre dışı

   İstek **gövdesini** kapalı olarak inceleyin ayarlayarak, tüm trafiğin istek gövdeleri WAF'ınız tarafından değerlendirilmeyecektir. İstek gövdeleri uygulamanız için kötü niyetli olmadığını biliyorsanız, bu yararlı olabilir.

   Bu seçeneği devre dışı bırakarak, yalnızca istek gövdesi denetlenmez. Üstbilgi ve tanımlama bilgileri, dışlama listesi işlevini kullanarak hariç tutulmadığı sürece denetlenmeye devam eder.

- Dosya boyutu sınırları

   WAF'ınız için dosya boyutunu sınırlandırarak, web sunucularınıza bir saldırı olasılığını sınırlandırıyorsunuz. Büyük dosyaların yüklenmesine izin vererek, arka ponponunuzu bunalmış olma riski artar. Dosya boyutunu uygulamanız için normal bir kullanım örneğiyle sınırlamak, saldırıları önlemenin başka bir yoludur.

   > [!NOTE]
   > Uygulamanızın belirli bir boyutun üzerinde herhangi bir dosya yüklemesi gerekmeyeceğini biliyorsanız, bir sınır belirleyerek bunu kısıtlayabilirsiniz.

## <a name="firewall-metrics-waf_v1-only"></a>Güvenlik Duvarı Ölçümleri (yalnızca WAF_v1)

v1 Web Uygulama Güvenlik Duvarları için aşağıdaki ölçümler artık portalda kullanılabilir: 

1. Web Uygulaması Güvenlik Duvarı Engellenen İstek Sayısı Engellenen istek sayısı engellendi
2. Web Uygulaması Güvenlik Duvarı Engellendi Kural Sayısı Eşleşen tüm kurallar **ve** istek engellendi
3. Web Uygulama Güvenlik Duvarı Toplam Kural Dağılımı Değerlendirme sırasında eşleşen tüm kurallar
     
Ölçümleri etkinleştirmek için portaldaki **Ölçümler** sekmesini seçin ve üç ölçümden birini seçin.

## <a name="next-steps"></a>Sonraki adımlar

[Bkz. Uygulama Ağ Geçidi'nde web uygulaması güvenlik duvarı nasıl yapılandırılabilirsiniz.](tutorial-restrict-web-traffic-powershell.md)
