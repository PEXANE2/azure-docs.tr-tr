---
title: Azure Active Directory B2C'de dil özelleştirme
description: Kullanıcı akışlarınızdaki dil deneyimini özelleştirme hakkında bilgi edinin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/13/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 5b3af812b2b78c276b5345b9b19226e6e1dba80b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78185769"
---
# <a name="language-customization-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'de dil özelleştirme

Azure Active Directory B2C'deki (Azure AD B2C) dil özelleştirme, kullanıcı akışının müşteri gereksinimlerinize uygun farklı dilleri barındırmasına olanak tanır. Microsoft [36 dil](#supported-languages)için çevirisağlar, ancak herhangi bir dil için kendi çevirilerinizi de sağlayabilirsiniz. Deneyiminiz yalnızca tek bir dil için sağlanmış olsa bile, sayfalardaki herhangi bir metni özelleştirebilirsiniz.

## <a name="how-language-customization-works"></a>Dil özelleştirme nasıl çalışır?

Kullanıcı akışınızın hangi dillerde kullanılabilerini seçmek için dil özelleştirmeyi kullanırsınız. Özellik etkinleştirildikten sonra, uygulamanızdan sorgu dize parametresini `ui_locales`sağlayabilirsiniz. Azure AD B2C'ye çağrı yaptığınızda, sayfanız belirttiğiniz yerel sayfaya çevrilir. Bu tür yapılandırma, kullanıcı akışınızdaki diller üzerinde tam denetim sağlar ve müşterinin tarayıcısının dil ayarlarını yok sayar.

Müşterinizin hangi dilleri gördüğü üzerinde bu düzeyde denetime ihtiyacınız olmayabilir. Bir `ui_locales` parametre sağlamazsanız, müşterinin deneyimi tarayıcıayarlarına göre belirlenir. Desteklenen bir dil olarak ekleyerek kullanıcı akışınızın hangi dillere çevrildiğini yine de denetleyebilirsiniz. Müşterinin tarayıcısı desteklemek istemediğiniz bir dili gösterecek şekilde ayarlanmışsa, desteklenen kültürlerde varsayılan olarak seçtiğiniz dil gösterilir.

* **ui-locales belirtilen dil**: Dil özelleştirmeyi etkinleştirdikten sonra, kullanıcı akışınız burada belirtilen dile çevrilir.
* **Tarayıcı tarafından istenen dil** `ui_locales` : Parametre belirtilmemişse, kullanıcı akışınız tarayıcıtarafından istenen dile çevrilir, *dil desteklenirse.*
* **İlke varsayılan dili**: Tarayıcı bir dil belirtmezse veya desteklenmeyen bir dil belirtmezse, kullanıcı akışı varsayılan dile çevrilir.

> [!NOTE]
> Özel kullanıcı öznitelikleri kullanıyorsanız, kendi çevirilerinizi sağlamanız gerekir. Daha fazla bilgi için [dizelerinizi Özelleştir'e](#customize-your-strings)bakın.

## <a name="support-requested-languages-for-ui_locales"></a>ui_locales için istenen dilleri destekleme

Dil özelleştirmesinin genel kullanılabilirliğinden önce oluşturulan ilkelerin önce bu özelliği etkinleştirmesi gerekir. Sonra oluşturulan ilkeler ve kullanıcı akışları varsayılan olarak dil özelleştirmesi etkinleştirilir.

Kullanıcı akışında dil özelleştirmeyi etkinleştirdiğinizde, parametreyi `ui_locales` ekleyerek kullanıcı akışının dilini denetleyebilirsiniz.

1. Azure AD B2C kiracınızda **Kullanıcı akışlarını**seçin.
1. Çeviriler için etkinleştirmek istediğiniz kullanıcı akışını tıklatın.
1. **Dilleri**Seçin.
1. **Dil özelleştirmeyi etkinleştir'i**seçin.

## <a name="select-which-languages-in-your-user-flow-are-enabled"></a>Kullanıcı akışınızda hangi dillerin etkin olduğunu seçme

Kullanıcı akışınız için tarayıcı tarafından parametre olmadan istendiğinde çevrilmesi `ui_locales` için bir dil kümesini etkinleştirin.

1. Kullanıcı akışınızın önceki yönergelerden dil özelleştirmeözelliğine sahip olduğundan emin olun.
1. Kullanıcı akışı için **Diller** sayfasında, desteklemek istediğiniz bir dil seçin.
1. Özellikler bölmesinde, **Etkin'den** **Evet'e**değiştirin.
1. Özellikler bölmesinin üst kısmında **Kaydet'i** seçin.

>[!NOTE]
>Bir `ui_locales` parametre sağlanmazsa, sayfa yalnızca etkinleştirildiğinde müşterinin tarayıcı diline çevrilir.
>

## <a name="customize-your-strings"></a>Dizelerinizi özelleştirin

Dil özelleştirme, kullanıcı akışınızdaki herhangi bir dizeyi özelleştirmenize olanak tanır.

1. Kullanıcı akışınızın önceki yönergelerden dil özelleştirmeözelliğine sahip olduğundan emin olun.
1. Kullanıcı akışı için **Diller** sayfasında, özelleştirmek istediğiniz dili seçin.
1. **Sayfa düzeyi kaynakları dosyaları**nın altında, yapmak istediğiniz sayfayı seçin.
1. **Varsayılanları Karşıdan Yükleme'yi** seçin (veya bu dili daha önce düzenlediyseniz **Karşıdan Yükleme** geçersiz kılar).

Bu adımlar, dizeleri düzenlemeye başlamak için kullanabileceğiniz bir JSON dosyası verir.

### <a name="change-any-string-on-the-page"></a>Sayfadaki herhangi bir dizeyi değiştirme

1. Bir JSON düzenleyicisinde önceki talimatlardan indirilen JSON dosyasını açın.
1. Değiştirmek istediğiniz öğeyi bulun. Aradığınız dizeyi bulabilir `StringId` veya değiştirmek istediğiniz özniteliği `Value` arayabilirsiniz.
1. Özniteliği, görüntülenmesini `Value` istediğiniz şeyle güncelleştirin.
1. Değiştirmek istediğiniz her dize `Override` için `true`değiştirin.
1. Dosyayı kaydedin ve değişikliklerinizi yükleyin. (Yükleme denetimini JSON dosyasını indirdiğiniz yerde bulabilirsiniz.)

> [!IMPORTANT]
> Bir dize geçersiz kılmanız gerekiyorsa, değeri `Override` 'ye `true`ayarladığınızdan emin olun Değer değiştirilmezse, giriş yoksayılır.

### <a name="change-extension-attributes"></a>Uzantı özniteliklerini değiştirme

Özel bir kullanıcı özniteliği için dize değiştirmek veya JSON'a bir tane eklemek istiyorsanız, aşağıdaki biçimdedir:

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

Özel `<ExtensionAttribute>` kullanıcı özniteliğinizin adı ile değiştirin.

Görüntülenecek yeni dizeyle değiştirin. `<ExtensionAttributeValue>`

### <a name="provide-a-list-of-values-by-using-localizedcollections"></a>Yerelleştirilmiş Koleksiyonlar'ı kullanarak değerlerin bir listesini sağlama

Yanıtlar için değerler kümesi listesi sağlamak istiyorsanız, bir `LocalizedCollections` öznitelik oluşturmanız gerekir. `LocalizedCollections`bir dizi `Name` ve `Value` çiftleridir. Öğeleriçin sıra, görüntülenen sıra olacaktır. Eklemek `LocalizedCollections`için aşağıdaki biçimi kullanın:

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

* `ElementId`bu `LocalizedCollections` öznitelik bir yanıt olduğu kullanıcı özniteliğidir.
* `Name`kullanıcıya gösterilen değerdir.
* `Value`bu seçenek seçildiğinde talepte döndürülen dir.

### <a name="upload-your-changes"></a>Değişikliklerinizi yükleyin

1. JSON dosyanızdaki değişiklikleri tamamladıktan sonra, B2C kiracınıza geri dön.
1. **Kullanıcı akışlarını** seçin ve çeviriler için etkinleştirmek istediğiniz kullanıcı akışını tıklatın.
1. **Dilleri**Seçin.
1. Çevirmek istediğiniz dili seçin.
1. Çeviri sağlamak istediğiniz sayfayı seçin.
1. Klasör simgesini seçin ve yüklemek için JSON dosyasını seçin.

Değişiklikler kullanıcı akışınıza otomatik olarak kaydedilir.

## <a name="customize-the-page-ui-by-using-language-customization"></a>Dil özelleştirmesini kullanarak sayfa kullanıcı arama sayfasını özelleştirme

HTML içeriğinizi yerelleştirmenin iki yolu vardır. Bir yolu dil [özelleştirme](user-flow-language-customization.md)açmaktır. Bu özelliği etkinleştirmek, Azure AD B2C'nin OpenID Connect parametresini `ui-locales`bitiş noktanıza iletmesine olanak tanır. İçerik sunucunuz bu parametreyi dile özgü özel html sayfaları sağlamak için kullanabilir.

Alternatif olarak, kullanılan yerel ekigöre içeriği farklı yerlerden çekebilirsiniz. CORS özellikli bitiş noktanızda, belirli diller için içeriği barındıracak bir klasör yapısı ayarlayabilirsiniz. Joker karakter değerini `{Culture:RFC5646}`kullanırsanız doğru olanı ararsınız. Örneğin, bunun özel sayfanız URI olduğunu varsayalım:

```
https://wingtiptoysb2c.blob.core.windows.net/{Culture:RFC5646}/wingtip/unified.html
```

Sayfayı ' ya `fr`yükleyebilirsiniz. Sayfa HTML ve CSS içeriğini çektiğinde aşağıdakilerden çeker:

```
https://wingtiptoysb2c.blob.core.windows.net/fr/wingtip/unified.html
```

## <a name="add-custom-languages"></a>Özel diller ekleme

Microsoft'un şu anda çeviri sağlamadığı diller de ekleyebilirsiniz. Kullanıcı akışındaki tüm dizeleri için çevirileri sağlamanız gerekir. Dil ve yerel kodlar ISO 639-1 standardındakilerle sınırlıdır.

1. Azure AD B2C kiracınızda **Kullanıcı akışlarını**seçin.
2. Özel diller eklemek istediğiniz kullanıcı akışını tıklatın ve ardından **Diller'i**tıklatın.
3. Sayfanın üst kısmından **özel dil ekle'yi** seçin.
4. Açılan bağlam bölmesinde, geçerli bir yerel kod girerek hangi dil için çeviri sağladığınızı belirleyin.
5. Her sayfa için, İngilizce için bir geçersiz kılma kümesi indirebilir ve çeviriler üzerinde çalışabilirsiniz.
6. JSON dosyaları yla işimiz bittikten sonra, bunları her sayfa için yükleyebilirsiniz.
7. **Etkinleştir'i**seçin ve kullanıcı akışınız artık bu dili kullanıcılarınız için gösterebilir.
8. Dili kaydet.

>[!IMPORTANT]
>Kaydedebilmeden önce özel dilleri etkinleştirmeniz veya bunun için geçersiz kılmaları yüklemeniz gerekir.

## <a name="additional-information"></a>Ek bilgiler

### <a name="page-ui-customization-labels-as-overrides"></a>Sayfa Arabirimi özelleştirme etiketleri geçersiz kılar

Dil özelleştirmeyi etkinleştirdiğinizde, sayfa Arabirimi özelleştirmesini kullanan etiketler için önceki deyişleriniz İngilizce (tr) için bir JSON dosyasında kalıcıhale alınır. Dil özelleştirmesine dil kaynaklarını yükleyerek etiketlerinizi ve diğer dizeleri değiştirmeye devam edebilirsiniz.

### <a name="up-to-date-translations"></a>Güncel çeviriler

Microsoft, kullanımınız için en güncel çevirileri sağlamayı taahhüt eder. Microsoft çevirileri sürekli olarak geliştirir ve sizin için uyumlu tutar. Microsoft, genel terminolojideki hataları ve değişiklikleri tanımlar ve kullanıcı akışınızda sorunsuz çalışacak güncelleştirmeler yapar.

### <a name="support-for-right-to-left-languages"></a>Sağdan sola diller için destek

Microsoft şu anda sağdan sola diller için destek sağlamamaktadır. Bunu, dizelerin görüntülenme biçimini değiştirmek için özel yerel yerel leştirilmiş ve CSS'yi kullanarak başarabilirsiniz. Bu özelliğe ihtiyacınız varsa, lütfen [Azure Geri Bildirimi'nde](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/19393000-provide-language-support-for-right-to-left-languag)bu özelliğe oy verin.

### <a name="social-identity-provider-translations"></a>Sosyal kimlik sağlayıcı çevirileri

Microsoft, `ui_locales` sosyal girişlere OIDC parametresini sağlar. Ancak Facebook ve Google da dahil olmak üzere bazı sosyal kimlik sağlayıcıları onları onurlandırmıyor.

### <a name="browser-behavior"></a>Tarayıcı davranışı

Chrome ve Firefox her ikisi de kendi set dili için istek. Desteklenen bir dilse, varsayılandan önce görüntülenir. Microsoft Edge şu anda bir dil talep etmez ve doğrudan varsayılan dile gider.

## <a name="supported-languages"></a>Desteklenen diller

Azure AD B2C, aşağıdaki diller için destek içerir. Kullanıcı akışı dilleri Azure AD B2C tarafından sağlanır. Çok faktörlü kimlik doğrulama (MFA) bildirim dilleri [Azure MFA](../active-directory/authentication/concept-mfa-howitworks.md)tarafından sağlanır.

| Dil              | Dil kodu | Kullanıcı akışları         | MFA bildirimleri  |
|-----------------------| :-----------: | :----------------: | :----------------: |
| Arapça                | Ar            | ![hayır](./media/user-flow-language-customization/no.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Bulgarca             | Bg            | ![hayır](./media/user-flow-language-customization/no.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Bangla                | Bn            | ![evet](./media/user-flow-language-customization/yes.png) | ![hayır](./media/user-flow-language-customization/no.png) |
| Katalanca               | Ca            | ![hayır](./media/user-flow-language-customization/no.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Çekçe                 | Cs            | ![evet](./media/user-flow-language-customization/yes.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Danca                | Savcı            | ![evet](./media/user-flow-language-customization/yes.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Almanca                | de            | ![evet](./media/user-flow-language-customization/yes.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Yunanca                 | El            | ![evet](./media/user-flow-language-customization/yes.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Türkçe               | tr            | ![evet](./media/user-flow-language-customization/yes.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| İspanyolca               | es            | ![evet](./media/user-flow-language-customization/yes.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Estonya Dili              | et            | ![hayır](./media/user-flow-language-customization/no.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Baskça                | Ab            | ![hayır](./media/user-flow-language-customization/no.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Fince               | ﬁ            | ![evet](./media/user-flow-language-customization/yes.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Fransızca                | Fr            | ![evet](./media/user-flow-language-customization/yes.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Galiçya Dili              | Gl            | ![hayır](./media/user-flow-language-customization/no.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Gucerat dili              | Gu            | ![evet](./media/user-flow-language-customization/yes.png) | ![hayır](./media/user-flow-language-customization/no.png) |
| İbranice                | Hge            | ![hayır](./media/user-flow-language-customization/no.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Hintçe                 | Merhaba            | ![evet](./media/user-flow-language-customization/yes.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Hırvatça              | hr            | ![evet](./media/user-flow-language-customization/yes.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Macarca             | Hu            | ![evet](./media/user-flow-language-customization/yes.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Endonezce            | id            | ![hayır](./media/user-flow-language-customization/no.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| İtalyanca               | bu            | ![evet](./media/user-flow-language-customization/yes.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Japonca              | Ja            | ![evet](./media/user-flow-language-customization/yes.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Kazakça                | kk            | ![hayır](./media/user-flow-language-customization/no.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Kannada dili               | kn            | ![evet](./media/user-flow-language-customization/yes.png) | ![hayır](./media/user-flow-language-customization/no.png) |
| Korece                | ko            | ![evet](./media/user-flow-language-customization/yes.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Litvanca            | lt            | ![hayır](./media/user-flow-language-customization/no.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Letonca               | Lv            | ![hayır](./media/user-flow-language-customization/no.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Malayalam dili             | ml            | ![evet](./media/user-flow-language-customization/yes.png) | ![hayır](./media/user-flow-language-customization/no.png) |
| Marathi               | Bay            | ![evet](./media/user-flow-language-customization/yes.png) | ![hayır](./media/user-flow-language-customization/no.png) |
| Malayca                 | Bayan            | ![evet](./media/user-flow-language-customization/yes.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Norveç Bokmal      | Nb            | ![evet](./media/user-flow-language-customization/yes.png) | ![hayır](./media/user-flow-language-customization/no.png) |
| Felemenkçe                 | Nl            | ![evet](./media/user-flow-language-customization/yes.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Norveççe             | hayır            | ![hayır](./media/user-flow-language-customization/no.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Pencap dili               | Pa            | ![evet](./media/user-flow-language-customization/yes.png) | ![hayır](./media/user-flow-language-customization/no.png) |
| Lehçe                | Pl            | ![evet](./media/user-flow-language-customization/yes.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Portekizce - Brezilya   | pt-br         | ![evet](./media/user-flow-language-customization/yes.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Portekizce - Portekiz | pt-pt         | ![evet](./media/user-flow-language-customization/yes.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Rumence              | Ro            | ![evet](./media/user-flow-language-customization/yes.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Rusça               | Ru            | ![evet](./media/user-flow-language-customization/yes.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Slovakça                | sk            | ![evet](./media/user-flow-language-customization/yes.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Slovence             | Sl            | ![hayır](./media/user-flow-language-customization/no.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Sırpça - Kiril    | sr-cryl-cs    | ![hayır](./media/user-flow-language-customization/no.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Sırpça - Latince       | sr-latn-cs    | ![hayır](./media/user-flow-language-customization/no.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| İsveççe               | Sv            | ![evet](./media/user-flow-language-customization/yes.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Tamil dili                 | ta            | ![evet](./media/user-flow-language-customization/yes.png) | ![hayır](./media/user-flow-language-customization/no.png) |
| Telugu dili                | te            | ![evet](./media/user-flow-language-customization/yes.png) | ![hayır](./media/user-flow-language-customization/no.png) |
| Tayca                  | inci            | ![evet](./media/user-flow-language-customization/yes.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Türkçe               | tr            | ![evet](./media/user-flow-language-customization/yes.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Ukraynaca             | Ingiltere            | ![hayır](./media/user-flow-language-customization/no.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Vietnamca            | Vı            | ![hayır](./media/user-flow-language-customization/no.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Çince - Basitleştirilmiş  | zh-hans       | ![evet](./media/user-flow-language-customization/yes.png) | ![evet](./media/user-flow-language-customization/yes.png) |
| Çince - Geleneksel | zh-hant       | ![evet](./media/user-flow-language-customization/yes.png) | ![evet](./media/user-flow-language-customization/yes.png) |
