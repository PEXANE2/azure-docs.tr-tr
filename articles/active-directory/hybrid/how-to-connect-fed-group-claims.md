---
title: Azure Active Directory ile uygulamalar için grup taleplerini yapılandırma | Microsoft Dokümanlar
description: Azure AD ile kullanılmak üzere grup taleplerinin nasıl yapılandırılabildiğini zedeb.
services: active-directory
documentationcenter: ''
ms.reviewer: paulgarn
manager: daveba
ms.subservice: hybrid
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 02/27/2019
ms.author: billmath
author: billmath
ms.openlocfilehash: b8708aec1137836516852135412c4c7cec2feba4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79408411"
---
# <a name="configure-group-claims-for-applications-with-azure-active-directory-public-preview"></a>Azure Etkin Dizini (Genel Önizleme) ile uygulamalar için grup taleplerini yapılandırma

Azure Active Directory, kullanıcılara uygulamalarda kullanılmak üzere belirteçlerde grup üyelik bilgileri sağlayabilir.  İki ana desen desteklenir:

- Azure Etkin Dizin nesne tanımlayıcı (OID) özniteliği (Genellikle Kullanılabilir) tarafından tanımlanan gruplar
- Active Directory (AD) senkronize gruplar ve kullanıcılar için sAMAccountName veya GroupSID öznitelikleri tarafından tanımlanan gruplar (Genel Önizleme)

> [!IMPORTANT]
> Bu önizleme işlevi için dikkat edilmesi gereken birkaç uyarı vardır:
>
>- Şirket içinde senkronize edilen sAMAccountName ve güvenlik tanımlayıcı (SID) özniteliklerinin kullanımı için destek, varolan uygulamaların AD FS ve diğer kimlik sağlayıcılarından taşınmasını sağlamak üzere tasarlanmıştır. Azure AD'de yönetilen gruplar, bu talepleri yattırmak için gereken öznitelikleri içermez.
>- Daha büyük kuruluşlarda, bir kullanıcının üyesi olduğu grup sayısı, Azure Etkin Dizin'inin belirtemeye ekeceği sınırı aşabilir. BIR SAML belirteci için 150 grup ve JWT için 200. Bu öngörülemeyen sonuçlara yol açabilir. Kullanıcılarınızın çok sayıda grup üyeliği varsa, talepte bulunan grupları uygulama için ilgili gruplara sınırlama seçeneğini kullanmanızı öneririz.  
>- Yeni uygulama geliştirme için veya uygulamanın bunun için yapılandırılabildiği durumlarda ve iç içe grup desteğinin gerekli olmadığı durumlarda, uygulama içi yetkilendirmenin gruplardan çok uygulama rollerine dayanmasını öneririz.  Bu, belirteciiçine gitmek için gereken bilgi miktarını sınırlar, daha güvenli ve uygulama yapılandırması kullanıcı atama ayırır.

## <a name="group-claims-for-applications-migrating-from-ad-fs-and-other-identity-providers"></a>AD FS ve diğer kimlik sağlayıcılardan geçiş yapılan uygulamalar için grup talepleri

AD FS ile kimlik doğrulaması yapmak üzere yapılandırılan birçok uygulama, Windows AD grup öznitelikleri biçiminde grup üyelik bilgilerine dayanır.   Bu öznitelikler, alan adı ile nitelikli olabilecek grup sAMAccountName veya Windows Group Security Identifier (GroupSID) grubudur.  Uygulama AD FS ile federe olduğunda, AD FS kullanıcı için grup üyelikleri almak için TokenGroups işlevini kullanır.

AD FS'den taşınmış bir uygulamanın aynı biçimde taleplere ihtiyacı vardır. Grup ve rol talepleri, grubun Azure Etkin Dizin objectID'si yerine Active Directory'den eşitlenen etki alanı nitelikli sAMAccountName veya GroupSID'yi içeren Azure Active Directory'den yayılabilir.

Grup talepleri için desteklenen biçimler şunlardır:

- **Azure Active Directory Group ObjectId** (Tüm gruplar için kullanılabilir)
- **sAMAccountName** (Active Directory'den senkronize edilen gruplar için kullanılabilir)
- **NetbiosDomain\sAMAccountName** (Active Directory'den senkronize edilmiş gruplar için kullanılabilir)
- **DNSDomainName\sAMAccountName** (Active Directory'den senkronize edilen gruplar için kullanılabilir)
- **On Premises Group Security Tanımlayıcı** (Active Directory'den eşitlenen gruplar için kullanılabilir)

> [!NOTE]
> sAMAccountName ve On Premises Group SID öznitelikleri yalnızca Active Directory'den eşitlenen Grup nesnelerinde kullanılabilir.   Azure Etkin Dizin veya Office365'te oluşturulan gruplarda kullanılamazlar.   Şirket içi grup özniteliklerinin senkronize edilmesi için Azure Active Directory'de yapılandırılan uygulamalar, bunları yalnızca eşitlenmiş gruplar için alır.

## <a name="options-for-applications-to-consume-group-information"></a>Grup bilgilerini tüketen uygulamalar için seçenekler

Uygulamalar, kimliği doğrulanan kullanıcı için grup bilgilerini elde etmek için MS Graph gruplarını uç nokta olarak arayabilir. Bu çağrı, ilgili çok sayıda grup olsa bile, bir kullanıcının üyesi olduğu tüm grupların kullanılabilir olmasını sağlar.  Grup numaralandırma daha sonra belirteç boyutu sınırlamalarından bağımsızdır.

Ancak, varolan bir uygulama talep yoluyla grup bilgilerini tüketmeyi bekliyorsa, Azure Etkin Dizini bir dizi farklı talep biçimiyle yapılandırılabilir.  Aşağıdaki seçenekleri göz önünde bulundurun:

- Grup üyeliğini uygulama içi yetkilendirme amacıyla kullanırken ObjectID grubu'nu kullanmak tercih edilir. Grup ObjectID değişmez ve Azure Etkin Dizini'nde benzersizdir ve tüm gruplar için kullanılabilir.
- Yetkilendirme için şirket içi sAMAccountName grubunu kullanıyorsanız, alan nitelikli adlarını kullanın;  İsimlerin çakışma ihtimali daha az. sAMAccountName Bir Active Directory etki alanında benzersiz olabilir, ancak birden fazla Active Directory etki alanı bir Azure Active Directory kiracıile eşitlenirse, birden fazla grubun aynı ada sahip olma olasılığı vardır.
- Grup üyeliği ve uygulama arasında bir yönlendirme katmanı sağlamak için [Uygulama Rolleri'ni](../../active-directory/develop/howto-add-app-roles-in-azure-ad-apps.md) kullanmayı düşünün.   Uygulama daha sonra belirteçteki rol istiridyelerine dayalı iç yetkilendirme kararları verir.
- Uygulama, Etkin Dizin'den eşitlenen grup özniteliklerini almak için yapılandırılırsa ve bir Grup bu öznitelikleri içermiyorsa, taleplere dahil edilmez.
- Grup taleplerini belirteçlerde, grup taleplerini uygulamaya atanan gruplarla sınırlama seçeneğini kullanmadışında iç içe olan grupları içerir.  Bir kullanıcı GroupB üyesiyse ve GroupB GroupA üyesiyse, kullanıcı için grup talepleri hem GroupA hem de GroupB içerir. Bir kuruluşun kullanıcıları çok sayıda grup üyeliğine sahipse, belirteçte listelenen grup sayısı belirteç boyutunu büyütebilir.  Azure Etkin Dizin, bir belirteç halinde yayacakları grup sayısını SAML iddiaları için 150 ve JWT için 200 ile sınırlar.  Bir kullanıcı daha fazla sayıda grubun üyesiyse, gruplar atlanır ve grup bilgilerini elde etmek için Grafik bitiş noktasına bir bağlantı dahil edilir.

## <a name="prerequisites-for-using-group-attributes-synchronized-from-active-directory"></a>Active Directory'den senkronize edilen Grup özniteliklerini kullanmak için ön koşullar

ObjectId biçimini kullanırsanız, grup üyelik talepleri herhangi bir grup için belirteçler halinde yayılabilir. Grup taleplerini ObjectId grubu dışındaki biçimlerde kullanmak için, grupların Azure AD Connect kullanarak Active Directory'den eşitlenmiş olması gerekir.

Azure Etkin Dizini'ni, Etkin Dizin Grupları için grup adları yatmak üzere yapılandırmak için iki adım vardır.

1. **Active Directory'den grup adlarını eşitle** Azure Etkin Dizin grup adlarını veya grup veya rol iddialarında sid grubu veya şirket içi grup yatsa, gerekli özniteliklerin Active Directory'den eşitlenmiş olması gerekir.  Azure AD Connect sürümünü 1.2.70 veya sonraki sürümde çalışıyor olmalısınız.   Azure AD Connect'in 1,2.70'ten önceki sürümleri Active Directory'deki grup nesnelerini eşitler, ancak gerekli grup adı özniteliklerini içermez.  Geçerli sürüme yükseltin.

2. **Azure Etkin Dizini'ndeki uygulama kaydını, grup taleplerini belirteçlere dahil etmek üzere yapılandırın** Grup talepleri portalın Kurumsal Uygulamalar bölümünde veya Uygulama Kayıtları bölümündeki Uygulama Bildirimi kullanılarak yapılandırılabilir.  Uygulama bildiriminde grup taleplerini yapılandırmak için aşağıdaki "Grup öznitelikleri için Azure Active Directory Application Registration yapılandırma" adlı yapıya bakın.

## <a name="add-group-claims-to-tokens-for-saml-applications-using-sso-configuration"></a>SSO yapılandırmasını kullanarak SAML uygulamaları için grup taleplerine grup talepleri ekleme

Galeri veya Galeri Olmayan SAML uygulaması için Grup Taleplerini yapılandırmak için, **Kurumsal Uygulamaları**açın, listedeki uygulamayı tıklatın, **Yapılandırmada Tek Oturum Açma'yı**seçin ve ardından Kullanıcı **Öznitelikleri & Talepleri'ni**seçin.

Grup **talebi ekle'ye** tıklayın  

![iddia UI](media/how-to-connect-fed-group-claims/group-claims-ui-1.png)

Belirteçte hangi grupların yer alması gerektiğini seçmek için radyo düğmelerini kullanın

![iddia UI](media/how-to-connect-fed-group-claims/group-claims-ui-2.png)

| Seçim | Açıklama |
|----------|-------------|
| **Tüm gruplar** | Güvenlik grupları ve dağıtım listeleri ve rolleri yayır.  |
| **Güvenlik grupları** | Kullanıcının üyesi olduğu güvenlik gruplarını gruplara yayır iddia |
| **Dizin rolleri** | Kullanıcıya dizin rolleri atanmışsa, bunlar 'wids' iddiası olarak yayılır (grup iddiası yayılamaz) |
| **Uygulamaya atanan gruplar** | Yalnızca uygulamaya açıkça atanan grupları yayır ve kullanıcı |

Örneğin, kullanıcının üyesi olduğu tüm Güvenlik Gruplarını yalamak için Güvenlik Grupları'nı seçin

![iddia UI](media/how-to-connect-fed-group-claims/group-claims-ui-3.png)

Azure AD objectID'leri yerine Active Directory'den eşitlenen Active Directory özniteliklerini kullanan gruplar oluşturmak için açılır yerden gerekli biçimi seçin. Taleplere yalnızca Active Directory'den eşitlenen gruplar dahil edilir.

![iddia UI](media/how-to-connect-fed-group-claims/group-claims-ui-4.png)

Yalnızca uygulamaya atanan grupları yaymak **için, uygulamaya Atanan Gruplar'ı** seçin

![iddia UI](media/how-to-connect-fed-group-claims/group-claims-ui-4-1.png)

Uygulamaya atanan gruplar belirtecidahil edilecektir.  Kullanıcının üyesi olduğu diğer gruplar atlanacaktır.  Bu seçenek ile iç içe gruplar dahil edilmez ve kullanıcı uygulamaya atanan grubun doğrudan bir üyesi olmalıdır.

Uygulamaya atanan grupları değiştirmek **için, Kurumsal Uygulamalar** listesinden uygulamayı seçin ve ardından uygulamanın sol daki gezinme menüsünden **Kullanıcılar ve Gruplar'ı** tıklatın.

Uygulamalara grup atamasını yönetme ayrıntıları için [bir kullanıcıyı veya grubu kurumsal bir uygulamaya atama](../../active-directory/manage-apps/assign-user-or-group-access-portal.md) kalım belgesine bakın.

### <a name="advanced-options"></a>Gelişmiş seçenekler

Grup taleplerinin yayma şekli Gelişmiş seçenekler altındaki ayarlarla değiştirilebilir

Grup talebinin adını özelleştirin: Seçilirse, grup talepleri için farklı bir talep türü belirtilebilir.   Ad alanına talep türünü ve ad alanı alanına talep için isteğe bağlı ad alanını girin.

![iddia UI](media/how-to-connect-fed-group-claims/group-claims-ui-5.png)

Bazı uygulamalar, grup üyelik bilgilerinin 'rol' iddiasında görünmesini gerektirir. İsteğe bağlı olarak , 'Rol iddialarını gruphaline erdir' kutusunu işaretleyerek kullanıcının gruplarını rol olarak yatabilirsiniz.

![iddia UI](media/how-to-connect-fed-group-claims/group-claims-ui-6.png)

> [!NOTE]
> Grup verilerini rol olarak yayacak seçeneği kullanılırsa, rol iddiasında yalnızca gruplar görünür.  Kullanıcının atandığı Uygulama Rolleri rol talebinde görünmez.

### <a name="edit-the-group-claims-configuration"></a>Grup taleplerini yapılandırmasını düzenleme

Kullanıcı Öznitelikleri & Talepler yapılandırmasına bir grup talebi yapılandırması eklendikten sonra, grup talebi ekleme seçeneği soluklanır.  Grup talep yapılandırmasını değiştirmek için Ek **talepler** listesindeki grup talebine tıklayın.

![iddia UI](media/how-to-connect-fed-group-claims/group-claims-ui-7.png)

## <a name="configure-the-azure-ad-application-registration-for-group-attributes"></a>Grup öznitelikleri için Azure AD Uygulama Kaydını yapılandırma

Grup talepleri, [Uygulama Bildirimi'nin](../../active-directory/develop/reference-app-manifest.md) [İsteğe Bağlı Talepler](../../active-directory/develop/active-directory-optional-claims.md) bölümünde de yapılandırılabilir.

1. >Azure Active Directory -> Uygulama Kayıtları->Select Application->Manifestosu

2. Grubu değiştirerek grup üyelik taleplerini etkinleştirinÜyelikClaim

Geçerli değerler:

| Seçim | Açıklama |
|----------|-------------|
| **"Tümü"** | Güvenlik grupları, dağıtım listeleri ve rolleri yayır |
| **"Güvenlik Grubu"** | Kullanıcının üyesi olduğu güvenlik gruplarını gruplara yayır iddia |
| **"DizinRol** | Kullanıcıya dizin rolleri atanmışsa, bunlar 'wids' iddiası olarak yayılır (grup iddiası yayılamaz) |
| **"Uygulama Grubu** | Yalnızca uygulamaya açıkça atanan grupları yayır ve kullanıcı |

   Örnek:

   ```json
   "groupMembershipClaims": "SecurityGroup"
   ```

   Varsayılan olarak Grup ObjectIDs grup talep değeri yayılacak.  Talep değerini şirket grup özniteliklerini içerecek şekilde değiştirmek veya talep türünü role değiştirmek için İsteğe Bağlı Talepler yapılandırmasını aşağıdaki gibi kullanın:

3. Grup adı yapılandırması isteğe bağlı talepleri ayarlayın.

   Belirteçteki grupların şirket içi AD grubu özniteliklerini içermesini istiyorsanız, isteğe bağlı talepler bölümünde hangi belirteç türüne isteğe bağlı talep uygulanması gerektiğini belirtin.  Birden çok belirteç türü listelenebilir:

   - OIDC kimlik belirteci için idToken
   - oAuth/OIDC erişim jetonu için Token'e erişin
   - SAML belirteçleri için Saml2Token.

   > [!NOTE]
   > Saml2Token türü hem SAML1.1 hem de SAML2.0 biçim belirteçleri için geçerlidir

   İlgili her belirteç türü için, bildirimdeki İsteğe Bağlı Talepler bölümünü kullanma iddiasını grupları değiştirin. İsteğe Bağlı İddialar şema aşağıdaki gibidir:

   ```json
   {
   "name": "groups",
   "source": null,
   "essential": false,
   "additionalProperties": []
   }
   ```

   | İsteğe Bağlı İddialar Şeması | Değer |
   |----------|-------------|
   | **Adı:** | "Gruplar" olmalıdır |
   | **Kaynak:** | Kullanılmadı. Geçersiz kılın |
   | **Temel:** | Kullanılmadı. Yanlışı atla veya belirt |
   | **ek Özellikler:** | Ek özelliklerin listesi.  Geçerli seçenekler "sam_account_name", "dns_domain_and_sam_account_name", "netbios_domain_and_sam_account_name", "emit_as_roles" |

   Ek Özellikler'de yalnızca "sam_account_name", "dns_domain_and_sam_account_name", "netbios_domain_and_sam_account_name" gerekir.  Birden fazla varsa, ilk kullanılır ve diğerleri göz ardı.

   Bazı uygulamalar, rol talebindeki kullanıcı hakkında grup bilgileri gerektirir.  Talep türünü bir grup talebinden bir rol talebine değiştirmek için ek özelliklere "emit_as_roles" ekleyin.  Grup değerleri rol talebinde yayılacaktır.

   > [!NOTE]
   > Kullanıcının atandığı yapılandırılan herhangi bir Uygulama Rolü "emit_as_roles" kullanılırsa, rol iddiasında görünmez

### <a name="examples"></a>Örnekler

Grupları OAuth erişim belirteçleri olarak grup adları olarak dnsDomainName\SAMAccountName biçiminde yaslar

```json
"optionalClaims": {
    "accessToken": [{
        "name": "groups",
        "additionalProperties": ["dns_domain_and_sam_account_name"]
    }]
}
 ```

SAML ve OIDC ID Belirteçleri'ndeki rollerin iddia ettiği gibi netbiosDomain\samAccountName formatında döndürülecek grup adlarını yalamak için:

```json
"optionalClaims": {
    "saml2Token": [{
        "name": "groups",
        "additionalProperties": ["netbios_name_and_sam_account_name", "emit_as_roles"]
    }],

    "idToken": [{
        "name": "groups",
        "additionalProperties": ["netbios_name_and_sam_account_name", "emit_as_roles"]
    }]
 }
 ```

## <a name="next-steps"></a>Sonraki adımlar

[Bir kurumsal uygulamaya kullanıcı veya grup atama](../../active-directory/manage-apps/assign-user-or-group-access-portal.md)

[Rol taleplerini yapılandırma](../../active-directory/develop/active-directory-enterprise-app-role-management.md)
