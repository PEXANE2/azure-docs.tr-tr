---
title: Azure AD UserPrincipalName popülasyonu
description: Aşağıdaki belgede, UserPrincipalName özniteliğinin nasıl doldurulduğu açıklanmaktadır.
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
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "81680033"
---
# <a name="azure-ad-userprincipalname-population"></a>Azure AD UserPrincipalName popülasyonu

Bu makalede, UserPrincipalName özniteliğinin Azure Active Directory (Azure AD) içinde nasıl doldurulduğu açıklanmaktadır.
UserPrincipalName özniteliği değeri, Kullanıcı hesapları için Azure AD kullanıcı adıdır.

## <a name="upn-terminology"></a>UPN terminolojisi
Bu makalede aşağıdaki terminoloji kullanılmıştır:

|Terim|Açıklama|
|-----|-----|
|İlk etki alanı|Azure AD kiracısındaki varsayılan etki alanı (onmicrosoft.com). Örneğin, contoso.onmicrosoft.com.|
|Microsoft çevrimiçi e-posta yönlendirme adresi (MOERA)|Azure AD, Azure AD Mailtakma ad özniteliği ve Azure AD başlangıç etki alanındaki ilk etki alanı &lt; &gt;&#64;&lt; ilk etkı alanı olan MOERA 'ı hesaplar &gt; .|
|Şirket içi Mailrumuz özniteliği|Bir Exchange kuruluşunda bir kullanıcının diğer adını temsil eden, Active Directory bir özniteliği.|
|Şirket içi posta özniteliği|Active Directory bir özniteliği, bir kullanıcının e-posta adresini temsil eden değeridir|
|Birincil SMTP Adresi|Bir Exchange alıcı nesnesinin birincil e-posta adresi. Örneğin, SMTP: user \@ contoso.com.|
|Alternatif oturum açma KIMLIĞI|Oturum açma için kullanılan posta özniteliği gibi UserPrincipalName dışında bir şirket içi özniteliği.|

## <a name="what-is-userprincipalname"></a>UserPrincipalName nedir?
UserPrincipalName, Internet standart [RFC 822](https://www.ietf.org/rfc/rfc0822.txt)' i temel alan bir kullanıcı için Internet stili bir oturum açma adı olan bir özniteliktir. 

### <a name="upn-format"></a>UPN biçimi
UPN, bir UPN öneki (Kullanıcı hesabı adı) ve bir UPN soneki (bir DNS etki alanı adı) içerir. Önek, "" simgesi kullanılarak soneke katılır \@ . Örneğin, "birisi \@ example.com". Bir UPN, bir Dizin ormanı içindeki tüm güvenlik sorumlusu nesneleri arasında benzersiz olmalıdır. 

## <a name="upn-in-azure-ad"></a>Azure AD 'de UPN 
UPN, kullanıcıların oturum açmasını sağlamak için Azure AD tarafından kullanılır.  Kullanıcı tarafından kullanılabilecek UPN, etki alanının doğrulanıp doğrulanmadığına bağlıdır.  Etki alanı doğrulandıysa, bu sonekine sahip bir kullanıcının Azure AD 'de oturum açmasına izin verilir.  

Öznitelik Azure AD Connect tarafından eşitlenir.  Yükleme sırasında doğrulanmış olan ve olmayan etki alanlarını görüntüleyebilirsiniz.

   ![Doğrulanmamış etki alanları](./media/plan-connect-userprincipalname/unverifieddomain.png) 

## <a name="alternate-login-id"></a>Alternatif oturum açma KIMLIĞI
Bazı ortamlarda, son kullanıcılar yalnızca kendi e-posta adreslerini biliyor olabilir ve UPN 'lerine göre değil.  E-posta adresinin kullanımı, kurumsal bir ilke veya şirket içi iş kolu uygulaması bağımlılığı nedeniyle olabilir.

Alternatif oturum açma KIMLIĞI, kullanıcıların, e-posta gibi UPN dışında bir öznitelikle oturum açbilecekleri bir oturum açma deneyimi yapılandırmanıza olanak tanır.

Azure AD ile alternatif oturum açma KIMLIĞINI etkinleştirmek için Azure AD Connect kullanırken başka bir yapılandırma adımı gerekmez. Alternatif KIMLIK doğrudan sihirbazdan yapılandırılabilir. Bölüm eşitleme altındaki kullanıcılarınız için bkz. Azure AD oturum açma yapılandırması. **Kullanıcı asıl adı** açılır bölümünde alternatif oturum açma kimliği için özniteliği seçin.

![Doğrulanmamış etki alanları](./media/plan-connect-userprincipalname/altloginid.png)  

Daha fazla bilgi için bkz. [Alternatif oturum açma kimliğini yapılandırma](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configuring-alternate-login-id) ve [Azure AD oturum açma yapılandırması](how-to-connect-install-custom.md#azure-ad-sign-in-configuration)

## <a name="non-verified-upn-suffix"></a>Doğrulanmamış UPN soneki
Şirket içi UserPrincipalName özniteliği/alternatif oturum açma KIMLIĞI soneki Azure AD kiracısı ile doğrulanmıyorsa, Azure AD UserPrincipalName özniteliği değeri MOERA olarak ayarlanır. Azure AD, Azure AD Mailtakma ad özniteliğinden ve Azure AD başlangıç etki alanındaki &lt; &gt; &lt; ilk etki alanı&#64;Ilk etkı alanındaki MOERA 'ı hesaplar &gt; .

## <a name="verified-upn-suffix"></a>Doğrulanan UPN soneki
Şirket içi UserPrincipalName özniteliği/alternatif oturum açma KIMLIĞI soneki Azure AD kiracısı ile doğrulanırsa, Azure AD UserPrincipalName özniteliği değeri şirket içi UserPrincipalName özniteliği/alternatif oturum açma KIMLIĞI değeriyle aynı olacaktır.

## <a name="azure-ad-mailnickname-attribute-value-calculation"></a>Azure AD Mailtakma ad öznitelik değeri hesaplama
Azure AD UserPrincipalName özniteliği değeri MOERA olarak ayarlanabileceğinden, MOERA ön eki olan Azure AD Mailrumuz özniteliği değerinin nasıl hesaplanacağını anlamak önemlidir.

Bir kullanıcı nesnesi bir Azure AD kiracısına ilk kez eşitlendiğinde, Azure AD belirtilen sırada aşağıdaki öğeleri denetler ve Mailrumuz öznitelik değerini ilk var olan bir değere ayarlar:

- Şirket içi Mailrumuz özniteliği
- Birincil SMTP adresinin öneki
- Şirket içi posta özniteliğinin öneki
- Şirket içi userPrincipalName özniteliği/alternatif oturum açma KIMLIĞI öneki
- İkincil SMTP adresinin öneki

Bir kullanıcı nesnesindeki güncelleştirmeler Azure AD kiracısıyla eşitlendiğinde Azure AD, yalnızca şirket içi Mailrumuz özniteliği değeri için bir güncelleştirme olması durumunda Mailtakma ad öznitelik değerini güncelleştirir.

>[!IMPORTANT]
>Azure AD, yalnızca şirket içi UserPrincipalName özniteliği/alternatif oturum açma KIMLIĞI değeri güncelleştirmesi Azure AD kiracısıyla eşitlendiğinde UserPrincipalName öznitelik değerini yeniden hesaplar. 
>
>Azure AD, UserPrincipalName özniteliğini her yeniden hesapladığında, MODÖNEMI de yeniden hesaplar. 

## <a name="upn-scenarios"></a>UPN senaryoları
Aşağıda, UPN 'nin verilen senaryoya göre nasıl hesaplanmasının örnek senaryoları verilmiştir.

### <a name="scenario-1-non-verified-upn-suffix--initial-synchronization"></a>Senaryo 1: doğrulanmamış UPN soneki – ilk eşitleme

![Scenario1](./media/plan-connect-userprincipalname/example1.png)

Şirket Içi Kullanıcı nesnesi:
- Mailtakma ad: &lt; ayarlanmadı&gt;
- proxyAddresses: { SMTP:us1@contoso.com }
- -us2@contoso.com
- userPrincipalNameus3@contoso.com

Kullanıcı nesnesi Azure AD kiracısı ile ilk kez eşitlendi
- Azure AD Mailrumuz özniteliğini birincil SMTP adresi ön eki olarak ayarlayın.
- MOERA 'ı &lt; mailtakma ad &gt;&#64;&lt; ilk etki alanı olarak ayarlayın &gt; .
- Azure AD UserPrincipalName özniteliğini MOERA olarak ayarlayın.

Azure AD Kiracı Kullanıcı nesnesi:
- Mailtakma ad: US1           
- UserPrincipalNameus1@contoso.onmicrosoft.com


### <a name="scenario-2-non-verified-upn-suffix--set-on-premises-mailnickname-attribute"></a>Senaryo 2: doğrulanmamış UPN soneki – şirket içi Mailrumuz özniteliğini ayarlayın

![Scenario2](./media/plan-connect-userprincipalname/example2.png)

Şirket Içi Kullanıcı nesnesi:
- Mailtakma ad: US4
- proxyAddresses: { SMTP:us1@contoso.com }
- -us2@contoso.com
- userPrincipalNameus3@contoso.com

Şirket içi Mailtakma ad özniteliğinde güncelleştirmeyi Azure AD kiracısına eşitler
- Azure AD Mailrumuz özniteliğini şirket içi Mailrumuz özniteliğiyle güncelleştirin.
- Şirket içi userPrincipalName özniteliğinde güncelleştirme olmadığından Azure AD UserPrincipalName özniteliğinde değişiklik yapılmaz.

Azure AD Kiracı Kullanıcı nesnesi:
- Mailtakma ad: US4
- UserPrincipalNameus1@contoso.onmicrosoft.com

### <a name="scenario-3-non-verified-upn-suffix--update-on-premises-userprincipalname-attribute"></a>Senaryo 3: doğrulanmamış UPN soneki – şirket içi userPrincipalName özniteliğini güncelleştirme

![Scenario3](./media/plan-connect-userprincipalname/example3.png)

Şirket Içi Kullanıcı nesnesi:
- Mailtakma ad: US4
- proxyAddresses: { SMTP:us1@contoso.com }
- -us2@contoso.com
- userPrincipalNameus5@contoso.com

Şirket içi userPrincipalName özniteliğinde güncelleştirmeyi Azure AD kiracısına eşitler
- Şirket içi userPrincipalName özniteliğinde güncelleştirme, MOERA ve Azure AD UserPrincipalName özniteliğinin yeniden hesaplanmasını tetikler.
- MOERA 'ı &lt; mailtakma ad &gt;&#64;&lt; ilk etki alanı olarak ayarlayın &gt; .
- Azure AD UserPrincipalName özniteliğini MOERA olarak ayarlayın.

Azure AD Kiracı Kullanıcı nesnesi:
- Mailtakma ad: US4
- UserPrincipalNameus4@contoso.onmicrosoft.com

### <a name="scenario-4-non-verified-upn-suffix--update-primary-smtp-address-and-on-premises-mail-attribute"></a>Senaryo 4: doğrulanmamış UPN soneki – birincil SMTP adresini ve şirket içi posta özniteliğini güncelleştirin

![Scenario4](./media/plan-connect-userprincipalname/example4.png)

Şirket Içi Kullanıcı nesnesi:
- Mailtakma ad: US4
- proxyAddresses: { SMTP:us6@contoso.com }
- -us7@contoso.com
- userPrincipalNameus5@contoso.com

Şirket içi posta özniteliğinde güncelleştirmeyi ve birincil SMTP adresini Azure AD kiracısına eşitler
- Kullanıcı nesnesinin ilk eşitlemeden sonra şirket içi posta özniteliğinde ve birincil SMTP adresinin güncelleştirmeleri Azure AD Mailrumuzu veya UserPrincipalName özniteliğini etkilemez.

Azure AD Kiracı Kullanıcı nesnesi:
- Mailtakma ad: US4
- UserPrincipalNameus4@contoso.onmicrosoft.com

### <a name="scenario-5-verified-upn-suffix--update-on-premises-userprincipalname-attribute-suffix"></a>Senaryo 5: doğrulanan UPN soneki – şirket içi userPrincipalName öznitelik sonekini güncelleştirme

![Scenario5](./media/plan-connect-userprincipalname/example5.png)

Şirket Içi Kullanıcı nesnesi:
- Mailtakma ad: US4
- proxyAddresses: { SMTP:us6@contoso.com }
- -us7@contoso.com
- userPrincipalNameus5@verified.contoso.com

Şirket içi userPrincipalName özniteliğinde güncelleştirmeyi Azure AD kiracısına eşitler
- Şirket içi userPrincipalName özniteliğinde güncelleştirme, Azure AD UserPrincipalName özniteliğinin yeniden hesaplanmasını tetikler.
- UPN soneki Azure AD kiracısı ile doğrulandığından, Azure AD UserPrincipalName özniteliğini şirket içi userPrincipalName özniteliğine ayarlayın.

Azure AD Kiracı Kullanıcı nesnesi:
- Mailtakma ad: US4     
- UserPrincipalNameus5@verified.contoso.com

## <a name="next-steps"></a>Sonraki Adımlar
- [Şirket içi dizinlerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)
- [Azure AD Connect özel yüklemesi](how-to-connect-install-custom.md)
