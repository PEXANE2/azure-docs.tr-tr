---
title: Azure Active Directory B2C dil özelleştirmesi
description: Kullanıcı akışlarınızda dil deneyimini özelleştirme hakkında bilgi edinin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 08/13/2019
ms.custom: project-no-code
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: d378e1526d5b253853fd306c8a1b7a8982436b8d
ms.sourcegitcommit: 0820c743038459a218c40ecfb6f60d12cbf538b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87115904"
---
# <a name="language-customization-in-azure-active-directory-b2c"></a>Azure Active Directory B2C dil özelleştirmesi

Azure Active Directory B2C (Azure AD B2C) dil özelleştirmesi, Kullanıcı akışlarınızın müşteri gereksinimlerinize uyacak şekilde farklı dillere uyum sağlamasına izin verir. Microsoft, [36 dil](#supported-languages)için Çeviriler sağlar, ancak herhangi bir dil için kendi çevirilerinizi de sağlayabilirsiniz. Deneyiminiz yalnızca tek bir dil için sağlanmış olsa bile, sayfalardaki tüm metinleri özelleştirebilirsiniz.

## <a name="how-language-customization-works"></a>Dil özelleştirmesi nasıl kullanılır?

Kullanıcı akışlarınızın hangi dillerde kullanılabilir olduğunu seçmek için dil özelleştirmesi ' nı kullanırsınız. Özellik etkinleştirildikten sonra, uygulamanızdan sorgu dizesi parametresini sağlayabilirsiniz `ui_locales` . Azure AD B2C ' a çağırdığınızda, sayfanız belirttiğiniz yerel ayara çevrilir. Bu tür bir yapılandırma, Kullanıcı akışlarınızdaki diller üzerinde tüm denetimleri denetlemenizi sağlar ve müşterinin tarayıcısının dil ayarlarını yoksayar.

Müşterinizin hangi dillerde gördüğü üzerinde bu denetim düzeyine ihtiyaç duymayabilir. Bir `ui_locales` parametre sağlamazsanız, müşterinin deneyimi tarayıcının ayarları tarafından belirlenir. Desteklenen bir dil olarak ekleyerek, Kullanıcı akışlarınızın hangi dillerde çevrildiğinden de denetleyebilirsiniz. Bir müşterinin tarayıcısı, desteklemek istemediğiniz bir dili gösterecek şekilde ayarlandıysa, desteklenen kültürler için varsayılan olarak seçtiğiniz dil bunun yerine gösterilir.

* **UI-yerel ayarlar belirtilen dil**: dil özelleştirmesini etkinleştirdikten sonra, Kullanıcı akışınız burada belirtilen dile çevrilir.
* **Tarayıcı tarafından istenen dil**: herhangi bir `ui_locales` parametre belirtilmemişse, *dil destekleniyorsa*Kullanıcı akışınız tarayıcı tarafından istenen dile çevrilir.
* **İlke varsayılan dili**: tarayıcı bir dil belirtmezse veya desteklenmeyen bir değer belirtiyorsa, Kullanıcı akışı, Kullanıcı akışı varsayılan diline çevrilir.

> [!NOTE]
> Özel Kullanıcı özniteliklerini kullanıyorsanız kendi çevirilerinizi sağlamanız gerekir. Daha fazla bilgi için bkz. [dizelerinizi özelleştirme](#customize-your-strings).

## <a name="support-requested-languages-for-ui_locales"></a>Ui_locales için istenen dilleri destekleme

Dil özelleştirmenin genel kullanıma sunulmadan önce oluşturulan ilkelerin önce bu özelliği etkinleştirmesi gerekir. Sonrasında oluşturulan ilkeler ve Kullanıcı akışları, varsayılan olarak dil özelleştirmesi etkindir.

Bir Kullanıcı akışında dil özelleştirmesini etkinleştirdiğinizde, parametresini ekleyerek Kullanıcı akışının dilini kontrol edebilirsiniz `ui_locales` .

1. Azure AD B2C kiracınızda **Kullanıcı akışları**' nı seçin.
1. Çeviriler için etkinleştirmek istediğiniz kullanıcı akışına tıklayın.
1. **Dilleri**seçin.
1. **Dil özelleştirmesini etkinleştir**' i seçin.

## <a name="select-which-languages-in-your-user-flow-are-enabled"></a>Kullanıcı akışındaki hangi dillerin etkinleştirildiğini seçin

Kullanıcı akışınız için parametresi olmadan tarayıcı tarafından istendiğinde çevrilecek bir dil kümesi etkinleştirin `ui_locales` .

1. Kullanıcı akışınızdan önceki yönergelerden dil özelleştirmesi etkinleştirildiğinden emin olun.
1. Kullanıcı akışının **Diller** sayfasında, desteklemek istediğiniz bir dil seçin.
1. Özellikler bölmesinde, **etkin** ' i **Evet**olarak değiştirin.
1. Özellikler bölmesinin en üstünde **Kaydet** ' i seçin.

>[!NOTE]
>Bir `ui_locales` parametre sağlanmazsa, sayfa yalnızca etkinleştirildiğinde müşterinin tarayıcı diline çevrilir.
>

## <a name="customize-your-strings"></a>Dizelerinizi özelleştirin

Dil özelleştirmesi, Kullanıcı akışındaki herhangi bir dizeyi özelleştirmenize olanak sağlar.

1. Kullanıcı akışınız, önceki yönergelerden dil özelleştirmesi etkinleştirilmiş olduğundan emin olun.
1. Kullanıcı akışının **Diller** sayfasında, özelleştirmek istediğiniz dili seçin.
1. **Sayfa düzeyi-kaynak dosyaları**altında, düzenlemek istediğiniz sayfayı seçin.
1. **Varsayılanları indir** ' i seçin (veya daha önce bu dili düzenlediyseniz, **geçersiz kılmaları indir** ).

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

```json
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

```json
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

1. JSON dosyanızdaki değişiklikleri tamamladıktan sonra B2C kiracınıza geri dönün.
1. **Kullanıcı akışları** ' nı seçin ve çeviriler için etkinleştirmek istediğiniz kullanıcı akışına tıklayın.
1. **Dilleri**seçin.
1. Çevirmek istediğiniz dili seçin.
1. Çevirileri sağlamak istediğiniz sayfayı seçin.
1. Klasör simgesini seçin ve karşıya yüklenecek JSON dosyasını seçin.

Değişiklikler Kullanıcı akışınıza otomatik olarak kaydedilir.

## <a name="customize-the-page-ui-by-using-language-customization"></a>Dil özelleştirmesini kullanarak sayfa Kullanıcı arabirimini özelleştirme

HTML içeriğinizi yerelleştirmenin iki yolu vardır. Bir yol, [dil özelleştirmesini](user-flow-language-customization.md)açmak için bir yoldur. Bu özelliğin etkinleştirilmesi, Azure AD B2C OpenID Connect parametresini uç noktanıza iletmesine izin verir `ui-locales` . İçerik sunucunuz bu parametreyi dile özgü özelleştirilmiş HTML sayfaları sağlamak için kullanabilir.

Alternatif olarak, kullanılan yerel ayara göre farklı yerlerden içerik çekebilirsiniz. CORS etkin uç noktanıza, belirli diller için içerik barındırmak üzere bir klasör yapısı ayarlayabilirsiniz. Joker karakter değerini kullanırsanız, doğru olanı çağıracaksınız `{Culture:RFC5646}` . Örneğin, bunun özel sayfa URI 'SI olduğunu varsayalım:

```
https://wingtiptoysb2c.blob.core.windows.net/{Culture:RFC5646}/wingtip/unified.html
```

Sayfasını ' de yükleyebilirsiniz `fr` . Sayfa HTML ve CSS içeriğini çeker, buradan çekilir:

```
https://wingtiptoysb2c.blob.core.windows.net/fr/wingtip/unified.html
```

## <a name="add-custom-languages"></a>Özel Dil ekle

Ayrıca, Microsoft 'un şu anda çevirisi sağlamayan diller ekleyebilirsiniz. Kullanıcı akışındaki tüm dizeler için çevirileri sağlamanız gerekir. Dil ve yerel ayar kodları ISO 639-1 standardına göre sınırlandırılmıştır.

1. Azure AD B2C kiracınızda **Kullanıcı akışları**' nı seçin.
2. Özel diller eklemek istediğiniz kullanıcı akışına tıklayın ve sonra **Diller**' e tıklayın.
3. Sayfanın üst kısmından **Özel Dil ekle** ' yi seçin.
4. Açılan bağlam bölmesinde geçerli bir yerel ayar kodu girerek hangi dilde çeviri sağlayadığınızı belirler.
5. Her sayfa için, Ingilizce için bir geçersiz kılmalar kümesi indirebilir ve çeviriler üzerinde çalışma yapabilirsiniz.
6. JSON dosyaları ile işiniz bittiğinde, bunları her bir sayfa için karşıya yükleyebilirsiniz.
7. **Etkinleştir**' i seçtiğinizde Kullanıcı akışınız artık kullanıcılarınız için bu dili görüntüleyebilir.
8. Dili kaydedin.

>[!IMPORTANT]
>Kaydedebilmeniz için özel dilleri etkinleştirmeniz veya için geçersiz kılmaları yüklemeniz gerekir.

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

Azure AD B2C aşağıdaki diller için destek içerir. Kullanıcı akış dilleri Azure AD B2C tarafından sağlanır. Multi-Factor Authentication (MFA) bildirim dilleri [Azure MFA](../active-directory/authentication/concept-mfa-howitworks.md)tarafından sağlanır.

| Dil              | Dil kodu | Kullanıcı akışları         | MFA bildirimleri  |
|-----------------------| :-----------: | :----------------: | :----------------: |
| Arapça                | Ar            | ![hayır](./media/user-flow-language-customization/no.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Bulgarca             | bg            | ![hayır](./media/user-flow-language-customization/no.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Bangla                | milyar TL            | ![evet](./media/user-flow-language-customization/yes.png) | ![hayır](./media/user-flow-language-customization/no.png) |
| Katalanca               | yetkilisini            | ![hayır](./media/user-flow-language-customization/no.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Çekçe                 | 'ye            | ![evet](./media/user-flow-language-customization/yes.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Danca                | kapattığımda            | ![evet](./media/user-flow-language-customization/yes.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Almanca                | seçimini            | ![evet](./media/user-flow-language-customization/yes.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Yunanca                 | seri            | ![evet](./media/user-flow-language-customization/yes.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| İngilizce               | tr            | ![evet](./media/user-flow-language-customization/yes.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| İspanyolca               | es            | ![evet](./media/user-flow-language-customization/yes.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Estonya Dili              | lale            | ![hayır](./media/user-flow-language-customization/no.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Baskça                | yapılan            | ![hayır](./media/user-flow-language-customization/no.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Fince               | Fi            | ![evet](./media/user-flow-language-customization/yes.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Fransızca                | kesir            | ![evet](./media/user-flow-language-customization/yes.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Galiçya Dili              | g            | ![hayır](./media/user-flow-language-customization/no.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Gucerat dili              | çubuğu            | ![evet](./media/user-flow-language-customization/yes.png) | ![hayır](./media/user-flow-language-customization/no.png) |
| İbranice                | LIP            | ![hayır](./media/user-flow-language-customization/no.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Hintçe                 | n            | ![evet](./media/user-flow-language-customization/yes.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Hırvatça              | sa            | ![evet](./media/user-flow-language-customization/yes.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Macarca             | Hu            | ![evet](./media/user-flow-language-customization/yes.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Endonezce            | kimlik            | ![hayır](./media/user-flow-language-customization/no.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| İtalyanca               | içerdiği            | ![evet](./media/user-flow-language-customization/yes.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Japonca              | Sofya            | ![evet](./media/user-flow-language-customization/yes.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Kazakça                | kk            | ![hayır](./media/user-flow-language-customization/no.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Kannada dili               | KN            | ![evet](./media/user-flow-language-customization/yes.png) | ![hayır](./media/user-flow-language-customization/no.png) |
| Korece                | dili            | ![evet](./media/user-flow-language-customization/yes.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Litvanca            | lt            | ![hayır](./media/user-flow-language-customization/no.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Letonca               | aramasını            | ![hayır](./media/user-flow-language-customization/no.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Malayalam dili             | ml            | ![evet](./media/user-flow-language-customization/yes.png) | ![hayır](./media/user-flow-language-customization/no.png) |
| Marathi               | Mr            | ![evet](./media/user-flow-language-customization/yes.png) | ![hayır](./media/user-flow-language-customization/no.png) |
| Malayca                 | SWM            | ![evet](./media/user-flow-language-customization/yes.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Norveççe Bokmal      | NB            | ![evet](./media/user-flow-language-customization/yes.png) | ![hayır](./media/user-flow-language-customization/no.png) |
| Felemenkçe                 | nl            | ![evet](./media/user-flow-language-customization/yes.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Norveççe             | hayır            | ![hayır](./media/user-flow-language-customization/no.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Pencap dili               | VARS            | ![evet](./media/user-flow-language-customization/yes.png) | ![hayır](./media/user-flow-language-customization/no.png) |
| Lehçe                | pl            | ![evet](./media/user-flow-language-customization/yes.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Portekizce - Brezilya   | pt-br         | ![evet](./media/user-flow-language-customization/yes.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Portekizce - Portekiz | pt-pt         | ![evet](./media/user-flow-language-customization/yes.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Rumence              | ro            | ![evet](./media/user-flow-language-customization/yes.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Rusça               | ru            | ![evet](./media/user-flow-language-customization/yes.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Slovakça                | sor            | ![evet](./media/user-flow-language-customization/yes.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Slovence             | SL            | ![hayır](./media/user-flow-language-customization/no.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Sırpça (Kiril)    | SR-cryıl-CS    | ![hayır](./media/user-flow-language-customization/no.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Sırpça (Latin)       | sr-Latn-CS    | ![hayır](./media/user-flow-language-customization/no.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| İsveççe               | v            | ![evet](./media/user-flow-language-customization/yes.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Tamil dili                 | dolu            | ![evet](./media/user-flow-language-customization/yes.png) | ![hayır](./media/user-flow-language-customization/no.png) |
| Telugu dili                | ot            | ![evet](./media/user-flow-language-customization/yes.png) | ![hayır](./media/user-flow-language-customization/no.png) |
| Tayca                  | 11            | ![evet](./media/user-flow-language-customization/yes.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Türkçe               | tr            | ![evet](./media/user-flow-language-customization/yes.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Ukraynaca             | tr            | ![hayır](./media/user-flow-language-customization/no.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Vietnamca            | v            | ![hayır](./media/user-flow-language-customization/no.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Basitleştirilmiş Çince  | zh-Hans       | ![evet](./media/user-flow-language-customization/yes.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Geleneksel Çince | zh-Hant       | ![evet](./media/user-flow-language-customization/yes.png) | ![evet](./media/user-flow-language-customization/yes.png) |
