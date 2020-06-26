---
title: Azure AD Kullanıcı akışlarında dil özelleştirmesi
description: Kullanıcı akışlarınızda dil deneyimini özelleştirme hakkında bilgi edinin.
services: active-directory
author: msmimart
manager: celestedg
ms.service: active-directory
ms.subservice: B2B
ms.topic: how-to
ms.date: 05/06/2020
ms.author: mimart
ms.reviewer: elisolMS
ms.collection: M365-identity-device-management
ms.openlocfilehash: 856338e6c5a2bac55fee57d3ea09575a1a4d14ed
ms.sourcegitcommit: b56226271541e1393a4b85d23c07fd495a4f644d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85386547"
---
# <a name="language-customization-in-azure-active-directory-preview"></a>Azure Active Directory dil özelleştirmesi (Önizleme)
|     |
| --- |
| Self Servis kaydolma, Azure Active Directory genel önizleme özelliğidir. Önizlemeler hakkında daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).|
|     |

Azure Active Directory (Azure AD) içindeki dil özelleştirmesi, Kullanıcı akışınızın kullanıcılarınızın ihtiyaçlarına uyacak şekilde farklı dillere uyum sağlamasına izin verir. Microsoft, [36 dil](#supported-languages)için Çeviriler sağlar. Deneyiminiz yalnızca tek bir dil için sağlanmış olsa bile öznitelik koleksiyonu sayfasında öznitelik adlarını özelleştirebilirsiniz.

## <a name="how-language-customization-works"></a>Dil özelleştirmesi nasıl kullanılır?

Varsayılan olarak, tutarlı bir kaydolma deneyimi sağlamak üzere kaydolan kullanıcılar için dil özelleştirmesi etkinleştirilmiştir. Kayıt sırasında öznitelik toplama işleminin parçası olarak kullanıcılara görüntülenecek dizeleri değiştirmek için dilleri kullanabilirsiniz.

> [!NOTE]
> Özel Kullanıcı özniteliklerini kullanıyorsanız kendi çevirilerinizi sağlamanız gerekir. Daha fazla bilgi için bkz. [dizelerinizi özelleştirme](#customize-your-strings).

## <a name="customize-your-strings"></a>Dizelerinizi özelleştirin

Dil özelleştirmesi, Kullanıcı akışındaki herhangi bir dizeyi özelleştirmenize olanak sağlar.

1. [Azure portalda](https://portal.azure.com) Azure AD yöneticisi olarak oturum açın.
2. **Azure hizmetleri**altında **Azure Active Directory**' yi seçin.
3. Sol taraftaki menüden **dış kimlikler**' i seçin.
4. **Kullanıcı akışları ' nı (Önizleme)** seçin.
3. Çeviriler için etkinleştirmek istediğiniz kullanıcı akışını seçin.
4. **Dilleri**seçin.
5. Kullanıcı akışının **Diller** sayfasında, özelleştirmek istediğiniz dili seçin.
6. **Öznitelik koleksiyonu sayfasını**genişletin.
7. **Varsayılanları indir** ' i seçin (veya daha önce bu dili düzenlediyseniz, **geçersiz kılmaları indir** ).

Bu adımlar size Dizelerinizin düzenlenmesine başlamak için kullanabileceğiniz bir JSON dosyası sağlar.

### <a name="change-any-string-on-the-page"></a>Sayfadaki herhangi bir dizeyi değiştirme

1. JSON düzenleyicisinde önceki yönergelerden indirilen JSON dosyasını açın.
1. Değiştirmek istediğiniz öğeyi bulun. `StringId`Aradığınız dize için arama yapabilir veya `Value` değiştirmek istediğiniz özniteliği arayabilirsiniz.
1. Özniteliğini, `Value` görüntülenmesini istediğiniz şekilde güncelleştirin.
1. Değiştirmek istediğiniz her dize için `Override` olarak değiştirin `true` .
1. Dosyayı kaydedin ve değişikliklerinizi karşıya yükleyin. (Karşıya yükleme denetimini, JSON dosyasını indirdiğiniz konum ile aynı yerde bulabilirsiniz.)

> [!IMPORTANT]
> Bir dizeyi geçersiz kılmanız gerekirse değerini olarak ayarladığınızdan emin olun `Override` `true` . Değer değiştirilmemişse, giriş yok sayılır.

### <a name="change-extension-attributes"></a>Uzantı özniteliklerini Değiştir

Bir özel kullanıcı özniteliği için dizeyi değiştirmek isterseniz veya bir JSON öğesine eklemek istiyorsanız, şu biçimdedir:

```JSON
{
  "LocalizedStrings": [
    {
      "ElementType": "ClaimType",
      "ElementId": "extension_<ExtensionAttribute>",
      "StringId": "DisplayName",
      "Override": true,
      "Value": "<ExtensionAttributeValue>"
    }
    [...]
}
```

`<ExtensionAttribute>`Özel Kullanıcı özniteme adıyla değiştirin.

`<ExtensionAttributeValue>`Görüntülenecek yeni dize ile değiştirin.

### <a name="provide-a-list-of-values-by-using-localizedcollections"></a>LocalizedCollections kullanarak değer listesi sağlama

Yanıtlar için bir değer listesi kümesi sağlamak istiyorsanız, bir öznitelik oluşturmanız gerekir `LocalizedCollections` . `LocalizedCollections`, `Name` ve `Value` çiftleri dizisidir. Öğelerin sırası görüntülendikleri sıra olacaktır. Eklemek için `LocalizedCollections` aşağıdaki biçimi kullanın:

```JSON
{
  "LocalizedStrings": [...],
  "LocalizedCollections": [{
      "ElementType":"ClaimType",
      "ElementId":"<UserAttribute>",
      "TargetCollection":"Restriction",
      "Override": true,
      "Items":[
           {
                "Name":"<Response1>",
                "Value":"<Value1>"
           },
           {
                "Name":"<Response2>",
                "Value":"<Value2>"
           }
     ]
  }]
}
```

* `ElementId`, bu `LocalizedCollections` özniteliğin yanıt olduğu Kullanıcı özniteliğidir.
* `Name`, kullanıcıya gösterilen değerdir.
* `Value`Bu seçenek belirlendiğinde talepte döndürülen değer.

### <a name="upload-your-changes"></a>Değişikliklerinizi karşıya yükleyin

1. JSON dosyanızdaki değişiklikleri tamamladıktan sonra kiracınıza geri dönün.
1. **Kullanıcı akışları** ' nı seçin ve çeviriler için etkinleştirmek istediğiniz kullanıcı akışına tıklayın.
1. **Dilleri**seçin.
1. Çevirmek istediğiniz dili seçin.
1. **Öznitelik koleksiyonu sayfası**seçin.
1. Klasör simgesini seçin ve karşıya yüklenecek JSON dosyasını seçin.

Değişiklikler Kullanıcı akışınıza otomatik olarak kaydedilir.

## <a name="additional-information"></a>Ek bilgiler

### <a name="page-ui-customization-labels-as-overrides"></a>Geçersiz kılmalar olarak sayfa Kullanıcı arabirimi özelleştirme etiketleri

Dil özelleştirmesini etkinleştirdiğinizde, sayfa UI özelleştirmesi kullanan etiketlere yönelik önceki düzenlemeleriniz Ingilizce (en) için bir JSON dosyasında kalıcıdır. Dil özelleştirmesindeki dil kaynaklarını karşıya yükleyerek etiketlerinizi ve diğer dizelerinizi değiştirmeye devam edebilirsiniz.

### <a name="up-to-date-translations"></a>Güncel Çeviriler

Microsoft, kullanım için en güncel çevirileri sağlamaya kararlıdır. Microsoft, çevirileri sürekli olarak geliştirir ve sizin için uyumluluk sağlar. Microsoft, genel terminolojilerin hatalarını ve değişikliklerini belirler ve Kullanıcı akışınızda sorunsuz bir şekilde çalışacak güncelleştirmeler yapar.

### <a name="support-for-right-to-left-languages"></a>Sağdan sola diller için destek

Microsoft şu anda sağdan sola diller için destek sağlamaz. Bu bunu, özel yerel ayarları kullanarak ve dizelerin görüntülenme biçimini değiştirmek için CSS kullanarak gerçekleştirebilirsiniz. Bu özelliğe ihtiyacınız varsa lütfen [Azure geri bildirimde](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/19393000-provide-language-support-for-right-to-left-languag)bu uygulamayı oylayın.

### <a name="social-identity-provider-translations"></a>Sosyal kimlik sağlayıcısı çevirileri

Microsoft, `ui_locales` sosyal oturumlar için OIDC parametresini sağlar. Ancak Facebook ve Google dahil bazı sosyal kimlik sağlayıcıları bunları dikkate almaz.

### <a name="browser-behavior"></a>Tarayıcı davranışı

Chrome ve Firefox, kendi set dilleri için istek. Desteklenen bir dilise, varsayılan değer olarak gösterilir. Microsoft Edge Şu anda bir dil isteyemez ve doğrudan varsayılan dile gider.

## <a name="supported-languages"></a>Desteklenen diller

Azure AD aşağıdaki diller için destek içerir. Kullanıcı akış dilleri Azure AD tarafından sağlanır. Multi-Factor Authentication (MFA) bildirim dilleri [Azure MFA](https://docs.microsoft.com/azure/active-directory/authentication/concept-mfa-howitworks)tarafından sağlanır.

| Dil              | Dil kodu | Kullanıcı akışları         | MFA bildirimleri  |
|-----------------------| :-----------: | :----------------: | :----------------: |
| Arapça                | Ar            | ![hayır](./media/user-flow-customize-language/no.png) | ![evet](./media/user-flow-customize-language/yes.png) |
| Bulgarca             | bg            | ![hayır](./media/user-flow-customize-language/no.png) | ![evet](./media/user-flow-customize-language/yes.png) |
| Bangla                | milyar TL            | ![evet](./media/user-flow-customize-language/yes.png) | ![hayır](./media/user-flow-customize-language/no.png) |
| Katalanca               | yetkilisini            | ![hayır](./media/user-flow-customize-language/no.png) | ![evet](./media/user-flow-customize-language/yes.png) |
| Çekçe                 | 'ye            | ![evet](./media/user-flow-customize-language/yes.png) | ![evet](./media/user-flow-customize-language/yes.png) |
| Danca                | kapattığımda            | ![evet](./media/user-flow-customize-language/yes.png) | ![evet](./media/user-flow-customize-language/yes.png) |
| Almanca                | seçimini            | ![evet](./media/user-flow-customize-language/yes.png) | ![evet](./media/user-flow-customize-language/yes.png) |
| Yunanca                 | seri            | ![evet](./media/user-flow-customize-language/yes.png) | ![evet](./media/user-flow-customize-language/yes.png) |
| İngilizce               | tr            | ![evet](./media/user-flow-customize-language/yes.png) | ![evet](./media/user-flow-customize-language/yes.png) |
| İspanyolca               | es            | ![evet](./media/user-flow-customize-language/yes.png) | ![evet](./media/user-flow-customize-language/yes.png) |
| Estonya Dili              | lale            | ![hayır](./media/user-flow-customize-language/no.png) | ![evet](./media/user-flow-customize-language/yes.png) |
| Baskça                | yapılan            | ![hayır](./media/user-flow-customize-language/no.png) | ![evet](./media/user-flow-customize-language/yes.png) |
| Fince               | Fi            | ![evet](./media/user-flow-customize-language/yes.png) | ![evet](./media/user-flow-customize-language/yes.png) |
| Fransızca                | kesir            | ![evet](./media/user-flow-customize-language/yes.png) | ![evet](./media/user-flow-customize-language/yes.png) |
| Galiçya Dili              | g            | ![hayır](./media/user-flow-customize-language/no.png) | ![evet](./media/user-flow-customize-language/yes.png) |
| Gucerat dili              | çubuğu            | ![evet](./media/user-flow-customize-language/yes.png) | ![hayır](./media/user-flow-customize-language/no.png) |
| İbranice                | LIP            | ![hayır](./media/user-flow-customize-language/no.png) | ![evet](./media/user-flow-customize-language/yes.png) |
| Hintçe                 | n            | ![evet](./media/user-flow-customize-language/yes.png) | ![evet](./media/user-flow-customize-language/yes.png) |
| Hırvatça              | sa            | ![evet](./media/user-flow-customize-language/yes.png) | ![evet](./media/user-flow-customize-language/yes.png) |
| Macarca             | Hu            | ![evet](./media/user-flow-customize-language/yes.png) | ![evet](./media/user-flow-customize-language/yes.png) |
| Endonezce            | kimlik            | ![hayır](./media/user-flow-customize-language/no.png) | ![evet](./media/user-flow-customize-language/yes.png) |
| İtalyanca               | içerdiği            | ![evet](./media/user-flow-customize-language/yes.png) | ![evet](./media/user-flow-customize-language/yes.png) |
| Japonca              | Sofya            | ![evet](./media/user-flow-customize-language/yes.png) | ![evet](./media/user-flow-customize-language/yes.png) |
| Kazakça                | kk            | ![hayır](./media/user-flow-customize-language/no.png) | ![evet](./media/user-flow-customize-language/yes.png) |
| Kannada dili               | KN            | ![evet](./media/user-flow-customize-language/yes.png) | ![hayır](./media/user-flow-customize-language/no.png) |
| Korece                | dili            | ![evet](./media/user-flow-customize-language/yes.png) | ![evet](./media/user-flow-customize-language/yes.png) |
| Litvanca            | lt            | ![hayır](./media/user-flow-customize-language/no.png) | ![evet](./media/user-flow-customize-language/yes.png) |
| Letonca               | aramasını            | ![hayır](./media/user-flow-customize-language/no.png) | ![evet](./media/user-flow-customize-language/yes.png) |
| Malayalam dili             | ml            | ![evet](./media/user-flow-customize-language/yes.png) | ![hayır](./media/user-flow-customize-language/no.png) |
| Marathi               | Mr            | ![evet](./media/user-flow-customize-language/yes.png) | ![hayır](./media/user-flow-customize-language/no.png) |
| Malayca                 | SWM            | ![evet](./media/user-flow-customize-language/yes.png) | ![evet](./media/user-flow-customize-language/yes.png) |
| Norveççe Bokmal      | NB            | ![evet](./media/user-flow-customize-language/yes.png) | ![hayır](./media/user-flow-customize-language/no.png) |
| Felemenkçe                 | nl            | ![evet](./media/user-flow-customize-language/yes.png) | ![evet](./media/user-flow-customize-language/yes.png) |
| Norveççe             | hayır            | ![hayır](./media/user-flow-customize-language/no.png) | ![evet](./media/user-flow-customize-language/yes.png) |
| Pencap dili               | VARS            | ![evet](./media/user-flow-customize-language/yes.png) | ![hayır](./media/user-flow-customize-language/no.png) |
| Lehçe                | pl            | ![evet](./media/user-flow-customize-language/yes.png) | ![evet](./media/user-flow-customize-language/yes.png) |
| Portekizce - Brezilya   | pt-br         | ![evet](./media/user-flow-customize-language/yes.png) | ![evet](./media/user-flow-customize-language/yes.png) |
| Portekizce - Portekiz | pt-pt         | ![evet](./media/user-flow-customize-language/yes.png) | ![evet](./media/user-flow-customize-language/yes.png) |
| Rumence              | ro            | ![evet](./media/user-flow-customize-language/yes.png) | ![evet](./media/user-flow-customize-language/yes.png) |
| Rusça               | ru            | ![evet](./media/user-flow-customize-language/yes.png) | ![evet](./media/user-flow-customize-language/yes.png) |
| Slovakça                | sor            | ![evet](./media/user-flow-customize-language/yes.png) | ![evet](./media/user-flow-customize-language/yes.png) |
| Slovence             | SL            | ![hayır](./media/user-flow-customize-language/no.png) | ![evet](./media/user-flow-customize-language/yes.png) |
| Sırpça-Kiril    | SR-cryıl-CS    | ![hayır](./media/user-flow-customize-language/no.png) | ![evet](./media/user-flow-customize-language/yes.png) |
| Sırpça-Latin       | sr-Latn-CS    | ![hayır](./media/user-flow-customize-language/no.png) | ![evet](./media/user-flow-customize-language/yes.png) |
| İsveççe               | v            | ![evet](./media/user-flow-customize-language/yes.png) | ![evet](./media/user-flow-customize-language/yes.png) |
| Tamil dili                 | dolu            | ![evet](./media/user-flow-customize-language/yes.png) | ![hayır](./media/user-flow-customize-language/no.png) |
| Telugu dili                | ot            | ![evet](./media/user-flow-customize-language/yes.png) | ![hayır](./media/user-flow-customize-language/no.png) |
| Tayca                  | 11            | ![evet](./media/user-flow-customize-language/yes.png) | ![evet](./media/user-flow-customize-language/yes.png) |
| Türkçe               | tr            | ![evet](./media/user-flow-customize-language/yes.png) | ![evet](./media/user-flow-customize-language/yes.png) |
| Ukraynaca             | tr            | ![hayır](./media/user-flow-customize-language/no.png) | ![evet](./media/user-flow-customize-language/yes.png) |
| Vietnamca            | v            | ![hayır](./media/user-flow-customize-language/no.png) | ![evet](./media/user-flow-customize-language/yes.png) |
| Çince-Basitleştirilmiş  | zh-Hans       | ![evet](./media/user-flow-customize-language/yes.png) | ![evet](./media/user-flow-customize-language/yes.png) |
| Çince-Geleneksel | zh-Hant       | ![evet](./media/user-flow-customize-language/yes.png) | ![evet](./media/user-flow-customize-language/yes.png) |