---
title: Özel ilkelere kendi öznitelerinizi ekleme
titleSuffix: Azure AD B2C
description: Uzantı özellikleri ve özel öznitelikler kullanma ve bunları kullanıcı arayüzüne ekleme hakkında bir anlatım.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81678262"
---
# <a name="azure-active-directory-b2c-enable-custom-attributes-in-a-custom-profile-policy"></a>Azure Active Directory B2C: özel bir profil ilkesinde özel öznitelikleri etkinleştirme

[Özel ilkeler kullanarak talep ekleme ve Kullanıcı girişini özelleştirme](custom-policy-configure-user-input.md) makalesinde yerleşik [Kullanıcı profili özniteliklerini](user-profile-attributes.md)nasıl kullanacağınızı öğreneceksiniz. Bu makalede, Azure Active Directory B2C (Azure AD B2C) dizininizde özel bir özniteliği etkinleştirirsiniz. Daha sonra, yeni özniteliğini [Kullanıcı akışlarında](user-flow-overview.md) veya [özel ilkelerde](custom-policy-get-started.md) aynı anda özel bir talep olarak kullanabilirsiniz.

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

## <a name="prerequisites"></a>Ön koşullar

[Azure Active Directory B2C: özel ilkeleri kullanmaya başlama](custom-policy-get-started.md)makalesindeki adımları izleyin.

## <a name="use-custom-attributes-to-collect-information-about-your-customers"></a>Müşterileriniz hakkında bilgi toplamak için özel öznitelikler kullanın 

Azure AD B2C dizininiz [yerleşik bir öznitelikler kümesiyle](user-profile-attributes.md)birlikte gelir. Bununla birlikte, belirli senaryonuzu yönetmek için genellikle kendi öznitelerinizi oluşturmanız gerekir, örneğin:

* Müşteriye yönelik bir uygulamanın **Loyaltyıd** özniteliğini kalıcı hale getirmek gerekir.
* Bir kimlik sağlayıcısı, kalıcı olması gereken benzersiz bir Kullanıcı tanımlayıcısına sahip olan **Uniqueuserguid**öğesine sahiptir.
* Özel bir Kullanıcı yolculuğu, diğer mantığın üzerinde çalışması için kullanıcının, **Migrationstatus**durumunu kalıcı hale getirebilmesini gerektirir.

Azure AD B2C, her kullanıcı hesabında depolanan özniteliklerin kümesini genişletmenizi sağlar. Ayrıca, [MICROSOFT Graph API](manage-user-accounts-graph-api.md)'sini kullanarak bu öznitelikleri okuyabilir ve yazabilirsiniz.

## <a name="azure-ad-b2c-extensions-app"></a>Azure AD B2C uzantıları uygulaması

Uzantı öznitelikleri, bir kullanıcı için veri içerse de, yalnızca bir uygulama nesnesine kaydedilebilir. Extension özniteliği B2C-Extensions-App adlı uygulamaya iliştirilir. Bu uygulamayı, Kullanıcı verilerini depolamak için Azure AD B2C tarafından kullanıldığından değiştirmeyin. Bu uygulamayı Azure AD B2C, uygulama kayıtları altında bulabilirsiniz.

Terimler *uzantı özelliği*, *özel öznitelik*ve *özel talep* , bu makalenin bağlamıyla aynı şeyi ifade eder. Ad, uygulama, nesne veya ilke gibi içeriğe göre değişir.

## <a name="get-the-application-properties"></a>Uygulama özelliklerini al

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Üst menüden **Dizin + abonelik** filtresi ' ni seçin ve ardından Azure AD B2C kiracınızı içeren dizini seçin.
1. Sol menüden **Azure AD B2C**' yi seçin. Ya da **tüm hizmetler** ' i seçin ve **Azure AD B2C**seçin.
1. **Uygulama kayıtları (Önizleme)** öğesini seçin ve ardından **tüm uygulamalar**' ı seçin.
1. `b2c-extensions-app. Do not modify. Used by AADB2C for storing user data.` Uygulamayı seçin.
1. Aşağıdaki tanımlayıcıları panonuza kopyalayın ve kaydedin:
    * **Uygulama kimliği**. Örnek: `11111111-1111-1111-1111-111111111111`.
    * **Nesne kimliği**. Örnek: `22222222-2222-2222-2222-222222222222`.

## <a name="modify-your-custom-policy"></a>Özel ilkenizi değiştirme

İlkenizde özel öznitelikleri etkinleştirmek için AAD ortak teknik profil meta verilerinde **uygulama kimliği** ve uygulama **nesne kimliği** sağlayın. *AAD ortak* teknik profili, temel [Azure Active Directory](active-directory-technical-profile.md) teknik PROFILINDE bulunur ve Azure AD Kullanıcı yönetimi için destek sağlar. Diğer Azure AD teknik profilleri, yapılandırmasından yararlanmak için AAD-Common ' i içerir. Uzantı dosyasındaki AAD ortak teknik profilini geçersiz kılın.

1. İlkenizin uzantıları dosyasını açın. Örneğin, <em> `SocialAndLocalAccounts/` </em>.
1. ClaimsProviders öğesini bulun. ClaimsProviders öğesine yeni bir ClaimsProvider ekleyin.
1. Daha `ApplicationObjectId` önce KAYDETTIĞINIZ nesne kimliğiyle değiştirin. Ardından aşağıdaki `ClientId` kod parçacığında daha önce KAYDETTIĞINIZ uygulama kimliğiyle değiştirin.

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

## <a name="upload-your-custom-policy"></a>Özel ilkenizi karşıya yükleyin

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Üst menüdeki **Dizin + abonelik** filtresini seçip Azure AD B2C kiracınızı içeren dizini seçerek Azure AD kiracınızı içeren dizini kullandığınızdan emin olun.
3. Azure portal sol üst köşesindeki **tüm hizmetler** ' i seçin ve ardından **uygulama kayıtları**' i arayıp seçin.
4. **Kimlik deneyimi çerçevesini**seçin.
5. **Özel Ilkeyi karşıya yükle**' yi seçin ve ardından değiştirdiğiniz TrustFrameworkExtensions. xml ilke dosyalarını karşıya yükleyin.

> [!NOTE]
> Azure AD teknik profili, Dizin talebini ilk kez sürdüren, özel özniteliğin mevcut olup olmadığını denetler. Aksi takdirde, özel özniteliğini oluşturur.  

## <a name="create-a-custom-attribute-through-azure-portal"></a>Azure portal aracılığıyla özel öznitelik oluşturma

Aynı uzantı öznitelikleri, yerleşik ve özel ilkeler arasında paylaşılır. Portal deneyimi aracılığıyla özel öznitelikler eklediğinizde, bu öznitelikler her B2C kiracısında bulunan **B2C-Extensions-App** kullanılarak kaydedilir.

Bu öznitelikleri, özel ilkeleriniz içinde kullanmadan önce veya sonra Portal Kullanıcı arabirimini kullanarak oluşturabilirsiniz. [Azure Active Directory B2C özel özniteliklerin tanımlanması](user-flow-custom-attributes.md)için yönergeleri izleyin. Portalda **Loyaltyıd** özniteliğini oluşturduğunuzda, aşağıdaki şekilde buna başvurmanız gerekir:

|Adı     |Kullanıldığı yer |
|---------|---------|
|`extension_loyaltyId`  | Özel ilke|
|`extension_<b2c-extensions-app-guid>_loyaltyId`  | [Microsoft Graph API'si](manage-user-accounts-graph-api.md)|

Aşağıdaki örnek, özel özniteliklerin Azure AD B2C özel bir ilke talep tanımında kullanımını gösterir.

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

Aşağıdaki örnek, bir teknik profilde, girişte, çıkışta ve kalıcı taleplerde Azure AD B2C özel ilkede özel bir özniteliğin kullanımını gösterir.

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

[Özel ilkeler kullanarak talepler ekleme ve Kullanıcı girişini özelleştirme](custom-policy-configure-user-input.md)kılavuzunu izleyin. Bu örnek, yerleşik bir ' City ' talebi kullanır. Özel bir öznitelik kullanmak için ' City ' değerini kendi özel nitelikleriyle değiştirin.


## <a name="next-steps"></a>Sonraki adımlar

Aşağıdakiler hakkında daha fazla bilgi edinin:

- [Kullanıcı profili özniteliklerini Azure AD B2C](user-profile-attributes.md)
- [Uzantı öznitelikleri tanımı](user-profile-attributes.md#extension-attributes)
- [Microsoft Graph ile Azure AD B2C Kullanıcı hesaplarını yönetme](manage-user-accounts-graph-api.md)
