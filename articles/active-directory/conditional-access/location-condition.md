---
title: Koşullu erişim Azure Active Directory konum koşulu
description: Bir kullanıcının ağ konumuna bağlı olarak, bulut uygulamalarınıza erişimi denetlemek için konum koşulunu nasıl kullanacağınızı öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 06/15/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.custom: contperfq4
ms.openlocfilehash: 091555bac780f3abc7089d6214d0f9a8cfbf98cd
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86518445"
---
# <a name="using-the-location-condition-in-a-conditional-access-policy"></a>Bir koşullu erişim ilkesinde konum koşulunu kullanma 

[Genel bakış makalesinde](overview.md) açıklandığı gibi, koşullu erişim ilkeleri en temel bir if-then deyimlerinin birleştirme sinyallerinde, kararlar almak ve kuruluş ilkelerini zorlamak için kullanılır. Karar verme işlemine dahil edilebilir sinyallerden biri ağ konumudur.

![Kavramsal koşullu sinyal ve zorlaması alma kararı](./media/location-condition/conditional-access-signal-decision-enforcement.png)

Kuruluşlar, aşağıdaki gibi yaygın görevler için bu ağ konumunu kullanabilir: 

- Şirket ağı kapalıyken bir hizmete erişen kullanıcılar için Multi-Factor Authentication gerektirme.
- Belirli ülkelerden veya bölgelerden bir hizmete erişen kullanıcılar için erişimi engelleme.

Ağ konumu, istemcinin Azure Active Directory için sağladığı genel IP adresi tarafından belirlenir. Koşullu erişim ilkeleri, varsayılan olarak tüm IPv4 ve IPv6 adresleri için geçerlidir. 

> [!TIP]
> IPv6 aralıkları yalnızca **[adlandırılmış konum (Önizleme)](#preview-features)** arabiriminde desteklenir. 

## <a name="named-locations"></a>Adlandırılmış konumlar

Konumlar, **Azure Active Directory**  >  **güvenlik**  >  **koşullu erişim**  >  **adlı konumlarda**Azure Portal belirlenir. Bu adlandırılmış ağ konumları, bir kuruluşun Yönetim Merkezi ağ aralıkları, VPN ağ aralıkları veya engellemek istediğiniz aralıklar gibi konumlar içerebilir. 

![Azure portal adlandırılmış konumlar](./media/location-condition/new-named-location.png)

Bir konum yapılandırmak için en az bir **ad** ve IP aralığı sağlamanız gerekir. 

Yapılandırabileceğiniz adlandırılmış konumların sayısı, Azure AD 'de ilgili nesnenin boyutuyla sınırlıdır. Aşağıdaki sınırlamalara göre konumları yapılandırabilirsiniz:

- En fazla 1200 IPv4 aralığına sahip bir adlandırılmış konum.
- Her birine bir IP aralığı atanmış en fazla 90 adlandırılmış konum.

> [!TIP]
> IPv6 aralıkları yalnızca **[adlandırılmış konum (Önizleme)](#preview-features)** arabiriminde desteklenir. 

### <a name="trusted-locations"></a>Güvenilen konumlar

Bir ağ konumu oluştururken, yöneticinin bir konumu güvenilir bir konum olarak işaretleme seçeneği vardır. 

![Azure portal güvenilen konumlar](./media/location-condition/new-trusted-location.png)

Bu seçenek, örneğin, güvenilir bir ağ konumundan Multi-Factor Authentication için kayıt gerekli kılmak üzere koşullu erişim ilkelerini etkileyebilir. Ayrıca, güvenilir olarak işaretlenmiş bir konumdan geldiği sırada kullanıcıların oturum açma riskini azaltmak için Azure AD Kimlik Koruması risk hesaplamasına da yönelik olarak da etken vardır.

### <a name="countries-and-regions"></a>Ülkeler ve bölgeler

Bazı kuruluşlar, tüm ülkelerin veya bölgelerin IP sınırlarının koşullu erişim ilkeleri için adlandırılmış konumlar olarak tanımlanması tercih edebilir. Bu konumlar, geçerli kullanıcıların, Kuzey Kore gibi bir konumdan hiçbir zaman gelmeyeceğinden gereksiz trafiği engellediği durumlarda bu konumları kullanabilirler. IP adresinin ülkeye olan bu eşlemeleri düzenli aralıklarla güncelleştirilir. 

> [!NOTE]
> Ülkeler IPv6 adres aralıklarını, yalnızca bilinen IPv4 adresi aralıklarını içermez ve güvenilir olarak işaretlenemez.

![Azure portal yeni bir ülke veya bölge tabanlı konum oluşturma](./media/location-condition/new-named-location-country-region.png)

#### <a name="include-unknown-areas"></a>Bilinmeyen alan ekle

Bazı IP adresleri belirli bir ülkeye veya bölgeye eşlenmedi. Bu IP konumlarını yakalamak için, bir konum tanımlarken **bilinmeyen alanı dahil et** kutusunu işaretleyin. Bu seçenek, bu IP adreslerinin adlandırılmış konuma dahil edilip edilmeyeceğini seçmenizi sağlar. Adlandırılmış konumu kullanan ilke bilinmeyen konumlara uygulanacaksa bu ayarı kullanın.

### <a name="configure-mfa-trusted-ips"></a>MFA güvenilir IP 'lerini yapılandırma

Ayrıca, [Multi-Factor Authentication hizmeti ayarlarındaki](https://account.activedirectory.windowsazure.com/usermanagement/mfasettings.aspx)kuruluşunuzun yerel intranetinizi temsıl eden IP adresi aralıklarını yapılandırabilirsiniz. Bu özellik, en fazla 50 IP adresi aralığı yapılandırmanızı sağlar. IP adresi aralıkları CıDR biçimindedir. Daha fazla bilgi için bkz. [Güvenilen IP 'ler](../authentication/howto-mfa-mfasettings.md#trusted-ips).  

Güvenilen IP 'Ler yapılandırdıysanız, konum koşulunun Konum listesinde **MFA güvenilir IP 'leri** olarak gösterilir.

### <a name="skipping-multi-factor-authentication"></a>Multi-Factor Authentication atlanıyor

Multi-Factor Authentication hizmet ayarları sayfasında, intranetteki şirket içi **kullanıcılardan gelen istekler için Multi-Factor Authentication 'ı atla**' yı seçerek kurumsal intranet kullanıcılarını belirleyebilirsiniz. Bu ayar, AD FS tarafından verilen şirket içi ağ talebinin güvenilen ve Kullanıcı tarafından şirket ağı üzerinde olduğunu belirlemek için kullanılması gerektiğini belirtir. Daha fazla bilgi için bkz. [koşullu erişim kullanarak güvenilir IP 'ler özelliğini etkinleştirme](../authentication/howto-mfa-mfasettings.md#enable-the-trusted-ips-feature-by-using-conditional-access).

Bu seçeneği denetledikten sonra, **MFA güvenilir IP 'leri** de dahil olmak üzere bu seçenek belirlenmiş olan her ilke için geçerlidir.

Uzun süreli oturum yaşam süreleri olan mobil ve masaüstü uygulamalarında, koşullu erişim düzenli aralıklarla yeniden değerlendirilecektir. Varsayılan değer bir saattir. Şirket içi ağ talebi yalnızca ilk kimlik doğrulama sırasında verildiğinde, Azure AD 'nin güvenilir IP aralıkları listesi olmayabilir. Bu durumda, kullanıcının hala şirket ağında olup olmadığını belirlemek daha zordur:

1. Kullanıcının IP adresinin güvenilir IP aralıklarından birinde olup olmadığını denetleyin.
1. Kullanıcının IP adresinin ilk üç sekizlinin ilk kimlik doğrulamanın IP adresinin ilk üç sekizlisi ile eşleşip eşleşmediğini denetleyin. Şirket içi ağ talebi ilk yayımlandığında ve Kullanıcı konumu doğrulandığında, IP adresi ilk kimlik doğrulamasıyla karşılaştırılır.

Her iki adım da başarısız olursa, kullanıcının artık güvenilen bir IP 'de olmaması kabul edilir.

## <a name="preview-features"></a>Önizleme özellikleri

Genel olarak kullanılabilir adlandırılmış konum özelliğine ek olarak, adlandırılmış bir konum (Önizleme) de vardır. Adlandırılmış Konum önizlemesine, geçerli adlı konum dikey penceresinin en üstündeki başlığı kullanarak erişebilirsiniz.

![Adlandırılmış konumlar önizlemesini deneyin](./media/location-condition/preview-features.png)

Adlandırılmış Konum önizlemesi sayesinde,

- En fazla 195 adlandırılmış konum yapılandırın
- Adlandırılmış Konum başına en fazla 2000 IP aralığı yapılandırın
- IPv6 adreslerini IPv4 adresleriyle birlikte yapılandırma

Yanlış yapılandırmanın değiştirilmesini azaltmaya yardımcı olmak için bazı ek denetimler de ekledik.

- Özel IP aralıkları artık yapılandırılamaz
- Bir aralığa dahil edilebilir IP adresi sayısı sınırlıdır. Bir IP aralığı yapılandırılırken yalnızca/8 ' den büyük CıDR maskelerine izin verilir.

Önizleme ile, artık iki oluşturma seçeneği vardır: 

- **Ülke konumu**
- **IP aralıkları konumu**

> [!NOTE]
> Ülkeler IPv6 adres aralıklarını, yalnızca bilinen IPv4 adresi aralıklarını içermez ve güvenilir olarak işaretlenemez.

![Adlandırılmış konumlar önizleme arabirimi](./media/location-condition/named-location-preview.png)

## <a name="location-condition-in-policy"></a>İlkede konum koşulu

Konum koşulunu yapılandırırken şunları ayırt etme seçeneğiniz vardır:

- Herhangi bir konum
- Tüm güvenilen konumlar
- Seçili konumlar

### <a name="any-location"></a>Herhangi bir konum

Varsayılan olarak, **herhangi bir konumun** seçilmesi bir ILKENIN tüm IP adreslerine uygulanmasına neden olur, bu da Internet 'teki herhangi bir adres anlamına gelir. Bu ayar, adlandırılmış konum olarak yapılandırdığınız IP adresleriyle sınırlı değildir. **Herhangi bir konum**seçtiğinizde, hala belirli konumları bir ilkeden dışlayabilirsiniz. Örneğin, şirket ağı dışında, kapsamı tüm konumlara ayarlamak için güvenilen konumlar hariç tüm konumlara bir ilke uygulayabilirsiniz.

### <a name="all-trusted-locations"></a>Tüm güvenilen konumlar

Bu seçenek şu şekilde geçerlidir:

- Güvenilen konum olarak işaretlenen tüm konumlar
- **MFA güvenilir IP 'leri** (yapılandırıldıysa)

### <a name="selected-locations"></a>Seçili konumlar

Bu seçenekle, bir veya daha fazla adlandırılmış konum seçebilirsiniz. Bu ayarın uygulanacağı bir ilke için, bir kullanıcının seçili konumlardan herhangi birinden bağlanması gerekir. ' A tıkladığınızda, adlandırılmış ağların listesini gösteren adlandırılmış ağ seçimi denetimini **Seç** ' e tıklayın. Liste ayrıca ağ konumunun güvenilir olarak işaretlenip işaretlenmediğini gösterir. **MFA güvenilir IP 'ler** adlı adlandırılmış konum, Multi-Factor Authentication hizmet ayarı sayfasında yapılandırılabilecek IP ayarlarını eklemek için kullanılır.

## <a name="ipv6-traffic"></a>IPv6 trafiği

Varsayılan olarak, koşullu erişim ilkeleri tüm IPv6 trafiği için geçerlidir. [Adlandırılmış konum önizlemesiyle](#preview-features), belirli IPv6 adres aralıklarını bir koşullu erişim ilkesinden dışlayabilirsiniz. Bu seçenek, ilkenin belirli IPv6 aralıkları için uygulanmasını istemediğiniz durumlarda faydalıdır. Örneğin, kurumsal ağınızda kullanım için bir ilkeyi zorunlu kılmak istiyorsanız ve şirket ağınız genel IPv6 aralıklarında barındırılır.  

### <a name="when-will-my-tenant-have-ipv6-traffic"></a>Kiracımın IPv6 trafiği var mı?

Azure Active Directory (Azure AD) Şu anda IPv6 kullanan doğrudan ağ bağlantılarını desteklememektedir. Ancak, kimlik doğrulama trafiğinin başka bir hizmet aracılığıyla proxy olduğu bazı durumlar vardır. Bu durumlarda, IPv6 adresi ilke değerlendirmesi sırasında kullanılacaktır.

Azure AD 'ye kullanan IPv6 trafiğinin çoğu Microsoft Exchange Online 'dan gelir. Kullanılabilir olduğunda, Exchange IPv6 bağlantılarını tercih edecektir. **Bu nedenle, belirli IPv4 aralıkları için yapılandırılmış Exchange için koşullu erişim ilkelerinize sahipseniz, kuruluşların IPv6 aralıklarını de eklediğinizden emin olmak isteyeceksiniz.** IPv6 aralıklarının dahil edilmesi, aşağıdaki iki durum için beklenmeyen davranışlara neden olur:

- Exchange Online 'a eski kimlik doğrulamasıyla bağlanmak için bir posta istemcisi kullanıldığında, Azure AD bir IPv6 adresi alabilir. İlk kimlik doğrulama isteği Exchange 'e gider ve daha sonra Azure AD 'ye gönderilir.
- Tarayıcıda Outlook Web Erişimi (OWA) kullanıldığında, tüm koşullu erişim ilkelerinin karşılanmasını düzenli olarak doğrular. Bu denetim, bir kullanıcının izin verilen bir IP adresinden yeni bir konuma taşındığı, kafeteryi aşağı doğru bir yere yakalayabildiği durumlarda kullanılır. Bu durumda, bir IPv6 adresi kullanılıyorsa ve IPv6 adresi yapılandırılmış bir aralıkta değilse, Kullanıcı oturumunun kesintiye uğratılmasını ve yeniden kimlik doğrulaması için Azure AD 'ye yönlendirilmesini sağlayabilir. 

Bunlar, adlandırılmış konumlarınızın IPv6 aralıklarını yapılandırmanız gerekebilecek en yaygın nedenlerdir. Ayrıca, Azure sanal ağları 'nı kullanıyorsanız bir IPv6 adresinden gelen trafiğe sahip olursunuz. Bir koşullu erişim ilkesi tarafından engellenen VNet trafiği varsa, Azure AD oturum açma günlüğlerinizi kontrol edin. Trafiği tanımladıktan sonra, kullanılan IPv6 adresini alabilir ve ilkenize dışlayabilirsiniz. 

> [!NOTE]
> Tek bir adres için bir IP CıDR aralığı belirtmek istiyorsanız/32 bit maskesini uygulayın. 2607: fb90: b27a: 6f69: f8d5: dea0: fb39:74A IPv6 adresini söylerseniz ve bu tek adresi bir Aralık olarak dışlamak istediğinizde, 2607: fb90: b27a: 6f69: f8d5: dea0: fb39:74A/32 kullanabilirsiniz.

### <a name="identifying-ipv6-traffic-in-the-azure-ad-sign-in-activity-reports"></a>Azure AD oturum açma etkinlik raporlarında IPv6 trafiğini tanımlama

[Azure AD oturum açma etkinlik raporları](../reports-monitoring/concept-sign-ins.md)' na giderek Kiracınızdaki IPv6 trafiğini bulabilirsiniz. Etkinlik raporunu açtıktan sonra, "IP adresi" sütununu ekleyin. Bu sütun, IPv6 trafiğini tanımlamanızı sağlayacak.

Ayrıca, rapordaki bir satıra tıklayarak ve ardından oturum açma etkinliği ayrıntılarından "konum" sekmesine giderek istemci IP 'sini bulabilirsiniz. 

## <a name="what-you-should-know"></a>Bilmeniz gerekenler

### <a name="when-is-a-location-evaluated"></a>Bir konum ne zaman değerlendirilir?

Koşullu erişim ilkeleri şu durumlarda değerlendirilir:

- Kullanıcı başlangıçta bir Web uygulaması, mobil veya masaüstü uygulamasında oturum açar.
- Modern kimlik doğrulaması kullanan bir mobil veya masaüstü uygulaması, yeni bir erişim belirteci almak için yenileme belirteci kullanır. Varsayılan olarak, bu denetim bir saattir.

Bu denetim, modern kimlik doğrulaması kullanan mobil ve Masaüstü uygulamaları için, ağ konumunun değiştirilmesi için bir saat içinde konum değişikliği tespit edilebilir. Modern kimlik doğrulaması kullanmayan mobil ve Masaüstü uygulamaları için her bir belirteç isteğine ilke uygulanır. İsteğin sıklığı uygulamaya göre farklılık gösterebilir. Benzer şekilde, Web uygulamaları için ilke ilk oturum açma sırasında uygulanır ve Web uygulamasındaki oturumun kullanım ömrü boyunca iyidir. Uygulamalar arasındaki oturum yaşam sürelerinin farklılığı nedeniyle, ilke değerlendirmesi arasındaki süre de farklılık gösterecektir. Uygulama yeni bir oturum açma belirteci istediğinde, ilke uygulanır.

Varsayılan olarak, Azure AD, saatlik olarak bir belirteç yayınlar. Şirket ağını kapattıktan sonra, bir saat içinde ilke modern kimlik doğrulaması kullanan uygulamalar için zorlanır.

### <a name="user-ip-address"></a>Kullanıcı IP adresi

İlke değerlendirmesinde kullanılan IP adresi, kullanıcının genel IP adresidir. Özel bir ağdaki cihazlar için, bu IP adresi intranetteki Kullanıcı cihazının istemci IP 'si değildir, ağ tarafından genel İnternet 'e bağlanmak için kullanılan adrestir.

### <a name="bulk-uploading-and-downloading-of-named-locations"></a>Adlandırılmış konumların toplu karşıya yüklenmesi ve indirilmesi

Adlandırılmış konumları oluştururken veya güncelleştirdiğinizde, toplu güncelleştirmeler için IP aralıklarıyla bir CSV dosyasını karşıya yükleyebilir veya indirebilirsiniz. Karşıya yükleme, listedeki IP aralıklarını, dosyadaki aralıklar ile değiştirir. Dosyanın her satırı CıDR biçiminde bir IP adresi aralığı içerir.

### <a name="cloud-proxies-and-vpns"></a>Bulut proxy 'leri ve VPN 'Ler

Bir bulut barındırılan proxy veya VPN çözümü kullandığınızda, bir ilkeyi değerlendirirken Azure AD 'nin kullandığı IP adresi, proxy 'nin IP adresidir. Kullanıcının genel IP adresini içeren X-Iletilmiş-for (XFF) üst bilgisi kullanılmıyor çünkü güvenilen bir kaynaktan geldiğini belirten bir doğrulama olmadığından, bir IP adresi için bir yöntem sunar.

Bir bulut proxy 'si olduğunda, etki alanına katılmış bir cihaz istemek için kullanılan bir ilke veya AD FS ' den gelen Corpnet talebi kullanılabilir.

### <a name="api-support-and-powershell"></a>API desteği ve PowerShell

Adlandırılmış konumlar için Graph API önizleme sürümü kullanılabilir. daha fazla bilgi için, [Namedlocation API 'sine](/graph/api/resources/namedlocation?view=graph-rest-beta)bakın.

## <a name="next-steps"></a>Sonraki adımlar

- Koşullu erişim ilkesini nasıl yapılandıracağınızı öğrenmek isterseniz, [koşullu erişim Ilkesi oluşturma](concept-conditional-access-policies.md)makalesine bakın.
- Konum koşulunu kullanarak örnek bir ilke mi arıyorsunuz? [Koşullu erişim: konuma göre erişimi engelleme](howto-conditional-access-policy-location.md) makalesine bakın
