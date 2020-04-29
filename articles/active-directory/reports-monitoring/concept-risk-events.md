---
title: Risk algılamalarını Azure Active Directory | Microsoft Docs
description: Bu arku, risk algılamaları hakkında ayrıntılı bir genel bakış sunar.
services: active-directory
keywords: Azure Active Directory kimlik koruması, güvenlik, risk, risk düzeyi, güvenlik açığı, güvenlik ilkesi
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80383965"
---
# <a name="azure-active-directory-risk-detections"></a>Risk algılamalarını Azure Active Directory

Saldırganlar bir kullanıcının kimliğini çalarak bir ortama erişim kazanıyorsa güvenlik ihlallerinin büyük çoğunluğu bu şekilde gerçekleşir. Güvenliği aşılmış kimlikleri bulma işlemi kolay bir görev değildir. Azure Active Directory, kullanıcı hesaplarınızla ilgili şüpheli eylemleri algılamak için uyarlamalı makine öğrenimi algoritmaları ve buluşsal yöntemler kullanır. Algılanan her şüpheli eylem, **risk algılama**adlı bir kayıtta saklanır.

Bildirilen risk algılamalarını gözden geçibileceğiniz iki yer vardır:

 - **Azure AD raporlama** -risk ALGıLAMALARı Azure AD 'nin güvenlik raporlarının bir parçasıdır. Daha fazla bilgi için [risk altındaki kullanıcılar güvenlik raporuna](concept-user-at-risk.md) ve [riskli oturum açma güvenlik raporuna](concept-risky-sign-ins.md)bakın.

 - **Azure AD kimlik koruması** riskli algılamalar Ayrıca [Azure Active Directory kimlik koruması](../active-directory-identityprotection.md)raporlama yeteneklerinin bir parçasıdır.

Ayrıca, Microsoft Graph kullanarak güvenlik algılamalarından programlı erişim kazanmak için [kimlik koruması risk ALGıLAMA API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/identityriskevent) 'sini de kullanabilirsiniz. Daha fazla bilgi için bkz. [Azure Active Directory kimlik koruması ve Microsoft Graph ile çalışmaya başlama](../identity-protection/graph-get-started.md). 

Şu anda Azure Active Directory altı riskli algılama türünü algılar:

- [Sızan kimlik bilgilerine sahip kullanıcılar](#leaked-credentials) 
- [Anonim IP adreslerinden oturum açma işlemleri](#sign-ins-from-anonymous-ip-addresses) 
- [Alışılmadık konumlara imkansız seyahat](#impossible-travel-to-atypical-locations) 
- [Bulaşma olan cihazlardan oturum açma işlemleri](#sign-ins-from-infected-devices) 
- [Şüpheli etkinlik gösteren IP adreslerinden gerçekleştirilen oturum açma işlemleri](#sign-ins-from-ip-addresses-with-suspicious-activity) 
- [Alışılmadık konumlardan oturum açma işlemleri](#sign-in-from-unfamiliar-locations) 

![Risk algılama](./media/concept-risk-events/91.png)

> [!IMPORTANT]
> Bazen, [oturum açma raporuna](concept-sign-ins.md)karşılık gelen bir oturum açma girişi olmadan bir risk algılamayı bulabilirsiniz. Bunun nedeni, kimlik korumasının hem **etkileşimli** hem de **etkileşimli olmayan** oturum açma işlemlerinin riskini değerlendirmesinde, oturum açma raporunda yalnızca etkileşimli oturum açma işlemleri gösterilir.

Algılanan bir risk algılaması için aldığınız Öngörüler, Azure AD aboneliğinize bağlıdır. 

* **Azure AD Premium P2 sürümüyle**, temeldeki Tüm algılamalar hakkında en ayrıntılı bilgileri alırsınız. 
* **Azure AD Premium P1 sürümü**ile, gelişmiş algılamalar (bilmediğiniz oturum açma özellikleri gibi) lisansınızın kapsamında değildir ve ad **oturum açma tarafından algılanan ek risk**altında görünür. Ayrıca, risk düzeyi ve risk ayrıntıları alanları gizlidir.

Risk algılamaları algılanırken, kimliklerinizi korumanın önemli bir yönü zaten var olsa da, koşullu erişim ilkelerini yapılandırarak onları el ile adresleyerek veya otomatikleştirilmiş yanıtları uygulama seçeneğiniz de vardır. Daha fazla bilgi için bkz. [Azure Active Directory Kimlik Koruması](../active-directory-identityprotection.md).

## <a name="risk-detection-types"></a>Risk algılama türleri

**Risk algılama türü** özelliği, için risk algılama kaydının oluşturulduğu şüpheli eyleme yönelik bir tanıtıcıdır.

Microsoft 'un algılama işlemine yönelik sürekli yatırımlar şu şekilde yapılır:

- Mevcut risk algılamalarının algılama doğruluğunu geliştirmeye yönelik iyileştirmeler 
- Daha sonra eklenecek yeni risk algılama türleri

### <a name="leaked-credentials"></a>Sızdırılan kimlik bilgileri

Sidolandırıcılar meşru kullanıcıların geçerli parolalarını tehlikeye ayorsa, genellikle bu kimlik bilgilerini paylaşır. Bu, genellikle koyu Web üzerinde herkese açık bir şekilde gönderilir veya siteleri yapıştırabilir ya da siyah Pazar üzerine kimlik bilgileri satıyor veya satarak yapılır. Microsoft sızdırılan kimlik bilgileri hizmeti, genel ve koyu Web sitelerini izleyerek ve ile çalışarak Kullanıcı adı/parola çiftleri alır:

- Araştırmacıları
- Yasalar zorlaması
- Microsoft 'ta güvenlik ekipleri
- Diğer güvenilir kaynaklar 

Hizmet, Kullanıcı adı/parola çiftleri edindiğinde AAD kullanıcılarının geçerli geçerli kimlik bilgileriyle denetlenir. Bir eşleşme bulunduğunda, kullanıcının parolasının tehlikeye girdiği ve bir **sızdırılan kimlik bilgileri risk algılaması** oluşturulduğu anlamına gelir.

### <a name="sign-ins-from-anonymous-ip-addresses"></a>Anonim IP adreslerinden oturum açma işlemleri

Bu risk algılama türü, anonim proxy IP adresi olarak tanımlanmış bir IP adresinden başarıyla oturum açan kullanıcıları tanımlar. Bu proxy 'ler, cihazının IP adreslerini gizlemek isteyen kişiler tarafından kullanılır ve kötü amaçlı olarak kullanılabilir.

### <a name="impossible-travel-to-atypical-locations"></a>Alışılmadık konumlara imkansız seyahat

Bu risk algılama türü, coğrafi olarak uzak konumlardan kaynaklanan iki oturum açma işlemini tanımlar. Bu konumlar, konumların en az birinin Kullanıcı için de normal bir şekilde (geçmiş davranışa göre) de olabileceğini belirtir. Bu makine öğrenimi algoritması, farklı bir kullanıcının aynı kimlik bilgilerini kullandığını belirten, iki oturum açma işlemi ve kullanıcının ilk konumdan ikincisine geçebileceği zaman arasındaki süreyi dikkate alır.

Algoritma, kuruluştaki diğer kullanıcılar tarafından düzenli olarak kullanılan VPN 'Ler ve konumlar gibi mümkün olmayan seyahat koşullarına katkıda bulunan açık "yanlış pozitif sonuçlar" i yoksayar. Sistemin yeni bir kullanıcının oturum açma davranışını öğrenirken 14 günlük ilk öğrenme dönemi vardır. 

### <a name="sign-in-from-unfamiliar-locations"></a>Bilmediğiniz konumlardan oturum açma

Bu risk algılama türü, yeni/bilinmeyen konumları saptamak için son oturum açma konumlarını (IP, Enlem/Boylam ve ASN) dikkate alır. Sistem, bir kullanıcı tarafından kullanılan önceki konumlara ilişkin bilgileri depolar ve bu "tanıdık" konumları dikkate alır. Risk algılama, oturum açma, zaten tanıdık konumlar listesinde olmayan bir konumdan gerçekleştirildiğinde tetiklenir. Sistemin ilk öğrenme dönemi 30 gün, bu sırada hiçbir yeni konumu bilmediğiniz olmayan konumlar olarak bayrak eklemez. Sistem ayrıca tanıdık cihazlardan oturum açma işlemlerini ve coğrafi olarak tanıdık bir konuma yakın konumları yoksayar. 

Kimlik koruması, temel kimlik doğrulaması/eski protokoller için de bilinmeyen konumlardan oturum açma işlemlerini algılar. Bu protokollerde istemci kimliği gibi modern tanıdık Özellikler olmadığından, hatalı pozitif sonuçları azaltmak için yeterli telemetri yoktur. Algılanan risk algılamaları sayısını azaltmak için modern kimlik doğrulamasına geçmeniz gerekir.   

> [!NOTE]
> Oturum açma Kullanıcı adı ve parolası eşleşmiyorsa, oturum açma başarısız olur ve risk algılaması gerçekleşmez. Bilinmeyen bir konumdan oturum açma risk algılamaları yalnızca başarılı oturumlarda tetiklenir.

### <a name="sign-ins-from-infected-devices"></a>Bulaşma olan cihazlardan oturum açma işlemleri

Bu risk algılama türü, kötü amaçlı yazılımdan etkilenen cihazlardan oturum açma işlemlerini tanımlar ve bu, bir bot sunucusuyla etkin bir şekilde iletişim kuran bilinmektedir. Bu, Kullanıcı cihazının IP adreslerinin, bir bot sunucusuyla iletişim içinde olan IP adresleriyle ilişkilendirilmesi ile belirlenir. 

### <a name="sign-ins-from-ip-addresses-with-suspicious-activity"></a>Şüpheli etkinlik gösteren IP adreslerinden gerçekleştirilen oturum açma işlemleri
Bu risk algılama türü, çok sayıda kullanıcı hesabında, kısa bir süre boyunca çok sayıda başarısız oturum açma girişiminin görüldüğü IP adreslerini tanımlar. Bu, saldırganlar tarafından kullanılan IP adreslerinin trafik desenleriyle eşleşir ve hesapların zaten ya da tehlikede olduğu bir güçlü göstergedir. Bu, kuruluştaki diğer kullanıcılar tarafından düzenli olarak kullanılan IP adresleri gibi belirgin hatalı pozitif durumları yok sayan bir makine öğrenimi algoritmasıdır.  Sistemin, yeni bir kullanıcının ve yeni kiracının oturum açma davranışını öğrendiğinde 14 günlük ilk öğrenme dönemi vardır.

## <a name="detection-type"></a>Algılama türü

Algılama türü özelliği, risk algılamada algılama zaman çerçevesi için bir göstergedir (**gerçek** zamanlı veya **çevrimdışı**). Şu anda, risk algılaması gerçekleştirildikten sonra bir işlem sonrası işleminde en çok risk algılamaları çevrimdışı olarak algılanır.

Aşağıdaki tabloda, bir algılama türünün ilgili bir raporda görünmesi için gereken süre miktarı listelenmektedir:

| Algılama türü | Raporlama gecikmesi |
| --- | --- |
| Gerçek zamanlı | 5 ila 10 dakika |
| Çevrimdışı | 2-4 saat |


Azure Active Directory algıladığı risk algılama türleri için, algılama türleri şunlardır:

| Risk algılama türü | Algılama türü |
| :-- | --- | 
| [Sızan kimlik bilgilerine sahip kullanıcılar](#leaked-credentials) | Çevrimdışı |
| [Anonim IP adreslerinden oturum açma işlemleri](#sign-ins-from-anonymous-ip-addresses) | Gerçek zamanlı |
| [Alışılmadık konumlara imkansız seyahat](#impossible-travel-to-atypical-locations) | Çevrimdışı |
| [Alışılmadık konumlardan oturum açma işlemleri](#sign-in-from-unfamiliar-locations) | Gerçek zamanlı |
| [Bulaşma olan cihazlardan oturum açma işlemleri](#sign-ins-from-infected-devices) | Çevrimdışı |
| [Şüpheli etkinlik gösteren IP adreslerinden gerçekleştirilen oturum açma işlemleri](#sign-ins-from-ip-addresses-with-suspicious-activity) | Çevrimdışı|


## <a name="risk-level"></a>Risk düzeyi

Risk algılamanın risk düzeyi özelliği, bir riskin önem derecesi ve güvenilirliği için bir göstergedir (**yüksek**, **Orta**veya **düşük**). Bu özellik, gerçekleştirmeniz gereken eylemleri önceliklendirmenize yardımcı olur. 

Risk algılamanın önem derecesi, sinyalin bir kimlik uzlaşması olarak tahmin düzeyini temsil eder. Güvenirlik, hatalı pozitif sonuçlar olasılığını için bir göstergedir. 

Örneğin, 

* **Yüksek**: yüksek güvenilirlik ve yüksek öneme sahip risk algılama. Bu olaylar, Kullanıcı kimliğinin tehlikeye girdiği güçlü göstergeler ve etkilenen tüm Kullanıcı hesapları hemen düzeltilmelidir.

* **Orta**: yüksek önem derecesi, ancak daha düşük güvenilirlik riski algılama veya tam tersi. Bu olaylar riskli olabilir ve etkilenen tüm Kullanıcı hesapları düzeltilmelidir.

* **Düşük**: düşük güvenilirlik ve düşük önem derecesi riski algılama. Bu olay acil eylem gerektirmeyebilir, ancak diğer risk algılamaları ile birleştirildiğinde, kimliğin güvenliğinin aşıldığına yönelik güçlü bir gösterge sağlayabilir.

![Risk düzeyi](./media/concept-risk-events/01.png)

### <a name="leaked-credentials"></a>Sızdırılan kimlik bilgileri

Sızdırılan kimlik bilgileri risk algılamaları, Kullanıcı adı ve parolasının bir saldırgan tarafından kullanılabilir olduğunu açık bir gösterge sağladığından **yüksek**olarak sınıflandırılmaktadır.

### <a name="sign-ins-from-anonymous-ip-addresses"></a>Anonim IP adreslerinden oturum açma işlemleri

Anonim bir IP adresi bir hesap güvenliğinin kesin bir göstergesi olmadığından, bu risk algılama türünün risk düzeyi **Orta** değildir. Anonim IP adresleri kullanıp kullandıklarından emin olmak için kullanıcıyla hemen iletişim kurmanız önerilir.


### <a name="impossible-travel-to-atypical-locations"></a>Alışılmadık konumlara imkansız seyahat

Mümkün olmayan seyahat genellikle bir korsanın başarıyla oturum açabilme konusunda iyi bir göstergedir. Bununla birlikte, bir Kullanıcı yeni bir cihaz kullanarak veya kuruluştaki diğer kullanıcılar tarafından kullanılmayan bir VPN kullanarak seyahat edildiğinde yanlış pozitif sonuçlar ortaya çıkabilir. Başka bir yanlış pozitif sonuç kaynağı, sunucu IP 'lerini istemci IP 'Leri olarak yanlış şekilde geçiren uygulamalardır. Bu, uygulamanın arka ucunun barındırıldığı veri merkezinden (çoğunlukla Microsoft 'un sahip olduğu IP adreslerinden gelen oturum açma bilgilerini veren Microsoft veri merkezleridir) oturum açma işlemlerinin görünümünü veren uygulamalardır. Bu yanlış pozitif sonuçlar nedeniyle, bu risk algılaması için risk düzeyi **Orta**' dir.

> [!TIP]
> [Adlandırılmış konumları](../active-directory-named-locations.md)yapılandırarak bu risk algılama türü için bildirilen hatalı pozitif sonuç miktarını azaltabilirsiniz. 

### <a name="sign-in-from-unfamiliar-locations"></a>Bilmediğiniz konumlardan oturum açma

Bilmediğiniz konumlar, bir saldırganın çalınmış bir kimlik kullanabilmesi için güçlü bir bildirim sağlayabilir. Kullanıcı seyahat ederken, yeni bir cihaz denerken veya yeni bir VPN kullanırken yanlış pozitif sonuçlar oluşabilir. Bu yanlış pozitif sonuçlar nedeniyle, bu olay türü için risk düzeyi **Orta**olur.

### <a name="sign-ins-from-infected-devices"></a>Bulaşma olan cihazlardan oturum açma işlemleri

Bu risk algılama, Kullanıcı aygıtlarını değil, IP adreslerini tanımlar. Birden çok cihaz tek bir IP adresinin arkasındaysa ve yalnızca bazıları bir bot ağı tarafından denetleniyorsa, diğer cihazlardan oturum açma işlemleri bu olayı gereksiz yere tetiklerim, bu da bu risk algılama işleminin **düşük**olarak sınıflandırıldığı anlamına gelir.  

Kullanıcıyla iletişim kurmanız ve tüm kullanıcıların cihazlarını taramanızı öneririz. Ayrıca, bir kullanıcının kişisel cihazının virüslü olması veya başka birinin kullanıcıyla aynı IP adresinden virüslü bir cihaz kullanıyor olması mümkündür. Virüslü cihazlar genellikle virüsten koruma yazılımı tarafından henüz tanımlanmayan kötü amaçlı yazılımlardan etkilenir ve cihazın virüs bulaşmış olmasına neden olabilecek kötü amaçlı kullanıcı alışkanlıklarını da belirtebilir.

Kötü amaçlı yazılımlardan etkilenme hakkında daha fazla bilgi için bkz. [kötü amaçlı yazılımdan koruma merkezi](https://www.microsoft.com/en-us/security/portal/definitions/adl.aspx/).

### <a name="sign-ins-from-ip-addresses-with-suspicious-activity"></a>Şüpheli etkinlik gösteren IP adreslerinden gerçekleştirilen oturum açma işlemleri

Şüpheli olarak işaretlenen bir IP adresinden gerçekten oturum açtıklarından emin olmak için kullanıcıyla iletişim kurmanız önerilir. Bu olay türü için risk düzeyi "**Orta**" olduğundan, birkaç CIHAZ aynı IP adresinin arkasında olabileceğinden, yalnızca bazıları şüpheli etkinlikten sorumlu olabilir. 


## <a name="next-steps"></a>Sonraki Adımlar

* [Risk altındaki kullanıcılar güvenlik raporu](concept-user-at-risk.md)
* [Riskli oturum açma işlemleri güvenlik raporu](concept-risky-sign-ins.md)
* [Azure AD kimlik koruması](../active-directory-identityprotection.md).
