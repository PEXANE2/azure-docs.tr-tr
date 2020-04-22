---
title: Azure AD UserPrincipalName popülasyonu
description: Aşağıdaki belgede UserPrincipalName özniteliğinin nasıl doldurulur açıklanmaktadır.
author: billmath
ms.subservice: hybrid
ms.author: billmath
ms.date: 06/26/2018
ms.topic: conceptual
ms.workload: identity
ms.service: active-directory
manager: daveba
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6c748df10e432e3bebbce0dc8cb39dd2101d52e2
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81680033"
---
# <a name="azure-ad-userprincipalname-population"></a>Azure AD UserPrincipalName popülasyonu

Bu makalede, UserPrincipalName özniteliğinin Azure Etkin Dizini'nde (Azure AD) nasıl doldurulur açıklanmaktadır.
UserPrincipalName öznitelik değeri, kullanıcı hesapları için Azure AD kullanıcı adıdır.

## <a name="upn-terminology"></a>UPN terminolojisi
Bu makalede aşağıdaki terminoloji kullanılmıştır:

|Sözleşme Dönemi|Açıklama|
|-----|-----|
|İlk etki alanı|Azure AD Kiracı'sında varsayılan etki alanı (onmicrosoft.com). Örneğin, contoso.onmicrosoft.com.|
|Microsoft Online E-posta Yönlendirme Adresi (MOERA)|Azure AD, MOERA'yı Azure AD MailNickName özniteliğinden &lt;ve&gt; Azure &lt;AD&gt;ilk etki alanından MailNickName&#64;ilk etki alanı olarak hesaplar.|
|Şirket içi postaNickName özniteliği|Etkin Dizin'deki bir öznitelik, değeri bir Exchange kuruluşundaki bir kullanıcının diğer adını temsil eder.|
|Şirket içi posta özniteliği|Etkin Dizin'de, değeri bir kullanıcının e-posta adresini temsil eden bir öznitelik|
|Birincil SMTP Adresi|Exchange alıcı nesnesinin birincil e-posta adresi. Örneğin, SMTP:kullanıcı\@contoso.com.|
|Alternatif giriş kimliği|Oturum açma için kullanılan posta özniteliği gibi UserPrincipalName dışındaki şirket içi bir öznitelik.|

## <a name="what-is-userprincipalname"></a>UserPrincipalName nedir?
UserPrincipalName, [RFC 822](https://www.ietf.org/rfc/rfc0822.txt)Internet standardına dayalı bir kullanıcı için Internet tarzı giriş adı olan bir özniteliktir. 

### <a name="upn-format"></a>UPN biçimi
UPN, UPN öneki (kullanıcı hesabı adı) ve UPN sonekinden (DNS etki alanı adı) oluşur. Önek , " "\@sembolü kullanılarak sonek ile birleştirilir. Örneğin, "birisi\@example.com". UPN, dizin ormanı içindeki tüm güvenlik temel nesneleri arasında benzersiz olmalıdır. 

## <a name="upn-in-azure-ad"></a>Azure AD'de UPN 
UPN, Azure AD tarafından kullanıcıların oturum açmalarına izin vermek için kullanılır.  Bir kullanıcının kullanabileceği UPN, etki alanının doğrulanıp doğrulanmadığına bağlıdır.  Etki alanı doğrulandıysa, bu soneke sahip bir kullanıcının Azure AD'de oturum açmasına izin verilir.  

Öznitelik Azure AD Connect tarafından senkronize edilir.  Yükleme sırasında, doğrulanmış ve doğrulanmamış etki alanlarını görüntüleyebilirsiniz.

   ![Doğrulanmamış etki alanları](./media/plan-connect-userprincipalname/unverifieddomain.png) 

## <a name="alternate-login-id"></a>Alternatif giriş kimliği
Bazı ortamlarda, son kullanıcılar UPN'lerinin değil, yalnızca e-posta adreslerinin farkında olabilirler.  E-posta adresinin kullanımı, bir şirket ilkesi nden veya şirket içi uygulama bağımlılığından kaynaklanıyor olabilir.

Alternatif oturum açma kimliği, kullanıcıların posta gibi UPN'lerinden başka bir öznitelikle oturum açabilecekleri bir oturum açma deneyimi yapılandırmanıza olanak tanır.

Azure AD ile Alternatif giriş kimliği etkinleştirmek için Azure AD Connect'i kullanırken ek yapılandırma adımları gerekmez. Alternatif kimlik doğrudan sihirbazdan yapılandırılabilir. Eşitleme bölümünde kullanıcılarınız için Azure AD oturum açma yapılandırması bölümüne bakın. Kullanıcı **Ana Adı** açılır listesi altında, Alternatif giriş kimliği için özniteliği seçin.

![Doğrulanmamış etki alanları](./media/plan-connect-userprincipalname/altloginid.png)  

Daha fazla bilgi için [bkz.](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configuring-alternate-login-id) [Azure AD sign-in configuration](how-to-connect-install-custom.md#azure-ad-sign-in-configuration)

## <a name="non-verified-upn-suffix"></a>Doğrulanmamış UPN Soneki
Şirket içi UserPrincipalName özniteliği/Alternatif oturum açma kimliği soneki Azure AD Kiracı ile doğrulanmazsa, Azure AD UserPrincipalName öznitelik değeri MOERA olarak ayarlanır. Azure AD, MOERA'yı Azure AD MailNickName özniteliğinden &lt;ve Azure&gt; &lt;AD&gt;ilk etki alanından MailNickName&#64;ilk etki alanı olarak hesaplar.

## <a name="verified-upn-suffix"></a>Doğrulanmış UPN soneki
Şirket içi UserPrincipalName özniteliği/Alternatif oturum açma kimliği soneki Azure AD Kiracısı ile doğrulanırsa, Azure AD UserPrincipalName öznitelik değeri şirket içi UserPrincipalName özniteliği/Alternatif oturum açma kimliği değeriyle aynı olacaktır.

## <a name="azure-ad-mailnickname-attribute-value-calculation"></a>Azure AD MailNickName öznitelik değer hesaplama
Azure AD UserPrincipalName öznitelik değeri MOERA olarak ayarlanabileceğinden, MOERA öneki olan Azure AD MailNickName öznitelik değerinin nasıl hesaplandığını anlamak önemlidir.

Bir kullanıcı nesnesi ilk kez bir Azure REKLAM Kiracısı ile eşitlendiğinde, Azure AD verilen sırada aşağıdaki öğeleri denetler ve MailNickName öznitelik değerini varolan ilk öğeye ayarlar:

- Şirket içi postaNickName özniteliği
- Birincil SMTP adresi öneki
- Şirket içi posta özniteliği öneki
- Şirket içi kullanıcı ÖnekiPrincipalName özniteliği/Alternatif giriş kimliği
- İkincil smtp adresi öneki

Bir kullanıcı nesnesi güncelleştirmeleri Azure AD Kiracısı ile senkronize edildiğinde, Azure AD yalnızca şirket içi mailNickName öznitelik değerinde bir güncelleştirme olması durumunda MailNickName öznitelik değerini güncelleştirir.

>[!IMPORTANT]
>Azure AD, Yalnızca şirket içi UserPrincipalName özniteliği/Alternatif oturum açma kimliği değerine yapılan bir güncelleştirmenin Azure AD Kiracısı ile senkronize edilmesi durumunda UserPrincipalName özniteliği değerini yeniden hesaplar. 
>
>Azure AD UserPrincipalName özniteliğini yeniden hesapladığında, MOERA'yı da yeniden hesaplar. 

## <a name="upn-scenarios"></a>UPN senaryoları
Aşağıda, UPN'nin verilen senaryoya göre nasıl hesaplandığına ilişkin örnek senaryolar verilmiştir.

### <a name="scenario-1-non-verified-upn-suffix--initial-synchronization"></a>Senaryo 1: Doğrulanmamış UPN soneki – ilk eşitleme

![Senaryo1](./media/plan-connect-userprincipalname/example1.png)

Şirket içi kullanıcı nesnesi:
- mailNickName &lt;: ayarlanmaz&gt;
- proxyAdres :SMTP:us1@contoso.com{ }
- Posta:us2@contoso.com
- Userprincipalname:us3@contoso.com

Kullanıcı nesnesini ilk kez Azure AD Kiracısı ile senkronize etti
- Azure AD MailNickName özniteliğini birincil SMTP adresi önekine ayarlayın.
- MOERA'yı &lt;&gt; mailNickName &lt;&#64;&gt;ilk etki alanına ayarlayın.
- Azure AD UserPrincipalName özniteliğini MOERA olarak ayarlayın.

Azure AD Kiracı kullanıcı nesnesi:
- MailNickName : us1           
- Userprincipalname:us1@contoso.onmicrosoft.com


### <a name="scenario-2-non-verified-upn-suffix--set-on-premises-mailnickname-attribute"></a>Senaryo 2: Doğrulanmamış UPN soneki – şirket içi postaNickName özniteliğini ayarla

![Senaryo2](./media/plan-connect-userprincipalname/example2.png)

Şirket içi kullanıcı nesnesi:
- mailNickName : us4
- proxyAdres :SMTP:us1@contoso.com{ }
- Posta:us2@contoso.com
- Userprincipalname:us3@contoso.com

Şirket içi postalarda güncelleştirmeyi Azure AD Kiracısına eşitlemeNickName özniteliği
- Azure AD MailNickName özniteliğini şirket içi postaNickName özniteliğiyle güncelleştirin.
- Şirket içi kullanıcıPrincipalName özniteliğinde güncelleştirme olmadığından, Azure AD UserPrincipalName özniteliğinde değişiklik yoktur.

Azure AD Kiracı kullanıcı nesnesi:
- MailNickName : us4
- Userprincipalname:us1@contoso.onmicrosoft.com

### <a name="scenario-3-non-verified-upn-suffix--update-on-premises-userprincipalname-attribute"></a>Senaryo 3: Doğrulanmamış UPN sonek – şirket içi kullanıcıPrincipalName özniteliğini güncelleştir

![Senaryo3](./media/plan-connect-userprincipalname/example3.png)

Şirket içi kullanıcı nesnesi:
- mailNickName : us4
- proxyAdres :SMTP:us1@contoso.com{ }
- Posta:us2@contoso.com
- Userprincipalname:us5@contoso.com

Azure AD Kiracısı'na şirket içi kullanıcıPrincipalName özniteliği güncelleştirmesini senkronize etme
- Şirket içi userPrincipalName özniteliğinin güncelleştirilen güncelleştirmesi, MOERA ve Azure AD UserPrincipalName özniteliğinin yeniden hesaplanmasını tetikler.
- MOERA'yı &lt;&gt; mailNickName &lt;&#64;&gt;ilk etki alanına ayarlayın.
- Azure AD UserPrincipalName özniteliğini MOERA olarak ayarlayın.

Azure AD Kiracı kullanıcı nesnesi:
- MailNickName : us4
- Userprincipalname:us4@contoso.onmicrosoft.com

### <a name="scenario-4-non-verified-upn-suffix--update-primary-smtp-address-and-on-premises-mail-attribute"></a>Senaryo 4: Doğrulanmamış UPN soneki – birincil SMTP adresini ve şirket içi posta özniteliğini güncelleştirme

![Senaryo4](./media/plan-connect-userprincipalname/example4.png)

Şirket içi kullanıcı nesnesi:
- mailNickName : us4
- proxyAdres :SMTP:us6@contoso.com{ }
- Posta:us7@contoso.com
- Userprincipalname:us5@contoso.com

Şirket içi posta özniteliği ve birincil SMTP adresiyle ilgili güncelleştirmeyi Azure AD Kiracısına senkronize etme
- Kullanıcı nesnesinin ilk eşitlenmesinden sonra, şirket içi posta özniteliği ve birincil SMTP adresigüncelleştirmeleri Azure AD MailNickName veya UserPrincipalName özniteliğini etkilemez.

Azure AD Kiracı kullanıcı nesnesi:
- MailNickName : us4
- Userprincipalname:us4@contoso.onmicrosoft.com

### <a name="scenario-5-verified-upn-suffix--update-on-premises-userprincipalname-attribute-suffix"></a>Senaryo 5: Doğrulanmış UPN sonek – güncelleştirme şirket içi userPrincipalName öznitelik soneki

![Senaryo5](./media/plan-connect-userprincipalname/example5.png)

Şirket içi kullanıcı nesnesi:
- mailNickName : us4
- proxyAdres :SMTP:us6@contoso.com{ }
- Posta:us7@contoso.com
- Userprincipalname:us5@verified.contoso.com

Azure AD Kiracısı'na şirket içi kullanıcıPrincipalName özniteliği güncelleştirmesini senkronize etme
- Şirket içi userPrincipalName özniteliğinin güncelleştirilen güncelleştirmesi, Azure AD UserPrincipalName özniteliğinin yeniden hesaplanmasını tetikler.
- UPN sonek Azure AD Kiracıile doğrulanırken Azure AD UserPrincipalName özniteliğini şirket içi kullanıcıPrincipalName özniteliğine ayarlayın.

Azure AD Kiracı kullanıcı nesnesi:
- MailNickName : us4     
- Userprincipalname:us5@verified.contoso.com

## <a name="next-steps"></a>Sonraki Adımlar
- [Şirket içi dizinlerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)
- [Azure AD Connect özel yüklemesi](how-to-connect-install-custom.md)
