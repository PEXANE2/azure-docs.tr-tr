---
title: Azure Ön Kapı 'daki Web uygulaması güvenlik duvarı hariç tutma listeleri - Azure portalı
description: Bu makalede, Azure portalı ile Azure Ön'teki dışlama listeleri yapılandırması hakkında bilgi verilmektedir.
services: web-application-firewall
author: vhorne
ms.service: web-application-firewall
ms.date: 02/25/2020
ms.author: victorh
ms.topic: conceptual
ms.openlocfilehash: 6ed382e88700e4ecd7f8de20a2c8da7ed3c13566
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77925936"
---
# <a name="web-application-firewall-waf-with-front-door-service-exclusion-lists"></a>Ön Kapı Hizmeti dışlama listeleri ile Web Uygulama Güvenlik Duvarı (WAF) 

Bazen Web Uygulaması Güvenlik Duvarı (WAF), uygulamanız için izin vermek istediğiniz bir isteği engelleyebilir. Örneğin, Active Directory kimlik doğrulama için kullanılan belirteçleri ekler. Bu belirteçler, WAF kurallarından yanlış pozitif itetikleyebilecek özel karakterler içerebilir. WAF dışlama listeleri, waf değerlendirmesinden belirli istek özniteliklerini atlayasınız.  Bir dışlama listesi [PowserShell,](https://docs.microsoft.com/powershell/module/az.frontdoor/New-AzFrontDoorWafManagedRuleExclusionObject?view=azps-3.5.0) [Azure CLI,](https://docs.microsoft.com/cli/azure/ext/front-door/network/front-door/waf-policy/managed-rules/exclusion?view=azure-cli-latest#ext-front-door-az-network-front-door-waf-policy-managed-rules-exclusion-add) [Rest API](https://docs.microsoft.com/rest/api/frontdoorservice/webapplicationfirewall/policies/createorupdate)veya Azure portalı kullanılarak yapılandırılabilir. Aşağıdaki örnekte Azure portal yapılandırması gösterilmektedir. 
## <a name="configure-exclusion-lists-using-the-azure-portal"></a>Azure portalını kullanarak dışlama listelerini yapılandırma
**Yönetilen kurallar** uyarınca WAF portalından **dışlamaları yönetme**

![Dışlamayı](../media/waf-front-door-exclusion/exclusion1.png)
![yönetexclusion_add yönet](../media/waf-front-door-exclusion/exclusion2.png)

 Örnek bir dışlama ![listesi: exclusion_define yönet](../media/waf-front-door-exclusion/exclusion3.png)

Bu örnek, *kullanıcı* üstbilgi alanındaki değeri dışlar. Geçerli bir istek, SQL enjeksiyon kuralını tetikleyen bir dize içeren *kullanıcı* alanını içerebilir. WAF kuralının alandaki hiçbir şeyi değerlendirmemesi için bu durumda *kullanıcı* parametresini hariç tutabilirsiniz.

Dışlama listelerine ada göre aşağıdaki öznitelikler eklenebilir. Kullandığınız alanların değerleri WAF kurallarına göre değerlendirilmez, ancak adları değerlendirilir. Dışlama listeleri, alanın değerinin denetimini kaldırır.

* Üstbilgi adı isteme
* Çerez adı isteği
* Sorgu dize args adı
* İstek gövde sonrası args adı

Tam istek üstbilgisi, gövde, çerez veya sorgu dize özniteliği eşleşmesi belirtebilirsiniz.  Veya isteğe bağlı olarak kısmi eşleşmeleri belirtebilirsiniz. Aşağıdaki işleçler desteklenen maç ölçütleridir:

- **Equals**: Bu işleç tam bir eşleşme için kullanılır. Örneğin, **bearerToken**adlı bir üstbilgi seçmek için, **bearerToken**olarak seçici kümesi ile eşitler işleci kullanın.
- **Başlangıç**: Bu işleç, belirtilen seçici değeriyle başlayan tüm alanları eşler.
- **Biter**: Bu işleç belirtilen seçici değeri ile biten tüm istek alanları eşleşir.
- **Contains**: Bu işleç, belirtilen seçici değeri içeren tüm istek alanlarıyla eşleşir.
- **Eşittir any**: Bu işleç tüm istek alanlarıyla eşleşir. * seçici değeridir.

Üstbilgi ve çerez adları büyük/küçük harf duyarsızdır.

Dışlama listesini yönetilen kural kümesiiçindeki tüm kurallara, belirli bir kural grubu için kurallara veya önceki örnekte gösterildiği gibi tek bir kurala uygulayabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

WAF ayarlarınızı yapılandırdıktan sonra, WAF günlüklerinizi nasıl görüntülediğinizi öğrenin. Daha fazla bilgi için [Ön Kapı tanılama](../afds/waf-front-door-monitor.md)bakın.
