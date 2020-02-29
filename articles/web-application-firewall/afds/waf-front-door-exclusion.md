---
title: Azure ön kapıdaki Web uygulaması güvenlik duvarı dışlama listeleri-Azure portal
description: Bu makalede, Azure portal ile Azure 'da dışlama listeleri yapılandırması hakkında bilgi sağlanır.
services: web-application-firewall
author: vhorne
ms.service: web-application-firewall
ms.date: 02/25/2020
ms.author: victorh
ms.topic: conceptual
ms.openlocfilehash: 6ed382e88700e4ecd7f8de20a2c8da7ed3c13566
ms.sourcegitcommit: 3c925b84b5144f3be0a9cd3256d0886df9fa9dc0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2020
ms.locfileid: "77925936"
---
# <a name="web-application-firewall-waf-with-front-door-service-exclusion-lists"></a>Ön kapı hizmeti dışlama listeleriyle Web uygulaması güvenlik duvarı (WAF) 

Bazen Web uygulaması güvenlik duvarı (WAF), uygulamanız için izin vermek istediğiniz bir isteği engelleyebilirler. Örneğin Active Directory, kimlik doğrulaması için kullanılan belirteçleri ekler. Bu belirteçler, WAF kurallarından yanlış pozitif bir pozitif tetikleyebildiği özel karakterler içerebilir. WAF dışlama listeleri, bir WAF değerlendirmesinden belirli istek özniteliklerini atlamanızı sağlar.  Bir dışlama listesi [Psershell](https://docs.microsoft.com/powershell/module/az.frontdoor/New-AzFrontDoorWafManagedRuleExclusionObject?view=azps-3.5.0), [Azure CLı](https://docs.microsoft.com/cli/azure/ext/front-door/network/front-door/waf-policy/managed-rules/exclusion?view=azure-cli-latest#ext-front-door-az-network-front-door-waf-policy-managed-rules-exclusion-add), [REST API](https://docs.microsoft.com/rest/api/frontdoorservice/webapplicationfirewall/policies/createorupdate)veya Azure Portal kullanılarak yapılandırılabilir. Aşağıdaki örnek Azure portal yapılandırmasını gösterir. 
## <a name="configure-exclusion-lists-using-the-azure-portal"></a>Dışlama listelerini Azure portal kullanarak yapılandırma
Özel **durumları yönetme** işlemi, WAF portalından **yönetilen kurallar** altında erişilebilir

![dışlama](../media/waf-front-door-exclusion/exclusion1.png)
yönetme ![exclusion_add yönetme](../media/waf-front-door-exclusion/exclusion2.png)

 Örnek bir dışlama listesi: ![Yönet exclusion_define](../media/waf-front-door-exclusion/exclusion3.png)

Bu örnek, *Kullanıcı* üstbilgisi alanındaki değeri dışlar. Geçerli bir istek, bir SQL ekleme kuralını tetikleyen bir dize içeren *Kullanıcı* alanı içerebilir. Bu durumda, WAF kuralının alanındaki herhangi bir şeyi değerlendirmemesi için *Kullanıcı* parametresini dışarıda bırakabilirsiniz.

Aşağıdaki öznitelikler ada göre dışlama listelerine eklenebilir. Kullandığınız alanların değerleri WAF kurallarına göre değerlendirilmez, ancak adları değerlendirilir. Dışlama Listeleri alanın değerinin incelemesini kaldırır.

* İstek üst bilgisi adı
* İstek tanımlama bilgisi adı
* Sorgu dizesi bağımsız değişkenleri adı
* İstek gövdesi Post bağımsız değişkenleri adı

Tam bir istek üst bilgisi, gövde, tanımlama bilgisi veya sorgu dizesi özniteliği eşleşmesi belirtebilirsiniz.  Ya da isteğe bağlı olarak kısmi eşleşmeler belirtebilirsiniz. Aşağıdaki işleçler desteklenen eşleşme ölçütleridir:

- **Eşittir**: Bu işleç tam eşleşme için kullanılır. Örneğin, **yataya**adı adlı bir üst bilgi seçmek için, Selector 'un **yataya**kümesi olarak ayarlandığı Equals işlecini kullanın.
- **Ile başlar**: Bu işleç, belirtilen Seçici değeriyle başlayan tüm alanlarla eşleşir.
- **Şununla biter**: Bu işleç, belirtilen Seçici değeriyle biten tüm istek alanlarıyla eşleşir.
- **Contains**: Bu işleç, belirtilen Seçici değerini içeren tüm istek alanlarıyla eşleşir.
- **Eşittir any**: Bu işleç tüm istek alanlarıyla eşleşir. * Seçici değeridir.

Üst bilgi ve tanımlama bilgisi adları büyük/küçük harfe duyarlıdır.

Dışlama listesini, yönetilen kural kümesi içindeki tüm kurallara, belirli bir kural grubu için kurallara veya önceki örnekte gösterildiği gibi tek bir kurala uygulayabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

WAF ayarlarınızı yapılandırdıktan sonra, WAF günlüklerinizi görüntülemeyi öğrenin. Daha fazla bilgi için bkz. [ön kapı tanılama](../afds/waf-front-door-monitor.md).
