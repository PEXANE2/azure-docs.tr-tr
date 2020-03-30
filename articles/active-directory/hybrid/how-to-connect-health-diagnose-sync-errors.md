---
title: Azure AD Connect Health - Yinelenen öznitelik eşitleme hatalarını tanıla | Microsoft Dokümanlar
description: Bu belge, yinelenen öznitelik eşitleme hatalarının tanılama işlemini ve doğrudan Azure portalından verilen adrötasyon senaryolarının olası düzeltmesini açıklar.
services: active-directory
documentationcenter: ''
author: zhiweiwangmsft
manager: maheshu
editor: billmath
ms.service: active-directory
ms.subservice: hybrid
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/11/2018
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 48ed9abf3e088e2581a3dd81b7c89e6b99da3ceb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76897185"
---
# <a name="diagnose-and-remediate-duplicated-attribute-sync-errors"></a>Yinelenen öznitelik eşitleme hatalarını tanılama ve düzeltme

## <a name="overview"></a>Genel Bakış
Eşitleme hatalarını vurgulamak için bir adım daha ileri giderek Azure Active Directory (Azure AD) Connect Health self servis düzeltmeyi sunar. Yinelenen öznitelik eşitleme hatalarını giderir ve Azure AD'den farklı olan nesneleri düzeltir.
Tanı özelliği şu avantajlara sahiptir:
- Yinelenen öznitelik eşitleme hatalarını daraltan bir tanılama yordamı sağlar. Ve belirli düzeltmeler verir.
- Hatayı tek bir adımda gidermek için Azure AD'den ayrılmış senaryolar için bir düzeltme uygular.
- Bu özelliği etkinleştirmek için yükseltme veya yapılandırma gerekmez.
Azure AD hakkında daha fazla bilgi için [Kimlik eşitleme ve yinelenen öznitelik esnekliğine](how-to-connect-syncservice-duplicate-attribute-resiliency.md)bakın.

## <a name="problems"></a>Sorun
### <a name="a-common-scenario"></a>Sık karşılaşılan bir senaryo
**Karantinaya AttributeValueMustBeUnique** ve **AttributeValueMustBeUnique** eşitleme hataları gerçekleştiğinde, Azure AD'de Bir **UserPrincipalName** veya **Proxy Adresleri** çakışması görmek yaygındır. Çakışan kaynak nesneyi şirket içi taraftan güncelleştirerek eşitleme hatalarını çözebilirsiniz. Eşitleme hatası sonraki eşitlemeden sonra çözülür. Örneğin, bu resim, iki kullanıcının **UserPrincipalName'lerinin**çakışması olduğunu gösterir. İkisi de **Joe.J\@contoso.com.** Çakışan nesneler Azure AD'de karantinaya alınır.

![Eşitleme hata ortak senaryosu tanılamak](./media/how-to-connect-health-diagnose-sync-errors/IIdFixCommonCase.png)

### <a name="orphaned-object-scenario"></a>Yetim nesne senaryosu
Bazen, varolan bir kullanıcının **Kaynak Bağlantı**Çapasını kaybettiğini görebilirsiniz. Kaynak nesnenin silinmesi şirket içi Active Directory'de gerçekleşti. Ancak silme sinyali değişikliği hiçbir zaman Azure AD ile senkronize olmadı. Bu kayıp, eşitleme altyapısı sorunları veya etki alanı geçişi gibi nedenlerle olur. Aynı nesne geri yüklendiğinde veya mantıksal olarak yeniden oluşturulduğunda, varolan bir kullanıcı **Kaynak Çapa'dan**eşitlenecek kullanıcı olmalıdır. 

Varolan bir kullanıcı yalnızca bulutnesnesi olduğunda, çakışan kullanıcının Azure AD ile senkronize edildiğini de görebilirsiniz. Kullanıcı varolan nesneyle eşitlenmis olamaz. **Kaynak Çapa'yı**yeniden eşlemenin doğrudan bir yolu yok. Varolan bilgi tabanı hakkında daha fazla bilgi için daha fazla bilgi [edin.](https://support.microsoft.com/help/2647098) 

Örnek olarak, Azure AD'deki varolan nesne Joe'nun lisansını korur. Azure AD'de yinelenen öznitelik durumunda, farklı bir **Kaynak Bağlantı** Noktası olan yeni eşitlenmiş bir nesne oluşur. Şirket içi Active Directory'deki Joe değişiklikleri, Azure AD'deki Joe'nun özgün kullanıcısına (varolan nesnesi) uygulanmaz.  

![Eşitleme hatası tanılama nesne senaryosu](./media/how-to-connect-health-diagnose-sync-errors/IIdFixOrphanedCase.png)

## <a name="diagnostic-and-troubleshooting-steps-in-connect-health"></a>Connect Health'te tanılama ve sorun giderme adımları 
Tanılama özelliği, kullanıcı nesnelerini aşağıdaki yinelenen özniteliklerle destekler:

| Öznitelik adı | Eşitleme hata türleri|
| ------------------ | -----------------|
| UserPrincipalName | KarantinaAtrÖzÖzDeğerMustBeUnique veya ÖznitelikValueMustBeUnique | 
| ProxyAddresses | KarantinaAtrÖzÖzDeğerMustBeUnique veya ÖznitelikValueMustBeUnique | 
| SipProxyAdresi | ÖznitelikValueMustBeUnique | 
| OnPremiseSecurityIdentifier |  ÖznitelikValueMustBeUnique |

>[!IMPORTANT]
> Bu özelliğe erişmek için **Global Admin** izni veya RBAC ayarlarından **Katkıda Bulunan'ın** izni gereklidir.
>

Eşitleme hata ayrıntılarını daraltmak ve daha spesifik çözümler sunmak için Azure portalındaki adımları izleyin:

![Eşitleme hatası tanılama adımları](./media/how-to-connect-health-diagnose-sync-errors/IIdFixSteps.png)

Azure portalından, belirli düzeltilebilir senaryoları belirlemek için birkaç adım atın:  
1.  Durum **Tanıla** sütununa bakın. Durum, bir eşitleme hatasını doğrudan Azure Etkin Dizini'nden düzeltmenin olası bir yolu olup olmadığını gösterir. Başka bir deyişle, hata örneğini daraltıp büyük ihtimalle düzeltebilecek bir sorun giderme akışı vardır.

| Durum | Bu ne anlama geliyor? |
| ------------------ | -----------------|
| Başlamadı | Bu teşhis sürecini ziyaret etmedin. Tanılama sonucuna bağlı olarak, eşitleme hatasını doğrudan portaldan düzeltmenin olası bir yolu vardır. |
| Manuel Düzeltme Gerekli | Hata, portaldaki kullanılabilir düzeltmelerin ölçütlerini sığdırmıyor. Çakışan nesne türleri kullanıcılar değildir veya tanılama adımlarını zaten geçtiniz ve portaldan düzeltme çözümü yoktu. İkinci durumda, şirket içi taraftan bir düzeltme hala çözümlerden biridir. [Şirket içi düzeltmeler hakkında daha fazla bilgi edinin.](https://support.microsoft.com/help/2647098) | 
| Bekleyen Eşitleme | Bir düzeltme uygulandı. Portal, hatayı temizlemek için bir sonraki eşitleme döngüsünü bekliyor. |

  >[!IMPORTANT]
  > Tanılama durum sütunu her eşitleme döngüsünden sonra sıfırlanacaktır. 
  >

1. Hata ayrıntılarının altındaki **Tanıla** düğmesini seçin. Birkaç soruyu yanıtlar ve eşitleme hatası ayrıntılarını tanımlarsınız. Soruların yanıtları, yetim bir nesne örneğini tanımlamaya yardımcı olur.

1. Tanılamanın sonunda **Bir Kapat** düğmesi görünürse, yanıtlarınıza bağlı olarak portaldan hızlı düzeltme yoktur. Son adımda gösterilen çözüme bakın. Şirket içi düzeltmeler hala çözümlerdir. **Kapat** düğmesini seçin. Geçerli eşitleme hatasının **durumu, gerekli El ile düzeltmeye**geçer. Durum geçerli eşitleme döngüsü sırasında kalır.

1. Yetim bir nesne servis talebi tanımlandıktan sonra, yinelenen öznitelikler eşitleme hatalarını doğrudan portaldan düzeltebilirsiniz. İşlemi tetiklemek için Düzelt düğmesini **uygula** düğmesini seçin. **Bekleyen eşitleme**için geçerli eşitleme hatası güncelleştirmeleri durumu .

1. Bir sonraki eşitleme döngüsünden sonra, hata listeden kaldırılmalıdır.

## <a name="how-to-answer-the-diagnosis-questions"></a>Tanı soruları nasıl cevaplanır? 
### <a name="does-the-user-exist-in-your-on-premises-active-directory"></a>Kullanıcı şirket içi Active Directory'nizde var mı?

Bu soru, şirket içi Active Directory'den varolan kullanıcının kaynak nesnesini tanımlamaya çalışır.  
1. Azure Active Directory'nin sağlanan **UserPrincipalName**ile bir nesnesi olup olmadığını denetleyin. Değilse, **hayır**cevap .
2. Varsa, nesnenin eşitleme için hala kapsamda olup olmadığını denetleyin.  
   - DN'yi kullanarak Azure AD bağlayıcı sı'nda arama yapın.
   - Nesne **Bekleyen Ekle** durumunda bulunursa, **No**yanıtını verin. Azure AD Connect nesneyi doğru Azure AD nesnesine bağlayamaz.
   - Nesne bulunamazsa **Evet'i**yanıtlayın.

Bu örneklerde, soru Joe **Jackson** hala şirket içi Active Directory var olup olmadığını belirlemeye çalışır.
Ortak **senaryo**için, her iki kullanıcı **Joe Johnson** ve **Joe Jackson** şirket içi Active Directory mevcuttur. Karantinaya alınmış nesneler iki farklı kullanıcıdır.

![Eşitleme hata ortak senaryosu tanılamak](./media/how-to-connect-health-diagnose-sync-errors/IIdFixCommonCase.png)

Yetim **nesne senaryosu**için, yalnızca tek kullanıcı **Joe Johnson** şirket içi Active Directory'de bulunur:

![Eşitleme hatası yetim nesne *kullanıcı var* senaryo tanı](./media/how-to-connect-health-diagnose-sync-errors/IIdFixOrphanedCase.png)

### <a name="do-both-of-these-accounts-belong-to-the-same-user"></a>Bu hesapların her ikisi de aynı kullanıcıya mı ait?
Bu soru, aynı kullanıcıya ait olup olmadığını görmek için gelen çakışan bir kullanıcıyı ve Azure AD'deki varolan kullanıcı nesnesini denetler.  
1. Çakışan nesne Azure Etkin Dizini ile yeni eşitlenir. Nesnelerin özniteliklerini karşılaştırın:  
   - Görünen Ad
   - Kullanıcı Asıl Adı
   - Nesne Kimliği
2. Azure AD bunları karşılaştıramazsa, Etkin Dizin'in sağlanan **UserPrincipalNames**ile nesneleri olup olmadığını denetleyin. Her ikisini de bulursanız **hayır** cevabı verin.

Aşağıdaki örnekte, iki nesne aynı kullanıcı **Joe Johnson**aittir.

![Eşitleme hatası yetim nesne *aynı kullanıcı* senaryo tanı](./media/how-to-connect-health-diagnose-sync-errors/IIdFixOrphanedCase.png)


## <a name="what-happens-after-the-fix-is-applied-in-the-orphaned-object-scenario"></a>Düzeltme, yetim nesne senaryosunda uygulandıktan sonra ne olur?
Önceki soruların yanıtlarına bağlı olarak, Azure AD'den bir düzeltme olduğunda **Düzeltdüğmesini Uygula** düğmesini görürsünüz. Bu durumda, şirket içi nesne beklenmeyen bir Azure AD nesnesiyle eşitlenir. İki nesne **Kaynak Çapa**kullanılarak eşlenir. **Fix Uygula** değişikliği şu veya benzer adımları uygular:
1. Kaynak **Çapa'yı** Azure AD'deki doğru nesneyle güncelleştirir.
2. Varsa Azure AD'deki çakışan nesneyi siler.

![Düzeltmeden sonra eşitleme hatasını tanılama](./media/how-to-connect-health-diagnose-sync-errors/IIdFixAfterFix.png)

>[!IMPORTANT]
> **Düzeltme Uygula** değişikliği yalnızca yetim nesne servis talepleri için geçerlidir.
>

Önceki adımlardan sonra, kullanıcı varolan bir nesneye bağlantı olan özgün kaynağa erişebilir. Liste görünümündeki **Tanı lama durum** değeri Bekleyen **Eşitleme**güncelleştirmelerini . Eşitleme hatası sonraki eşitlemeden sonra çözülür. Connect Health artık liste görünümünde çözülmüş eşitleme hatasını göstermez.

## <a name="failures-and-error-messages"></a>Hatalar ve hata iletileri
**Çakışan özniteliği olan kullanıcı, Azure Etkin Dizini'nde yumuşak silinir. Yeniden denemeden önce kullanıcının zor silindiğinden emin olun.**  
Düzeltmeyi uygulamadan önce Azure AD'de çakışan özniteliği olan kullanıcı temizlenmelidir. Düzeltmeyi yeniden denemeden önce [kullanıcıyı Azure AD'de kalıcı olarak nasıl sileceye](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-users-restore) göz atın. Kullanıcı da yumuşak silinmiş durumda 30 gün sonra kalıcı olarak silinir. 

**Kiracınızdaki bulut tabanlı kullanıcıya kaynak bağlantı nın güncelleştirilmesi desteklenmez.**  
Azure AD'deki bulut tabanlı kullanıcının kaynak çapası olmamalıdır. Bu durumda kaynak çapanın güncelleştirilmesi desteklenmez. Tesisten manuel düzeltme gereklidir. 

## <a name="faq"></a>SSS
**S.** **Düzeltme Uygula'nın** yürütülmesi başarısız olursa ne olur?  
**A.** Yürütme başarısız olursa, Azure AD Connect bir dışa aktarım hatası çalıştırıyor olabilir. Portal sayfasını yenileyin ve bir sonraki eşitlemeden sonra yeniden deneyin. Varsayılan eşitleme döngüsü 30 dakikadır. 


**S.** **Varolan nesne** silinecek nesne olması gerekiyorsa ne olur?  
**A.** **Varolan nesne** silinirse, işlem **Kaynak Bağlantı**değişikliği içermez. Genellikle, şirket içi Active Directory'den düzeltebilirsiniz. 


**S.** Bir kullanıcının düzeltmeyi uygulamak için ne gibi izinlere ihtiyacı var?  
**A.** **Global Admin**veya RBAC ayarlarından **Katılımcı,** tanılama ve sorun giderme işlemine erişim iznine sahiptir.


**S.** Bu özellik için Azure AD Connect'i yapılandırmam veya Azure AD Connect Sistem aracısını güncelleştirmem gerekiyor mu?  
**A.** Hayır, tanılama işlemi tamamen bulut tabanlı bir özelliktir.


**S.** Varolan nesne yumuşak silinirse, tanılama işlemi nesneyi yeniden etkin hale getirecek mi?  
**A.** Hayır, düzeltme **Kaynak Çapa**dışındaki nesne özniteliklerini güncelleştirmez.
