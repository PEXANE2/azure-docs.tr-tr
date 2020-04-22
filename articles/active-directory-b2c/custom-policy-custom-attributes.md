---
title: Özel ilkelere kendi özniteliklerinizi ekleyin
titleSuffix: Azure AD B2C
description: Uzantı özelliklerini ve özel özniteliklerini kullanma ve bunları kullanıcı arabirimine dahil etme konusunda bir gözden geçirme.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/17/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: b5990f79891a9cbc0d18c3499691a3d7ef309a73
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81678262"
---
# <a name="azure-active-directory-b2c-enable-custom-attributes-in-a-custom-profile-policy"></a>Azure Active Directory B2C: Özel profil ilkesinde özel öznitelikleri etkinleştirme

Talep [ekle ve özel ilkeler makalesini kullanarak kullanıcı girdisini özelleştirdiğinizde](custom-policy-configure-user-input.md) yerleşik [kullanıcı profili özniteliklerini](user-profile-attributes.md)kullanmayı öğrenirsiniz. Bu makalede, Azure Etkin Dizin B2C (Azure AD B2C) dizininizde özel bir öznitelik etkinleştirin. Daha sonra, yeni özniteliği [kullanıcı akışlarında](user-flow-overview.md) veya [özel ilkelerde](custom-policy-get-started.md) aynı anda özel bir talep olarak kullanabilirsiniz.

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

## <a name="prerequisites"></a>Ön koşullar

Azure Active Directory B2C makalesindeki adımları [izleyin: Özel ilkelerle başlayın.](custom-policy-get-started.md)

## <a name="use-custom-attributes-to-collect-information-about-your-customers"></a>Müşterileriniz hakkında bilgi toplamak için özel öznitelikleri kullanın 

Azure AD B2C dizininiz yerleşik bir [öznitelikler kümesiyle](user-profile-attributes.md)birlikte gelir. Ancak, örneğin:

* Müşteriye yönelik bir uygulamanın **LoyaltyId** özniteliğini devam etmesi gerekir.
* Bir kimlik sağlayıcısı, kalıcı olması gereken benzersiz bir kullanıcı tanımlayıcısı, **benzersizUserGUID**vardır.
* Özel bir kullanıcı yolculuğu kullanıcının durumunu devam etmesi gerekir, **migrationStatus**, diğer mantık üzerinde çalışması için.

Azure AD B2C, her kullanıcı hesabında depolanan öznitelikler kümesini genişletmenize olanak tanır. Ayrıca [Microsoft Graph API'yi](manage-user-accounts-graph-api.md)kullanarak bu öznitelikleri okuyabilir ve yazabilirsiniz.

## <a name="azure-ad-b2c-extensions-app"></a>Azure AD B2C uzantıları uygulaması

Uzantı öznitelikleri, kullanıcı için veri içerse ler bile yalnızca bir uygulama nesnesi üzerinde kaydedilebilir. Uzantı özniteliği b2c-extensions-app adı verilen uygulamaya eklenir. Azure AD B2C tarafından kullanıcı verilerini depolamak için kullanıldığından bu uygulamayı değiştirmeyin. Bu uygulamayı Azure AD B2C, uygulama kayıtları altında bulabilirsiniz.

Terimler *uzantısı özelliği,* *özel öznitelik*ve *özel talep* bu makalenin bağlamında aynı şeyi ifade eder. Ad, uygulama, nesne veya ilke gibi içeriğe bağlı olarak değişir.

## <a name="get-the-application-properties"></a>Uygulama özelliklerini alma

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Üst menüdeki **Dizin + abonelik** filtresini seçin ve ardından Azure AD B2C kiracınızı içeren dizin'i seçin.
1. Sol menüde **Azure AD B2C'yi**seçin. Veya **Tüm hizmetleri** seçin ve **Azure AD B2C'yi**arayın ve seçin.
1. **Uygulama kayıtlarını (Önizleme)** seçin ve ardından **Tüm Uygulamalar'ı**seçin.
1. `b2c-extensions-app. Do not modify. Used by AADB2C for storing user data.` Uygulamayı seçin.
1. Aşağıdaki tanımlayıcıları panonuza kopyalayın ve kaydedin:
    * **Başvuru Kimliği**. Örnek: `11111111-1111-1111-1111-111111111111`.
    * **Nesne Kimliği**. Örnek: `22222222-2222-2222-2222-222222222222`.

## <a name="modify-your-custom-policy"></a>Özel politikanızı değiştirme

İlkenizde özel öznitelikleri etkinleştirmek için AAD-Common teknik profil meta verilerinde **Uygulama Kimliği** ve Uygulama **Nesnesi Kimliği** sağlayın. *AAD-Common* teknik profili temel [Azure Active Directory](active-directory-technical-profile.md) teknik profilinde bulunur ve Azure AD kullanıcı yönetimi için destek sağlar. Diğer Azure AD teknik profilleri, yapılandırmasını sağlamak için AAD-Common'ı içerir. Uzantı dosyasındaki AAD-Common teknik profilini geçersiz kılın.

1. İlkinizin uzantılar dosyasını açın. Örneğin, <em> `SocialAndLocalAccounts/` </em>.
1. İddia Sağlayıcıları öğesini bulun. İddia Sağlayıcıları öğesine yeni bir Talep Sağlayıcısı ekleyin.
1. Daha `ApplicationObjectId` önce kaydettiğiniz Nesne Kimliği ile değiştirin. Ardından, `ClientId` daha önce aşağıdaki snippet'te kaydettiğiniz Uygulama Kimliği ile değiştirin.

    ```xml
    <ClaimsProvider>
      <DisplayName>Azure Active Directory</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="AAD-Common">
          <Metadata>
            <!--Insert b2c-extensions-app application ID here, for example: 11111111-1111-1111-1111-111111111111-->  
            <Item Key="ClientId"></Item>
            <!--Insert b2c-extensions-app application ObjectId here, for example: 22222222-2222-2222-2222-222222222222-->
            <Item Key="ApplicationObjectId"></Item>
          </Metadata>
        </TechnicalProfile>
      </TechnicalProfiles> 
    </ClaimsProvider>
    ```

## <a name="upload-your-custom-policy"></a>Özel politikanızı yükleyin

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Üst menüdeki **Dizin + abonelik** filtresini seçerek ve Azure AD B2C kiracınızı içeren dizin seçerek Azure AD kiracınızı içeren dizini kullandığınızdan emin olun.
3. Azure portalının sol üst köşesindeki **tüm hizmetleri** seçin ve ardından **Uygulama kayıtlarını**arayın ve seçin.
4. **Kimlik Deneyimi Çerçevesi'ni**seçin.
5. **Özel İlke Yükle'yi**seçin ve ardından değiştirdiğiniz TrustFrameworkExtensions.xml ilke dosyalarını yükleyin.

> [!NOTE]
> Azure AD teknik profili dizin iddiasını ilk kez devam ettir, özel özniteliğin var olup olmadığını denetler. Değilse, özel öznitelik oluşturur.  

## <a name="create-a-custom-attribute-through-azure-portal"></a>Azure portalı üzerinden özel bir öznitelik oluşturma

Aynı uzantı öznitelikleri yerleşik ve özel ilkeler arasında paylaşılır. Portal deneyimi aracılığıyla özel öznitelikler eklediğinizde, bu öznitelikler her B2C kiracısında bulunan **b2c uzantıları-uygulaması** kullanılarak kaydedilir.

Bu öznitelikleri, özel ilkelerinizde kullanmadan önce veya sonra portal Kullanıcı Arabirimi'ni kullanarak oluşturabilirsiniz. [Azure Etkin Dizin B2C'de özel öznitelikleri nasıl tanımlayacaklarına](user-flow-custom-attributes.md)yönelik kılavuzu izleyin. Portalda bir öznitelik **loyaltyId** oluşturduğunuzda, bu özneye aşağıdaki gibi başvurmanız gerekir:

|Adı     |Kullanılan |
|---------|---------|
|`extension_loyaltyId`  | Özel ilke|
|`extension_<b2c-extensions-app-guid>_loyaltyId`  | [Microsoft Graph API'si](manage-user-accounts-graph-api.md)|

Aşağıdaki örnek, Azure AD B2C özel ilke talep tanımında özel özniteliklerin kullanımını göstermektedir.

```xml
<BuildingBlocks>
  <ClaimsSchema>
    <ClaimType Id="extension_loyaltyId">
      <DisplayName>Loyalty Identification</DisplayName>
      <DataType>string</DataType>
      <UserHelpText>Your loyalty number from your membership card</UserHelpText>
      <UserInputType>TextBox</UserInputType>
    </ClaimType>
  </ClaimsSchema>
</BuildingBlocks>
```

Aşağıdaki örnek, Azure AD B2C özel ilkesinde teknik profil, giriş, çıktı ve kalıcı taleplerde özel bir öznitelik kullanımını göstermektedir.

```xml
<InputClaims>
  <InputClaim ClaimTypeReferenceId="extension_loyaltyId"  />
</InputClaims>
<PersistedClaims>
  <PersistedClaim ClaimTypeReferenceId="extension_loyaltyId" />
</PersistedClaims>
<OutputClaims>
  <OutputClaim ClaimTypeReferenceId="extension_loyaltyId" />
</OutputClaims>
```

## <a name="use-a-custom-attribute-in-a-policy"></a>İlkede özel bir öznitelik kullanma

Özel ilkeleri kullanarak [talep ekleme ve kullanıcı girişini özelleştirme kılavuzunu izleyin.](custom-policy-configure-user-input.md) Bu örnek, yerleşik bir talep 'şehir' kullanır. Özel bir öznitelik kullanmak için kendi özel öznitelikleriile 'şehir' değiştirin.


## <a name="next-steps"></a>Sonraki adımlar

Aşağıdakiler hakkında daha fazla bilgi edinin:

- [Azure AD B2C kullanıcı profili öznitelikleri](user-profile-attributes.md)
- [Uzantı öznitelikleri tanımı](user-profile-attributes.md#extension-attributes)
- [Microsoft Graph ile Azure AD B2C kullanıcı hesaplarını yönetme](manage-user-accounts-graph-api.md)
