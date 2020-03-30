---
title: Azure Active Directory risk algılamaları | Microsoft Dokümanlar
description: Bu iş, risk algılamalarının ne olduğuna ayrıntılı bir genel bakış sağlar.
services: active-directory
keywords: azure aktif dizin kimlik koruması, güvenlik, risk, risk düzeyi, güvenlik açığı, güvenlik ilkesi
author: MarkusVi
manager: daveba
ms.assetid: fa2c8b51-d43d-4349-8308-97e87665400b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4caa248f6972609ecb6bf71dd521c68d78cebd70
ms.sourcegitcommit: e040ab443f10e975954d41def759b1e9d96cdade
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2020
ms.locfileid: "80383965"
---
# <a name="azure-active-directory-risk-detections"></a>Azure Active Directory risk algılamaları

Güvenlik ihlallerinin büyük çoğunluğu, saldırganların bir kullanıcının kimliğini çalarak bir ortama erişmelerinde gerçekleşir. Tehlikeye edilmiş kimlikleri keşfetmek kolay bir iş değildir. Azure Active Directory, kullanıcı hesaplarınızla ilgili şüpheli eylemleri algılamak için uyarlanabilir makine öğrenme algoritmaları ve buluşsal algoritmalar kullanır. Algılanan her şüpheli **eylem, risk algılama**adı verilen bir kayıtta depolanır.

Bildirilen risk tespitlerini gözden geçirdiğiniz iki yer vardır:

 - **Azure AD raporlaması** - Risk algılamaları Azure AD'nin güvenlik raporlarının bir parçasıdır. Daha fazla bilgi için risk [güvenliği raporundaki kullanıcılara](concept-user-at-risk.md) ve [riskli oturum açma güvenlik raporuna](concept-risky-sign-ins.md)bakın.

 - **Azure AD Kimlik Koruması** - Risk algılamaları da [Azure Active Directory Identity Protection'ın](../active-directory-identityprotection.md)raporlama özelliklerinin bir parçasıdır.

Ayrıca, Microsoft Graph'ı kullanarak güvenlik algılamalarına programlı erişim elde etmek için [Kimlik Koruması risk algılamaları API'sini](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/identityriskevent) kullanabilirsiniz. Daha fazla bilgi için Azure [Active Directory Identity Protection ve Microsoft Graph ile başlayın'](../identity-protection/graph-get-started.md)a bakın. 

Şu anda Azure Etkin Dizini altı tür risk algılaması algılar:

- [Sızan kimlik bilgilerine sahip kullanıcılar](#leaked-credentials) 
- [Anonim IP adreslerinden oturum açma işlemleri](#sign-ins-from-anonymous-ip-addresses) 
- [Alışılmadık konumlara imkansız seyahat](#impossible-travel-to-atypical-locations) 
- [Bulaşma olan cihazlardan oturum açma işlemleri](#sign-ins-from-infected-devices) 
- [Şüpheli etkinlik gösteren IP adreslerinden gerçekleştirilen oturum açma işlemleri](#sign-ins-from-ip-addresses-with-suspicious-activity) 
- [Alışılmadık konumlardan oturum açma işlemleri](#sign-in-from-unfamiliar-locations) 

![Risk tespiti](./media/concept-risk-events/91.png)

> [!IMPORTANT]
> Bazen, [oturum açma raporunda](concept-sign-ins.md)karşılık gelen bir oturum açma girişi olmadan bir risk algılama bulabilirsiniz. Bunun nedeni, Kimlik Korumasının hem **etkileşimli** hem de **etkileşimli olmayan** oturum açma riskini değerlendirirken, oturum açma raporunda yalnızca etkileşimli oturum açma ları gösterir.

Algılanan bir risk algılama için aldığınız bilgiler Azure AD aboneliğinize bağlıdır. 

* Azure **AD Premium P2 sürümü**yle, temel deki tüm algılamalar hakkında en ayrıntılı bilgilere sahip olursunuz. 
* Azure **AD Premium P1 sürümünde,** gelişmiş algılamalar (bilmediğiniz oturum açma özellikleri gibi) lisansınız kapsamında değildir ve **ek risk algılanan Oturum Açma**adı altında görünür. Ayrıca, risk düzeyi ve risk ayrıntı alanları gizlenir.

Risk algılamanın algılanması zaten kimliklerinizi korumanın önemli bir yönünü temsil etse de, Koşullu Erişim ilkelerini yapılandırarak bunları el ile ele alma veya otomatik yanıtları uygulama seçeneğiniz de vardır. Daha fazla bilgi için bkz. [Azure Active Directory Kimlik Koruması](../active-directory-identityprotection.md).

## <a name="risk-detection-types"></a>Risk algılama türleri

**Risk algılama türü** özelliği, bir risk algılama kaydı nın oluşturulduğu şüpheli eylem için bir tanımlayıcıdır.

Microsoft'un algılama sürecine sürekli yatırımları aşağıdakilere yol açar:

- Mevcut risk tespitlerinin algılama doğruluğunda iyileştirmeler 
- Gelecekte eklenecek yeni risk algılama türleri

### <a name="leaked-credentials"></a>Sızdırılan kimlik bilgileri

Siber suçlular yasal kullanıcıların geçerli parolalarını tehlikeye attıklarında, genellikle bu kimlik bilgilerini paylaşırlar. Bu genellikle karanlık web veya yapıştırma sitelerinde kamuya yayınlayarak veya ticaret veya karaborsada kimlik bilgileri satarak yapılır. Microsoft sızdırılan kimlik bilgileri hizmeti, genel ve karanlık web sitelerini izleyerek ve aşağıdakilerle çalışarak kullanıcı adı / parola çiftleri edinir:

- Araştırmacı -lar
- Kolluk
- Microsoft'ta güvenlik ekipleri
- Diğer güvenilir kaynaklar 

Hizmet kullanıcı adı / parola çiftleri edindiğinde, AAD kullanıcılarının geçerli kimlik bilgileriyle karşılaştırılır. Bir eşleşme bulunduğunda, kullanıcının parolası ele geçirilmiş ve **sızdırılmış kimlik bilgileri risk algılama** sı oluşturulur.

### <a name="sign-ins-from-anonymous-ip-addresses"></a>Anonim IP adreslerinden oturum açma işlemleri

Bu risk algılama türü, anonim proxy IP adresi olarak tanımlanan bir IP adresinden başarıyla oturum açmış kullanıcıları tanımlar. Bu ekskavatlar, cihazlarının IP adresini gizlemek isteyen kişiler tarafından kullanılır ve kötü amaçlı olarak kullanılabilir.

### <a name="impossible-travel-to-atypical-locations"></a>Alışılmadık konumlara imkansız seyahat

Bu risk algılama türü, geçmiş davranışlar göz önüne alındığında, konumlardan en az birinin kullanıcı için atipik olabileceği coğrafi olarak uzak konumlardan kaynaklanan iki oturum açma yı tanımlar. Diğer bazı faktörlerin yanı sıra, bu makine öğrenme algoritması, iki oturum açma arasındaki süreyi ve kullanıcının ilk konumdan ikinci ye seyahat etmesi için gereken süreyi dikkate alır ve farklı bir kullanıcının aynı Kimlik bilgi -leri.

Algoritma, VPN'ler ve kuruluştaki diğer kullanıcılar tarafından düzenli olarak kullanılan konumlar gibi imkansız seyahat koşullarına katkıda bulunan bariz "yanlış pozitifleri" yok sayar. Sistem, yeni bir kullanıcının oturum açma davranışını öğrendiği 14 günlük bir başlangıç öğrenme süresine sahiptir. 

### <a name="sign-in-from-unfamiliar-locations"></a>Yabancı konumlardan oturum açma

Bu risk algılama türü, yeni / yabancı konumları belirlemek için geçmiş oturum açma konumlarını (IP, Latitude / Boylam ve ASN) dikkate alır. Sistem, kullanıcı tarafından kullanılan önceki konumlar la ilgili bilgileri depolar ve bu "tanıdık" konumları dikkate alır. Oturum açma, tanıdık konumlar listesinde zaten olmayan bir konumdan gerçekleştiğinde risk algılama tetiklenir. Sistem, 30 günlük bir başlangıç öğrenme süresine sahiptir ve bu süre zarfında yeni yerleri yabancı konumlar olarak işaretlemez. Sistem ayrıca, tanıdık aygıtlardan gelen oturum açma ları ve coğrafi olarak tanıdık bir konuma yakın olan konumları da yok sayar. 

Kimlik Koruması, temel kimlik doğrulama / eski protokoller için de yabancı konumlardan oturum açma ları algılar. Bu protokoller istemci kimliği gibi modern tanıdık özelliklere sahip olmadığından, yanlış pozitifleri azaltmak için yeterli telemetri yoktur. Algılanan risk algılama larının sayısını azaltmak için, modern kimlik doğrulamasına geçmeniz gerekir.   

> [!NOTE]
> Oturum açma kullanıcı adı ve parola eşleşmiyorsa, oturum açma başarısız olur ve risk algılama gerçekleşmez. Yabancı konum risk algılamalarından oturum açma yalnızca başarılı oturumaçmalarda tetiklenir.

### <a name="sign-ins-from-infected-devices"></a>Bulaşma olan cihazlardan oturum açma işlemleri

Bu risk algılama türü, bir bot sunucusuyla etkin olarak iletişim kurduğu bilinen kötü amaçlı yazılım bulaşmış aygıtlardan gelen oturum açma ları tanımlar. Bu, kullanıcının cihazının IP adreslerinin bir bot sunucusuyla temas halinde olan IP adresleriyle ilişkilendirilerek belirlenir. 

### <a name="sign-ins-from-ip-addresses-with-suspicious-activity"></a>Şüpheli etkinlik gösteren IP adreslerinden gerçekleştirilen oturum açma işlemleri
Bu risk algılama türü, kısa bir süre içinde birden fazla kullanıcı hesabında çok sayıda başarısız oturum açma denemesinin görüldüğü IP adreslerini tanımlar. Bu, saldırganlar tarafından kullanılan IP adreslerinin trafik desenlerini eşleşir ve hesapların zaten veya açığa çıkmak üzere olduğunun güçlü bir göstergesidir. Bu, kuruluştaki diğer kullanıcılar tarafından düzenli olarak kullanılan IP adresleri gibi açık yanlış pozitifleri yok sayan bir makine öğrenme algoritmasıdır.  Sistem, yeni bir kullanıcı ve yeni kiracının oturum açma davranışını öğrendiği 14 günlük bir başlangıç öğrenme süresine sahiptir.

## <a name="detection-type"></a>Algılama türü

Algılama türü özelliği, risk algılama süresi için bir göstergedir **(Gerçek zamanlı** veya **Çevrimdışı).** Şu anda, risk algılama oluştuktan sonra bir işlem sonrası operasyonda çoğu risk algılamaçevrimdışı algılanır.

Aşağıdaki tablo, bir algılama türünün ilgili bir raporda gösterilmesi için gereken süreyi listeler:

| Algılama Türü | Gecikme Gecikmesi Bildirme |
| --- | --- |
| Gerçek zamanlı | 5 ila 10 dakika |
| Çevrimdışı | 2 ila 4 saat |


Azure Active Directory'nin algılayıştığı risk algılama türleri için algılama türleri:

| Risk Algılama Türü | Algılama Türü |
| :-- | --- | 
| [Sızan kimlik bilgilerine sahip kullanıcılar](#leaked-credentials) | Çevrimdışı |
| [Anonim IP adreslerinden oturum açma işlemleri](#sign-ins-from-anonymous-ip-addresses) | Gerçek zamanlı |
| [Alışılmadık konumlara imkansız seyahat](#impossible-travel-to-atypical-locations) | Çevrimdışı |
| [Alışılmadık konumlardan oturum açma işlemleri](#sign-in-from-unfamiliar-locations) | Gerçek zamanlı |
| [Bulaşma olan cihazlardan oturum açma işlemleri](#sign-ins-from-infected-devices) | Çevrimdışı |
| [Şüpheli etkinlik gösteren IP adreslerinden gerçekleştirilen oturum açma işlemleri](#sign-ins-from-ip-addresses-with-suspicious-activity) | Çevrimdışı|


## <a name="risk-level"></a>Risk düzeyi

Risk algılamanın risk düzeyi özelliği, risk tespitinin şiddeti ve güveni için bir göstergedir **(Yüksek,** **Orta**veya **Düşük).** Bu özellik, gerçekleştirdiğiniz eylemleri önceliklendirmenize yardımcı olur. 

Risk algılamanın şiddeti, kimlik uzlaşmasının bir göstergesi olarak sinyalin gücünü temsil eder. Güven yanlış pozitif olasılığı için bir göstergedir. 

Örneğin, 

* **Yüksek**: Yüksek güven ve yüksek önem riski algılama. Bu olaylar, kullanıcının kimliğinin gizliliğinin ihlal edildiğinin güçlü göstergeleridir ve etkilenen kullanıcı hesaplarının derhal düzeltilmesi gerekir.

* **Orta**: Yüksek şiddet, ancak daha düşük güven riski algılama, ya da tersi. Bu olaylar potansiyel olarak risklidir ve etkilenen kullanıcı hesapları düzeltilmelidir.

* **Düşük**: Düşük güven ve düşük şiddet riski tespiti. Bu olay hemen bir eylem gerektirmeyebilir, ancak diğer risk algılamaları ile birleştirildiğinde, kimliğin tehlikeye girdiğine dair güçlü bir gösterge sağlayabilir.

![Risk Düzeyi](./media/concept-risk-events/01.png)

### <a name="leaked-credentials"></a>Sızdırılan kimlik bilgileri

Sızdırılan kimlik bilgileri risk algılamaları **Yüksek**olarak sınıflandırılır, çünkü kullanıcı adı ve parolanın bir saldırgan tarafından kullanılabildiğinin açık bir göstergesidir.

### <a name="sign-ins-from-anonymous-ip-addresses"></a>Anonim IP adreslerinden oturum açma işlemleri

Anonim bir IP adresi bir hesap uzlaşma güçlü bir göstergesi olmadığından, bu risk algılama türü için risk düzeyi **Orta.** Anonim IP adresleri kullanıp kullanmadıklarını doğrulamak için kullanıcıyla derhal iletişime geçmenizi öneririz.


### <a name="impossible-travel-to-atypical-locations"></a>Alışılmadık konumlara imkansız seyahat

İmkansız seyahat genellikle bir hacker başarıyla oturum başardı iyi bir göstergesidir. Ancak, bir kullanıcı yeni bir aygıt kullanarak seyahat ederken veya genellikle kuruluştaki diğer kullanıcılar tarafından kullanılmayan bir VPN kullanırken yanlış pozitif sonuçlar oluşabilir. Yanlış pozitif diğer bir kaynak da sunucu IP'lerini istemci IP'leri olarak yanlış bir şekilde geçen ve bu uygulamanın arka ucunun barındırıldığı veri merkezinden gerçekleşen oturum açma ların görünümünü veren uygulamalardır (genellikle bunlar Microsoft veri merkezleridir ve bunlar Microsoft veri merkezleridir ve Microsoft'un sahip olduğu IP adreslerinden gerçekleşen oturum açma ların görünümünü verebilir). Bu yanlış pozitiflerin bir sonucu olarak, bu risk tespiti için risk düzeyi **Orta.**

> [!TIP]
> [Adlandırılmış konumları](../active-directory-named-locations.md)yapılandırarak bu risk algılama türü için bildirilen yanlış pozitif miktarını azaltabilirsiniz. 

### <a name="sign-in-from-unfamiliar-locations"></a>Yabancı konumlardan oturum açma

Yabancı konumlar, saldırganın çalıntı bir kimliği kullanabileceğine dair güçlü bir gösterge sağlayabilir. Yanlış pozitif bir kullanıcı seyahat ederken, yeni bir cihaz deniyor veya yeni bir VPN kullanıyor oluşabilir. Bu yanlış pozitif lerin bir sonucu olarak, bu olay türü için risk düzeyi **Orta**.

### <a name="sign-ins-from-infected-devices"></a>Bulaşma olan cihazlardan oturum açma işlemleri

Bu risk algılama, kullanıcı aygıtlarını değil, IP adreslerini tanımlar. Birkaç cihaz tek bir IP adresi arkasında ise ve sadece bazı bir bot ağı tarafından kontrol edilir, diğer cihazlardan oturum açma benim gereksiz yere bu olayı tetiklemek, bu nedenle bu risk algılama **Düşük**olarak sınıflandırılır .  

Kullanıcıyla iletişim kurmanızı ve kullanıcının tüm cihazlarını tarayıp tarayıp tamanızı öneririz. Bir kullanıcının kişisel aygıtına bulaşmış olması veya başka birinin kullanıcıyla aynı IP adresinden virüslü bir aygıt kullanması da mümkündür. Virüslü cihazlar genellikle henüz anti-virüs yazılımı tarafından tespit edilmemiş kötü amaçlı yazılım tarafından enfekte, ve aynı zamanda cihazın enfekte olmasına neden olabilir herhangi bir kötü kullanıcı alışkanlıkları gösterebilir.

Kötü amaçlı yazılım enfeksiyonlarının nasıl ele alınılabildiğini hakkında daha fazla bilgi için [Kötü Amaçlı YazılımLarı Koruma Merkezi'ne](https://www.microsoft.com/en-us/security/portal/definitions/adl.aspx/)bakın.

### <a name="sign-ins-from-ip-addresses-with-suspicious-activity"></a>Şüpheli etkinlik gösteren IP adreslerinden gerçekleştirilen oturum açma işlemleri

Şüpheli olarak işaretlenmiş bir IP adresinden oturum yapıp olmadığını doğrulamak için kullanıcıyla iletişim ebaşvurmanızı öneririz. Bu olay türü için risk düzeyi "**Orta**" dır, çünkü birkaç aygıt aynı IP adresinin arkasında olabilir, ancak şüpheli etkinlikten yalnızca bazıları sorumlu olabilir. 


## <a name="next-steps"></a>Sonraki Adımlar

* [Risk altındaki kullanıcılar güvenlik raporu](concept-user-at-risk.md)
* [Riskli oturum açma güvenlik raporu](concept-risky-sign-ins.md)
* [Azure AD Kimlik Koruması](../active-directory-identityprotection.md).
