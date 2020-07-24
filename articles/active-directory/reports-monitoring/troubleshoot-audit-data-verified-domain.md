---
title: Doğrulanan etki alanı değişikliğinin denetim verileri sorunlarını giderme | Microsoft Docs
description: Kullanıcı tarafından doğrulanan etki alanını değiştirdiğinizde Azure Active Directory etkinlik günlüklerinde görünecek bilgiler sağlar.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 07/22/2020
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: f3c9ec3b1e96e47dbf46c6acb2c81147b614d069
ms.sourcegitcommit: 0820c743038459a218c40ecfb6f60d12cbf538b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87117440"
---
# <a name="troubleshoot-audit-data-on-verified-domain-change"></a>Sorun giderme: doğrulanan etki alanı değiştiğinde denetim verileri 


## <a name="i-have-a-lot-of-changes-to-my-users-and-i-am-not-sure-what-the-cause-of-it-is"></a>Kullanıcılarıma çok fazla değişiklik yapmam ve bunun nedeninin ne olduğundan emin olmadım.

### <a name="symptoms"></a>Belirtiler

Azure AD denetim günlüklerini denetm ve Azure AD kiracımda birden çok Kullanıcı güncelleştirmesi olduğunu görüyorum. Bu **güncelleştirme Kullanıcı** olayları, kullanıcılara toplu değişikliklerin ne yaptığını ne de tetiklediğini belirleyen **aktör** bilgilerini görüntülemez. 

### <a name="cause"></a>Nedeni

 Yığın nesnesi değişikliklerinin arka planda yaygın bir nedeni, **Proxycalc**adlı zaman uyumlu olmayan bir arka uç işlemidir.  **Proxycalc** , Azure AD kullanıcıları, grupları veya kişileri 'nde güncelleştirilmiş uygun **userPrincipalName** ve **proxy adreslerini**belirleyen mantığdır. **Proxycalc** arkasındaki tasarım, tüm **userPrincipalName** ve **proxy adreslerinin** Azure AD 'de dilediğiniz zaman tutarlı olmasını sağlamaktır. **Proxycalc** , doğrulanmış bir etki alanı değişikliği gibi açık bir değişiklik tarafından tetiklenmesi gerekir ve bir görev olarak arka planda çalışmıyor adet sürekli. 

  

#### <a name="what-does-userprincipalname-consistency-mean"></a>UserPrincipalName tutarlılığı ne anlama geliyor? 

Yalnızca bulutta bulunan kullanıcılar için tutarlılık, **userPrincipalName** 'in doğrulanmış bir etki alanı sonekine ayarlandığı anlamına gelir. Tutarsız bir **userPrincipalName** Işlendiğinde, **proxycalc** onu varsayılan onmicrosoft.com sonekine dönüştürür, örneğin:username@Contoso.onmicrosoft.com 

Eşitlenmiş kullanıcılar için tutarlılık, **userPrincipalName** 'in doğrulanmış bir etki alanı sonekine ayarlandığı ve şirket içi **userPrincipalName** değeriyle (shadowuserprincipalname) eşleştiği anlamına gelir. Tutarsız bir **userPrincipalName** Işlendiğinde, **Proxycalc** , **shadowuserprincipalname** ile aynı değere döndürülür veya etki alanı son ekinin kiracıdan kaldırılması durumunda, bunu varsayılan *. onmicrosoft.com etki alanı sonekine dönüştürür. 

  

#### <a name="what-does-proxy-address-consistency-mean"></a>Proxy adresi tutarlılığı ne anlama geliyor? 

Yalnızca bulutta bulunan kullanıcılar için tutarlılık, proxy adreslerinin doğrulanmış bir etki alanı sonekiyle eşleştiği anlamına gelir. Tutarsız bir proxy adresi işlendiğinde, **Proxycalc** onu varsayılan *. onmicrosoft.com etki alanı sonekine dönüştürür, örneğin:SMTP:username@Contoso.onmicrosoft.com 

Eşitlenmiş kullanıcılar için tutarlılık, proxy adreslerinin şirket içi ara sunucu adresleri (ler) değerleri ile eşleştiği anlamına gelir (ör. ShadowProxyAddresses). **ProxyAddresses** 'ın **ShadowProxyAddresses**ile eşitlenmesi bekleniyor. Eşitlenmiş kullanıcıya bir Exchange lisansı atanmışsa, proxy adreslerinin şirket içi proxy adresi (ler) değeriyle eşleşmesi ve ayrıca doğrulanmış bir etki alanı sonekiyle eşleşmesi gerekir. Bu senaryoda, **Proxycalc** , doğrulanmamış bir etki alanı sonekine sahip tutarsız proxy adresini temizler ve Azure AD 'deki nesnesinden kaldırılacak. Bu doğrulanmamış etki alanı daha sonra doğrulanırsa, **Proxycalc** yeniden hesaplama ve proxy adresini **SHADOWPROXYADDRESSES** 'den Azure AD 'deki nesnesine eklemektir.  

> [!NOTE]
> Eşitlenmiş nesneler için, **Proxycalc** mantığının beklenmedik sonuçları hesaplamasından kaçınmak Için, proxy adreslerini şirket içi nesnede BIR Azure AD doğrulanmış etki alanına ayarlamak en iyisidir.  

  
**Proxycalc** tetikleyesağlayan yönetici görevlerden biri doğrulanmış bir etki alanı değişikliği olduğunda olur. Bu görev, bir Azure AD kiracısından her bir doğrulanan etki alanı eklendiğinde/kaldırıldığında **oluşur.**  

Örneğin, Contoso.onmicrosoft.com kiracınıza doğrulanmış bir etki alanı Fabrikam.com eklerseniz, bu eylem Kiracıdaki tüm nesnelerde bir ProxyCalc işlemi tetikler. Bu olay, **doğrulanmış bir etki alanı Ekle** olayı tarafından öncesinde **Kullanıcı olaylarını GÜNCELLEŞTIR** olarak Azure AD denetim günlüklerinde yakalanacaktır. Diğer taraftan, Fabrikam.com Contoso.onmicrosoft.com kiracısından kaldırılmışsa, tüm **Kullanıcı** olaylarına yönelik bir daha önce **doğrulanmış bir etki alanı** olayı gönderilir.   

#### <a name="additional-notes"></a>Ek notlar:

ProxyCalc, belirli nesnelerde değişiklik yapılmasına neden olmaz: 

- etkin bir Exchange lisansına sahip değilsiniz 
- **msExchRemoteRecipientType** null olarak ayarlanmış 
- , paylaşılan bir kaynak olarak kabul edilmez. **Cloudmsexchrecipientdisplaytype** , şu değerlerden birini Içerdiğinde paylaşılan kaynak: **mailboxuser (Shared)**, **PublicFolder**, **ConferenceRoomMailbox**, **ekipmanımailbox**, **Aracı, yönetim**kutusu, **roomlist**, **teammailboxuser**, **Grup posta kutusu**, **zamanlama posta kutusu**, **aclablemailboxuser**, **aclableteammailboxuser** 
  
 Bu iki farklı olay arasında daha fazla bağıntı oluşturmak için, Microsoft bu değişiklikleri doğrulanmış bir etki alanı değişikliği tarafından tetiklenen şekilde belirlemek üzere denetim günlüklerinde **aktör** bilgilerini güncelleştirme üzerinde çalışmaktadır. Bu eylem, doğrulanmış etki alanı değişikliği olayının ne zaman gerçekleştiği ve kiracısındaki nesneleri toplu olarak güncelleştirmeye başladığında denetlemeye yardımcı olur. 

Ayrıca, çoğu durumda, Kullanıcı tarafından **userPrincipalName** ve **Ara sunucu adresleri** tutarlı olduğundan, denetim günlüklerinde görüntülenmek üzere yalnızca nesnenin gerçek değişikliğine neden olan güncelleştirmeler için çalışıyoruz. Bu eylem, denetim günlüklerinde paraziti engeller ve yöneticilerin, kalan Kullanıcı değişikliklerini, yukarıda açıklandığı gibi doğrulanmış etki alanı değişikliği olayına ilişkilendirmenize yardımcı olur. 

## <a name="next-steps"></a>Sonraki Adımlar

[Azure AD Connect eşitleme hizmeti gölge öznitelikleri](../hybrid/how-to-connect-syncservice-shadow-attributes.md)
