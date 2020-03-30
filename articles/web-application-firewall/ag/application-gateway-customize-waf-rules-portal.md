---
title: Portalı kullanarak kuralları özelleştir - Azure Web Uygulaması Güvenlik Duvarı
description: Bu makalede, Azure portalı ile Uygulama Ağ Geçidi'nde Web Uygulaması Güvenlik Duvarı kurallarının nasıl özelleştirilene ilişkin bilgiler verilmektedir.
services: web-application-firewall
author: vhorne
ms.service: web-application-firewall
ms.date: 11/14/2019
ms.author: victorh
ms.topic: article
ms.openlocfilehash: c4635333614ee1c0fd0322c29a659380fb4315c9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74048370"
---
# <a name="customize-web-application-firewall-rules-using-the-azure-portal"></a>Azure portalını kullanarak Web Uygulaması Güvenlik Duvarı kurallarını özelleştirme

Azure Uygulama Ağ Geçidi Web Uygulaması Güvenlik Duvarı (WAF), web uygulamaları için koruma sağlar. Bu korumalar Açık Web Uygulama Güvenlik Projesi (OWASP) Temel Kural Kümesi (CRS) tarafından sağlanır. Bazı kurallar yanlış pozitif neden olabilir ve gerçek trafiği engellemek. Bu nedenle, Uygulama Ağ Geçidi kural gruplarını ve kuralları özelleştirme olanağı sağlar. Belirli kural grupları ve kuralları hakkında daha fazla bilgi için [bkz.](application-gateway-crs-rulegroups-rules.md)

>[!NOTE]
> Uygulama ağ geçidiniz WAF katmanını kullanmıyorsa, başvuru ağ geçidini WAF katmanına yükseltme seçeneği doğru bölmede görünür. 

![WAF'ı etkinleştir][fig1]

## <a name="view-rule-groups-and-rules"></a>Kural gruplarını ve kuralları görüntüleme

**Kural gruplarını ve kuralları görüntülemek için**
1. Uygulama ağ geçidine göz atın ve ardından **Web uygulaması güvenlik duvarını**seçin.  
2. **WAF İlkenizi**seçin.
2. **Yönetilen Kuralları**seçin.

   Bu görünüm, seçilen kural kümesiyle sağlanan tüm kural gruplarının sayfasında bir tablo gösterir. Kuralın tüm onay kutuları seçilir.

## <a name="disable-rule-groups-and-rules"></a>Kural gruplarını ve kurallarını devre dışı

> [!IMPORTANT]
> Kural gruplarını veya kuralları devre dışı bırakırken dikkatli olun. Bu, sizi artan güvenlik risklerine maruz bırakabilir.

Kuralları devre dışı bırakırken, bir veya daha fazla kural grubu altında tüm kural grubunu veya belirli kuralları devre dışı bırakabilirsiniz. 

**Kural gruplarını veya belirli kuralları devre dışı kalmak için**

   1. Devre dışı kalmak istediğiniz kuralları veya kural gruplarını arayın.
   2. Devre dışı kalmak istediğiniz kurallar için onay kutularını seçin. 
   3. Seçili kurallar için sayfanın üst kısmındaki eylemi (etkinleştirme/devre dışı bırakma) seçeneğini belirleyin.
   2. **Kaydet'i**seçin. 

![Değişiklikleri kaydetme][3]

## <a name="mandatory-rules"></a>Zorunlu kurallar

Aşağıdaki liste, WAF'ın Önleme Modundayken isteği engellemesine neden olan koşulları içerir. Algılama Modunda, özel durum olarak kaydedilirler.

Bunlar yapılandırılamaz veya devre dışı tutulamaz:

* Vücut denetimi kapalı olmadığı sürece istek gövdesinin ayrıştırılmaması, isteğin engellenmesine neden olur (XML, JSON, form verileri)
* İstek gövdesi (dosya olmadan) veri uzunluğu yapılandırılan sınırdan daha büyüktür
* İstek gövdesi (dosyalar dahil) sınırdan daha büyüktür
* WAF motorunda bir iç hata oluştu

CRS 3.x özel:

* Gelen anomali puanı eşik aşıldı

## <a name="next-steps"></a>Sonraki adımlar

Devre dışı bırakılmış kurallarınızı yapılandırıldıktan sonra WAF günlüklerinizi nasıl görüntülediğinizi öğrenebilirsiniz. Daha fazla bilgi için [Uygulama Ağ Geçidi tanılama](../../application-gateway/application-gateway-diagnostics.md#diagnostic-logging)bakın.

[fig1]: ../media/application-gateway-customize-waf-rules-portal/1.png
[3]: ../media/application-gateway-customize-waf-rules-portal/figure3.png
