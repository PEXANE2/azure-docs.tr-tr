---
title: Azure AD uygulaması SAML belirteç taleplerini özelleştirme
titleSuffix: Microsoft identity platform
description: Azure AD'deki kurumsal uygulamalar için SAML belirtecinde verilen talepleri nasıl özelleştirebilirsiniz öğrenin.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: article
ms.date: 10/22/2019
ms.author: ryanwi
ms.reviewer: luleon, paulgarn, jeedes
ms.custom: aaddev
ms.openlocfilehash: 87a9632ec2433b8698e3ae3761ba733aa6bc63a5
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80885693"
---
# <a name="how-to-customize-claims-issued-in-the-saml-token-for-enterprise-applications"></a>Nasıl: kurumsal uygulamalar için SAML belirteci verilen iddiaları özelleştirmek

Bugün, Azure Etkin Dizin (Azure AD), Azure AD uygulama galerisinde önceden tümleştirilmiş uygulamaların yanı sıra özel uygulamalar da dahil olmak üzere çoğu kurumsal uygulamayla tek oturum açma (SSO) destekler. Bir kullanıcı SAML 2.0 protokolünü kullanarak Azure AD aracılığıyla bir uygulamaya kimlik doğruladığında, Azure AD uygulamaya bir belirteç gönderir (http post aracılığıyla). Ve sonra, uygulama doğrular ve kullanıcı adı ve parola için soran yerine kullanıcı oturum açmak için belirteci kullanır. Bu SAML belirteçleri, kullanıcı hakkında *talep*olarak bilinen bilgi parçalarını içerir.

*Talep,* kimlik sağlayıcısının, sözkonusu kullanıcı için verdiği belirteç içindeki bir kullanıcı hakkında belirttiği bilgilerdir. [SAML belirteci,](https://en.wikipedia.org/wiki/SAML_2.0)bu veriler genellikle SAML Öznitelik Bildirimi bulunur. Kullanıcının benzersiz kimliği genellikle AD Tanımlayıcı olarak da adlandırılan SAML Konusu'nda temsil edilir.

Varsayılan olarak, Azure AD, Azure AD'de kullanıcının kullanıcı adı (kullanıcı adı adı olarak da bilinir) değerine sahip bir `NameIdentifier` talep içeren ve kullanıcıyı benzersiz olarak tanımlayabilen bir SAML belirteci uygulamanız için sorun. SAML belirteci, kullanıcının e-posta adresini, adını ve soyadını içeren ek talepler de içerir.

SAML'de verilen başvuruları uygulamayla görüntülemek veya görüntülemek için uygulamayı Azure portalında açın. Ardından **Kullanıcı Öznitelikleri & Talepler** bölümünü açın.

![Azure portalında Kullanıcı Öznitelikleri & Talepler bölümünü açma](./media/active-directory-saml-claims-customization/sso-saml-user-attributes-claims.png)

SAML belirtecinde verilen talepleri düzenlemeniz in iki olası nedeni vardır:

* Uygulama, Veya `NameIdentifier` NameID talebinin Azure AD'de depolanan kullanıcı adı (veya kullanıcı adı) dışında bir şey olmasını gerektirir.
* Uygulama, farklı bir talep URI'leri veya talep değerleri kümesi ni gerektirecek şekilde yazılmıştır.

## <a name="editing-nameid"></a>NameID düzenleme

NameID'yi (ad tanımlayıcı değeri) yeniden yapmak için:

1. Ad **tanımlayıcı değeri** sayfasını açın.
1. Öznitelik için uygulamak istediğiniz öznitelik veya dönüşüm seçin. İsteğe bağlı olarak, NameID iddiasının sahip olmasını istediğiniz biçimi belirtebilirsiniz.

   ![NameID (ad tanımlayıcı) değerini edin](./media/active-directory-saml-claims-customization/saml-sso-manage-user-claims.png)

### <a name="nameid-format"></a>NameID biçimi

SAML isteği belirli bir biçime sahip NameIDPolicy öğesi içeriyorsa, Azure AD istekteki biçimi onurlandırır.

SAML isteği NameIDPolicy için bir öğe içermiyorsa, Azure AD nameid'i belirttiğiniz biçimle birlikte yayımlar. Hiçbir biçim belirtilmemişse Azure AD, seçilen talep kaynağıyla ilişkili varsayılan kaynak biçimini kullanır.

Ad **tanımlayıcısı biçim** açılır lay'i seç'ten aşağıdaki seçeneklerden birini seçebilirsiniz.

| NameID biçimi | Açıklama |
|---------------|-------------|
| **Varsayılan** | Azure AD varsayılan kaynak biçimini kullanır. |
| **Kalıcı** | Azure AD, Kalıcı'yı NameID biçimi olarak kullanır. |
| **Emailaddress** | Azure AD, NameID biçimi olarak E-posta Adresi'ni kullanır. |
| **Belirtilmemiş** | Azure AD, AdKimliği biçimi olarak Belirtilmemiş'i kullanır. |
| **Windows etki alanı nitelikli adı** | Azure AD, WindowsDomainQualifiedName'i NameID biçimi olarak kullanır. |

Geçici NameID de desteklenir, ancak açılır açılır durumda kullanılamaz ve Azure tarafında yapılandırılamaz. NameIDPolicy özniteliği hakkında daha fazla bilgi edinmek için [Tek Oturum AÇMA SAML protokolüne](single-sign-on-saml-protocol.md)bakın.

### <a name="attributes"></a>Öznitelikler

`NameIdentifier` (veya NameID) talebi için istenen kaynağı seçin. Aşağıdaki seçenekler arasından seçim yapabilirsiniz.

| Adı | Açıklama |
|------|-------------|
| Email | Kullanıcının e-posta adresi |
| Userprincipalname | Kullanıcının kullanıcı ana adı (UPN) |
| onpremisessamaccount | Şirket içi Azure AD'den senkronize edilen SAM hesap adı |
| Objectıd | Azure AD'de kullanıcının Objectid'i |
| Employeeıd | Kullanıcının çalışan kimliği |
| Dizin genişletmeleri | Azure AD [Connect Eşitlemeyi kullanarak şirket içi Active Directory'den senkronize edilen](../hybrid/how-to-connect-sync-feature-directory-extensions.md) dizin uzantıları |
| Uzantı Öznitelikleri 1-15 | Azure AD şemasını genişletmek için kullanılan şirket içi uzantı öznitelikleri |

Daha fazla bilgi için tablo [3: Kaynak başına geçerli kimlik değerleri.](active-directory-claims-mapping.md#table-3-valid-id-values-per-source)

Azure AD'de tanımladığınız tüm taleplere sabit (statik) herhangi bir değer atayabilirsiniz. Sabit bir değer atamak için lütfen aşağıdaki adımları izleyin:

1. Azure [portalında,](https://portal.azure.com/) **Kullanıcı Öznitelikleri & Talepler** bölümünde, talepleri yeniden oluşturmak için **Edit** simgesine tıklayın.

1. Değiştirmek istediğiniz gerekli talebi tıklatın.

1. Kuruluşuna göre **Kaynak özniteliğine** tırnak işareti olmadan sabit değeri girin ve **Kaydet'i**tıklatın.

    ![Azure portalında Kullanıcı Öznitelikleri & Talepler bölümünü açma](./media/active-directory-saml-claims-customization/organization-attribute.png)

1. Sabit değer aşağıdaki gibi görüntülenir.

    ![Azure portalında Kullanıcı Öznitelikleri & Talepler bölümünü açma](./media/active-directory-saml-claims-customization/edit-attributes-claims.png)

### <a name="special-claims---transformations"></a>Özel talepler - dönüşümler

Talep dönüşümleri işlevlerini de kullanabilirsiniz.

| İşlev | Açıklama |
|----------|-------------|
| **ExtractMailPrefix()** | Etki alanı sonekini e-posta adresinden veya kullanıcı ana adından kaldırır. Bu, geçirilen kullanıcı adının yalnızca ilk bölümünü ayıklar (örneğin, "joe_smith" joe_smith@contoso.comyerine). |
| **Katılın()** | Doğrulanmış bir etki alanıyla bir öznitelik birleştirir. Seçili kullanıcı tanımlayıcı değerinin bir etki alanı varsa, seçilen doğrulanmış etki alanını eklemek için kullanıcı adını ayıklar. Örneğin, kullanıcı tanımlayıcı değerijoe_smith@contoso.comolarak e-postayı ( ) seçerseniz ve doğrulanmış etki alanı joe_smith@contoso.onmicrosoft.comolarak contoso.onmicrosoft.com seçerseniz, bu sonuç . |
| **ToLower()** | Seçili öznitelik karakterlerini küçük karakterlere dönüştürür. |
| **ToUpper()** | Seçili öznitelik karakterlerini büyük harf karakterlerine dönüştürür. |

## <a name="adding-application-specific-claims"></a>Uygulamaya özgü talepler ekleme

Uygulamaya özgü talepler eklemek için:

1. **Kullanıcı Öznitelikleri &** Talepler'de, **Kullanıcı taleplerini yönet** sayfasını açmak için yeni talep **ekle'yi** seçin.
1. Taleplerin **adını** girin. Değer kesinlikle BIR URI desen takip etmek gerekmez, SAML spec başına. Uri deseni gerekiyorsa, **bunu Namespace** alanına koyabilirsiniz.
1. Talebin değerini geri alacağı **Kaynağı** seçin. Kaynak öznitelik açılır bırakarak bir kullanıcı özniteliği seçebilir veya talep olarak yaymadan önce kullanıcı özniteliğine bir dönüşüm uygulayabilirsiniz.

### <a name="claim-transformations"></a>Talep dönüşümleri

Bir kullanıcı özniteliğine dönüşüm uygulamak için:

1. **Claim'i**Yönet'te, **Dönüşüm'i Yönet** sayfasını açmak için talep kaynağı olarak *Dönüşüm''u* seçin.
2. Dönüşüm açılır tarihinden işlevi seçin. Seçilen işleve bağlı olarak, dönüşümde değerlendirmek için parametreler ve sabit bir değer sağlamanız gerekir. Kullanılabilir işlevler hakkında daha fazla bilgi için aşağıdaki tabloya bakın.
3. Birden çok dönüştürme uygulamak için **dönüşüm ekle'ye**tıklayın. Bir hak talebine en fazla iki dönüşüm uygulayabilirsiniz. Örneğin, önce `user.mail`e-posta önekisini ayıklayabilirsiniz. Sonra, dize büyük durumda olun.

   ![NameID (ad tanımlayıcı) değerini edin](./media/active-directory-saml-claims-customization/sso-saml-multiple-claims-transformation.png)

Talepleri dönüştürmek için aşağıdaki işlevleri kullanabilirsiniz.

| İşlev | Açıklama |
|----------|-------------|
| **ExtractMailPrefix()** | Etki alanı sonekini e-posta adresinden veya kullanıcı ana adından kaldırır. Bu, geçirilen kullanıcı adının yalnızca ilk bölümünü ayıklar (örneğin, "joe_smith" joe_smith@contoso.comyerine). |
| **Katılın()** | İki öznitelik birleştirerek yeni bir değer oluşturur. İsteğe bağlı olarak, iki öznitelik arasında bir ayırıcı kullanabilirsiniz. NameID talep dönüştürmesi için, birleştirme doğrulanmış bir etki alanıyla sınırlıdır. Seçili kullanıcı tanımlayıcı değerinin bir etki alanı varsa, seçilen doğrulanmış etki alanını eklemek için kullanıcı adını ayıklar. Örneğin, kullanıcı tanımlayıcı değerijoe_smith@contoso.comolarak e-postayı ( ) seçerseniz ve doğrulanmış etki alanı joe_smith@contoso.onmicrosoft.comolarak contoso.onmicrosoft.com seçerseniz, bu sonuç . |
| **ToLower()** | Seçili öznitelik karakterlerini küçük karakterlere dönüştürür. |
| **ToUpper()** | Seçili öznitelik karakterlerini büyük harf karakterlerine dönüştürür. |
| **Içerir()** | Giriş belirtilen değerle eşleşiyorsa, bir öznitelik veya sabit çıkar. Aksi takdirde, eşleşme yoksa başka bir çıktı belirtebilirsiniz.<br/>Örneğin, değerin kullanıcının e-posta adresi olduğu bir talep tesbicetmek istiyorsanız, ""@contoso.cometki alanını içeriyorsa, aksi takdirde kullanıcı ana adı çıktısını almak istersiniz. Bunu yapmak için aşağıdaki değerleri yapılandırabilirsiniz:<br/>*Parametre 1(giriş)*: user.email<br/>*Değer*:@contoso.com" "<br/>Parametre 2 (çıktı): user.email<br/>Parametre 3 (eşleşme yoksa çıktı): user.userprincipalname |
| **EndWith()** | Giriş belirtilen değerle biterse bir öznitelik veya sabit çıkar. Aksi takdirde, eşleşme yoksa başka bir çıktı belirtebilirsiniz.<br/>Örneğin, çalışan kimliği "000" ile biterse, değerin kullanıcının çalışan kimliği olduğu bir talep tesbit etmek istiyorsanız, aksi takdirde bir uzantı özniteliği çıkarmak istersiniz. Bunu yapmak için aşağıdaki değerleri yapılandırabilirsiniz:<br/>*Parametre 1(giriş)*: user.employeeid<br/>*Değer*: "000"<br/>Parametre 2 (çıktı): user.employeeid<br/>Parametre 3 (eşleşme yoksa çıktı): user.extensionattribute1 |
| **Başlangıç()** | Giriş belirtilen değerle başlarsa bir öznitelik veya sabit çıkar. Aksi takdirde, eşleşme yoksa başka bir çıktı belirtebilirsiniz.<br/>Örneğin, ülke/bölge "ABD" ile başlarsa, değerin kullanıcının çalışan kimliği olduğu bir talep temenni etmek istiyorsanız, aksi takdirde bir uzantı özniteliği çıkarmak istersiniz. Bunu yapmak için aşağıdaki değerleri yapılandırabilirsiniz:<br/>*Parametre 1(giriş)*: user.country<br/>*Değer*: "ABD"<br/>Parametre 2 (çıktı): user.employeeid<br/>Parametre 3 (eşleşme yoksa çıktı): user.extensionattribute1 |
| **Extract() - Eşleştirme den sonra** | Belirtilen değerle eşleştikten sonra alt dizeyi döndürür.<br/>Örneğin, girdinin değeri "Finance_BSimon", eşleşen değer "Finance_" ise, talebin çıktısı "BSimon" olur. |
| **Extract() - Eşleştirmeden önce** | Belirtilen değerle eşleşene kadar alt dizeyi döndürür.<br/>Örneğin, girdinin değeri "BSimon_US", eşleşen değer "_US" ise, talebin çıktısı "BSimon"dur. |
| **Extract() - Eşleştirme arasında** | Belirtilen değerle eşleşene kadar alt dizeyi döndürür.<br/>Örneğin, girdinin değeri "Finance_BSimon_US", ilk eşleşen değer "Finance_", ikinci eşleşen değer "_US", sonra talebin çıktısı "BSimon" ise. |
| **ExtractAlpha() - Önek** | Dize yekponi alfabetik kısmını verir.<br/>Örneğin, girdinin değeri "BSimon_123" ise, "BSimon" döndürür. |
| **ExtractAlpha() - Sonek** | Dize alfabetik sonek kısmını döndürür.<br/>Örneğin, girişin değeri "123_Simon" ise, "Simon" döndürür. |
| **ExtractNumeric() - Önek** | Dize önek sayısal kısmını döndürür.<br/>Örneğin, girişin değeri "123_BSimon" ise, "123" döndürür. |
| **ExtractNumeric() - Sonek** | Dize sayısal bölümünü döndürür.<br/>Örneğin, girişin değeri "BSimon_123" ise, "123" döndürür. |
| **IfEmpty()** | Giriş boş veya boşsa bir öznitelik veya sabit çıkar.<br/>Örneğin, belirli bir kullanıcının çalışan kimliği boşsa, uzantıözde depolanan bir öznitelik çıktısı almak isterseniz. Bunu yapmak için aşağıdaki değerleri yapılandırabilirsiniz:<br/>Parametre 1(giriş): user.employeeid<br/>Parametre 2 (output): user.extensionattribute1<br/>Parametre 3 (eşleşme yoksa çıktı): user.employeeid |
| **IfNotEmpty()** | Giriş boş veya boş değilse, bir öznitelik veya sabit çıkar.<br/>Örneğin, belirli bir kullanıcının çalışan kimliği boş değilse, uzantıözözde depolanan bir öznitelik çıktısı almak isterseniz. Bunu yapmak için aşağıdaki değerleri yapılandırabilirsiniz:<br/>Parametre 1(giriş): user.employeeid<br/>Parametre 2 (output): user.extensionattribute1 |

Ek dönüşümlere ihtiyacınız varsa, fikrinizi *SaaS uygulama* kategorisi altındaki [Azure AD'deki geri bildirim forumunda](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=160599) gönderin.

## <a name="emitting-claims-based-on-conditions"></a>Koşullara göre talep yayma

Kullanıcı türüne ve kullanıcının ait olduğu gruba göre bir talebin kaynağını belirtebilirsiniz. 

Kullanıcı türü şu olabilir:
- **Any**: Tüm kullanıcıların uygulamaya erişmesine izin verilir.
- **Üyeler**: Kiracının yerli üyesi
- **Tüm konuklar**: Kullanıcı, Azure AD'li veya Olmayan harici bir kuruluştan getirilir.
- **AAD konukları**: Konuk kullanıcı Azure AD'yi kullanarak başka bir kuruluşa aittir.
- **Dış konuklar**: Konuk kullanıcı, Azure REKLAMı olmayan harici bir kuruluşa aittir.


Bunun yararlı olduğu senaryolardan biri, bir talebin kaynağının bir konuk ve bir uygulamaya erişen bir çalışan için farklı olmasıdır. Kullanıcı bir çalışansa, NameID'nin user.email'den geldiğini, ancak kullanıcı konuksa NameID'nin user.extensionattribute1'den geldiğini belirtmek isteyebilirsiniz.

Talep koşulu eklemek için:

1. **İddiayı**Yönet'te, Talep koşullarını genişletin.
2. Kullanıcı türünü seçin.
3. Kullanıcının ait olması gereken grubu(lar) seçin. Belirli bir uygulama için tüm talepler arasında en fazla 10 benzersiz grup seçebilirsiniz. 
4. Talebin değerini geri alacağı **Kaynağı** seçin. Kaynak öznitelik açılır bırakarak bir kullanıcı özniteliği seçebilir veya talep olarak yaymadan önce kullanıcı özniteliğine bir dönüşüm uygulayabilirsiniz.

Koşulları ekleme sırası önemlidir. Azure AD, talepte hangi değeri yayışacağına karar vermek için koşulları yukarıdan aşağıya doğru değerlendirir. 

Örneğin, Brita Simon Contoso kiracı bir konuk kullanıcıdır. Azure AD kullanan başka bir kuruluşa ait. Fabrikam uygulaması için aşağıdaki yapılandırma göz önüne alındığında, Brita Fabrikam'da oturum açmaya çalıştığında, Azure AD aşağıdaki koşulları değerlendirecektir.

İlk olarak, Azure AD, Brita'nın `All guests`kullanıcı türü. Bu doğrudur, sonra Azure AD iddia için kaynak `user.extensionattribute1`atar. İkinci olarak, Azure AD, Brita'nın `AAD guests`kullanıcı türü olup olmadığını doğrular, çünkü bu `user.mail`da doğrudur, azure AD talep için kaynak atar. Son olarak, iddia Brita `user.email` için değer ile yayılır.

![Koşullu yapılandırma talepleri](./media/active-directory-saml-claims-customization/sso-saml-user-conditional-claims.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure AD'de uygulama yönetimi](../manage-apps/what-is-application-management.md)
* [Azure AD uygulama galerisinde olmayan uygulamaları tek oturum açma işlemlerini yapılandırma](../manage-apps/configure-federated-single-sign-on-non-gallery-applications.md)
* [Sorun giderme SAML tabanlı tek oturum açma](../azuread-dev/howto-v1-debug-saml-sso-issues.md)
